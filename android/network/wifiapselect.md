# wifi 망 사용 관련
## 고정 IP 설정
```java
private WifiSelector.AccessPoint createAccessPoint(String id, String serverIp) {
        int[] ip = { 172, 27 };

        for (int i = 0; i < 2; i++) {
            try {
                InetAddress inetAddress = InetAddress.getByName(serverIp);
                byte[] address = inetAddress.getAddress();
                ip[0] = address[0] & 0xFF;
                ip[1] = address[1] & 0xFF;
                break;
            } catch (UnknownHostException e) {
                e.printStackTrace();
                serverIp = Config.DEFAULT_MONITORING_SERVER_IP_1;
            }
        }

        String myIp = String.format("%d.%d.0.%d",
                ip[0], ip[1], Integer.parseInt(id.substring(2, 6)) + 10);
        String gateway = String.format("%d.%d.80.0", ip[0], ip[1]);

       return new WifiSelector.WpaKeySsid(SSID, PASSWORD, myIp, gateway);        
    }
```

## wifi ap 선택
enableNetwork API를 사용해서 특정 SSID에 접속하도록 할 수 있다.
enableNetwork 호출 시, 시간이 오래 걸리거나 연결이 안되는 현상이 발생한다(롤리팝 이상부터 발생)
```java
// accessPoint = new WifiSelector.WpaKeySsid(SSID, PASSWORD, ip, gateway);
public void select(AccessPoint accessPoint) {

        List<WifiConfiguration> configuredNetworks = wifiManager.getConfiguredNetworks();
        if (configuredNetworks == null) {
            wifiManager.setWifiEnabled(true);
            return;
        }

        WifiConfiguration wifiConfiguration = null;
        for (WifiConfiguration wifiConf : configuredNetworks) {
            Logger.d("WifiSelector", "SSID:" + wifiConf.SSID);
            if (accessPoint.getWifiConfiguration().SSID.equals(wifiConf.SSID)) {
                wifiConfiguration = wifiConf;
            }
        }

        try {
            int id;
            int prefixLength = Config.DEFAULT_NETMASK_PREFIX_LENGTH;

            if (wifiConfiguration != null) {

                wifiManager.removeNetwork(wifiConfiguration.networkId);
                wifiManager.saveConfiguration();
                Logger.d("WifiSelector", "remove:" + wifiConfiguration.networkId);
                return ;


            } else {
                wifiConfiguration = accessPoint.getWifiConfiguration();
                setAssignmentStatic(wifiConfiguration);
                setIpAddress(accessPoint.getIpAddress(),
                        prefixLength, accessPoint.getGateway(), wifiConfiguration);
                id = wifiManager.addNetwork(wifiConfiguration);
                wifiManager.saveConfiguration();
                Logger.d("WifiSelector", "new id:" + id);
            }

            enableNetwork(wifiConfiguration.SSID, mContext);


        } catch (Exception e) {
            Logger.w("WifiSelector", "", e);
        }

    }
```

## 참고
[WifiConfiguration enable network in Lollipop](http://stackoverflow.com/questions/26986023/wificonfiguration-enable-network-in-lollipop)
[Android 6.0 WifiManager enableNetwork with disableOthers=true hangs](https://code.google.com/p/android/issues/detail?id=192989)
[Android에서 연결된 Wi-fi Access Point 변경할 때 브로드캐스트](http://oldguy.tistory.com/entry/Android%EC%97%90%EC%84%9C-%EC%97%B0%EA%B2%B0%EB%90%9C-Wifi-Access-Point-%EB%B3%80%EA%B2%BD%ED%95%A0-%EB%95%8C-%EB%B8%8C%EB%A1%9C%EB%93%9C%EC%BA%90%EC%8A%A4%ED%8A%B8)
