# fabric-ca
`fabric-ca-jav-sdk` 를 사용하여 이것저것 테스트
관세청 네트워크 구성과 MSP 사용 (aws)



## CA Admin MSP enroll

JAVA에서 받은 certificate

| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = admin,OU = client                                       |
| Valid From          | 19 Mar 2020, 11:34 p.m.                                      |
| Valid To            | 19 Mar 2021, 11:39 p.m.                                      |
| Serial Number       | 20:C5:B8:F3:0B:B6:66:12:3E:71:B9:11:95:26:F6:9B:31:5B:16:69 (187097062835268579440872205132942300103881463401) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | E1:A7:F8:76:E1:F3:8C:E2:DC:26:A7:93:D0:A3:AC:BB:45:7B:D9:8A  |
| Fingerprint (MD5)   | 95:2F:20:2D:7C:D0:20:C1:AF:4E:84:A9:0F:F2:76:A1              |
| SANS                |                                                              |


CLI 에서 받은 certificate
| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = admin,OU = client,O = Hyperledger,ST = North Carolina,C = US |
| Valid From          | 24 Sep 2019, 9:02 a.m.                                       |
| Valid To            | 23 Sep 2020, 9:07 a.m.                                       |
| Serial Number       | 11:34:FF:5E:13:F3:3B:EB:37:EB:0D:05:39:9B:EF:79:A2:B1:F2:CC (98234727500302229101061763518841809708480852684) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | 99:D8:5A:EB:CC:F3:8D:9C:44:18:BE:32:57:05:AF:2B:C0:EE:17:10  |
| Fingerprint (MD5)   | F0:73:C4:5F:CB:27:A0:20:2C:33:CF:3E:8F:9F:AD:6B              |
| SANS                | ip-172-31-19-1.ap-northeast-2.compute.internal               |



## Client MSP enroll
- MspId : MallOrgMSP
- affiliation : mall
- type : client

| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = MallOrgMSP-api-1584661515851,OU = mall+OU = client      |
| Valid From          | 19 Mar 2020, 11:40 p.m.                                      |
| Valid To            | 19 Mar 2021, 11:45 p.m.                                      |
| Serial Number       | 30:60:DF:F3:65:E2:EC:D3:03:75:13:C6:50:D8:D6:F4:E7:57:01:E1 (276191937402411471339011273063026759043898606049) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | 47:99:3F:46:58:DD:6A:4C:C2:F0:C2:C6:89:BD:18:9A:45:86:AE:E3  |
| Fingerprint (MD5)   | F7:D8:4A:77:61:85:25:6F:55:69:D8:00:A6:9F:10:AC              |
| SANS                |                                                              |

CLI enroll
| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = Admin@mall,OU = mall+OU = client,O = Hyperledger,ST = North Carolina,C = US |
| Valid From          | 24 Sep 2019, 10:04 a.m.                                      |
| Valid To            | 23 Sep 2020, 10:09 a.m.                                      |
| Serial Number       | 22:62:89:E5:DC:1F:58:30:34:50:17:09:EA:F7:16:01:B5:48:4E:A6 (196303171837894129886979149923535279414982495910) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | 3B:44:E6:C7:5A:F3:2E:27:9D:29:A5:C7:20:36:CC:EB:C0:86:52:EC  |
| Fingerprint (MD5)   | 01:4C:1F:DF:04:AC:E1:E9:2A:00:D6:17:B1:0F:BA:C1              |
| SANS                | ip-172-31-29-13.ap-northeast-2.compute.internal              |


## version query
위에서 enroll 한 MallOrgMSP 사용
- peer0.mall.com 호출
```
success=true, transactionID=8772a59c40200bffc68b2ba890b074cb641ea749bfdda00817e380c358e9b69e, payload=1.1.1.6
```
- peer3.customs.com 호출
```
success=true, transactionID=d040ec46f2dc02d533f11a081764a31352f11757a7f8cd8e0ad8ebe90e2740d6, payload=1.1.1.6
```

policies
```yaml
Organizations:

    # SampleOrg defines an MSP using the sampleconfig. It should never be used
    # in production but may be used as a template for other definitions.
    - &SampleOrg
        # Name is the key by which this org will be referenced in channel
        # configuration transactions.
        # Name can include alphanumeric characters as well as dots and dashes.
        Name: SampleOrg

        # ID is the key by which this org's MSP definition will be referenced.
        # ID can include alphanumeric characters as well as dots and dashes.
        ID: SampleOrg

        # MSPDir is the filesystem path which contains the MSP configuration.
        MSPDir: msp

        # Policies defines the set of policies at this level of the config tree
        # For organization policies, their canonical path is usually
        #   /Channel/<Application|Orderer>/<OrgName>/<PolicyName>
        Policies: &SampleOrgPolicies
            Readers:
                Type: Signature
                Rule: "OR('SampleOrg.member')"
                # If your MSP is configured with the new NodeOUs, you might
                # want to use a more specific rule like the following:
                # Rule: "OR('SampleOrg.admin', 'SampleOrg.peer', 'SampleOrg.client')"
            Writers:
                Type: Signature
                Rule: "OR('SampleOrg.member')"
                # If your MSP is configured with the new NodeOUs, you might
                # want to use a more specific rule like the following:
                # Rule: "OR('SampleOrg.admin', 'SampleOrg.client')"
            Admins:
                Type: Signature
                Rule: "OR('SampleOrg.admin')"

Orderer: &OrdererDefaults
    # Policies defines the set of policies at this level of the config tree
    # For Orderer policies, their canonical path is
    #   /Channel/Orderer/<PolicyName>
    Policies:
        Readers:
            Type: ImplicitMeta
            Rule: "ANY Readers"
        Writers:
            Type: ImplicitMeta
            Rule: "ANY Writers"
        Admins:
            Type: ImplicitMeta
            Rule: "MAJORITY Admins"
        # BlockValidation specifies what signatures must be included in the block
        # from the orderer for the peer to validate it.
        BlockValidation:
            Type: ImplicitMeta
            Rule: "ANY Writers"
```

## eome1011 invoke
MallOrgMSP
- peer0.mall.com 호출
```
success=true, message=, transactionID=3e85d0dcc89a91f6924709f55ca22763793b6f2032f1272f79b373c5fbf31c54, payload={"arrProdInfo":[{"blckProdMtNo":"T20200320091603566566DA00009801","prodDgcnt":"1","trkNo":"20200320091603_1"}],"ordBlckHash":"b153edc84623219cf51469bb8774c639188bd3373b5152b33eb60994be587515","trkNo":"20200320091603_1"}
```

- peer3.customs.com 호출
```
success=true, message=, transactionID=ae4428cb8621041a3148c2817c332114dd2d76fce35cbc2927fd9834f443ad05, payload={"arrProdInfo":[{"blckProdMtNo":"T20200320091734368368DA00009801","prodDgcnt":"1","trkNo":"20200320091734_1"}],"ordBlckHash":"e03b25281f887354680bdc37de763b6d78dafdf0dc20e014bacfcb977cd42b02","trkNo":"20200320091734_1"}
```

getHistory
```
success=false, message=MallOrgMSP 는 getHistory 를 invoke 할 수 있는 권한이 없습니다., transactionID=579f9d4e424276aa58a4ccafecdb10902b0ba6635851a905a83badc1a5435568, payload=null)
```

CustomsOrgMSP
| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = CustomsOrgMSP-api-1584663532043,OU = customs+OU = client |
| Valid From          | 20 Mar 2020, 12:14 a.m.                                      |
| Valid To            | 20 Mar 2021, 12:19 a.m.                                      |
| Serial Number       | 23:2C:BA:ED:90:12:F3:45:DA:84:15:27:77:21:1E:06:62:4E:05:1A (200812193491181225331065543874575048702759404826) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | 44:06:41:E6:9A:85:CA:B6:75:FE:15:9E:69:0E:1B:AD:8B:78:FD:26  |
| Fingerprint (MD5)   | 60:3F:29:1C:8D:FE:FA:3F:E0:20:C2:EF:74:A0:CD:40              |
| SANS                |                                                              |

getHistory
```
success=true, message=, transactionID=5844ecb8061e9ae6ffacf6a9510b2b526e7d56a5381dd49782152f3fe3ef8cb3, payload={"results":[{"arrProdInfo ...
```

## 체인코드 install
MallOrgMSP
peer0.mall.com

```
# A Shotgun policy xx
identities:  # list roles to be used in the policy
#  admin1: {"role": {"name": "admin", "mspId": "Org1MSP"}} # admin role.
  user1: {"role": {"name": "member", "mspId": "MallOrgMSP"}} # role member in org with mspid Org1MSP

policy: # the policy  .. could have been flat but show grouping.
  1-of: # signed by one of these groups  can be <n>-of  where <n> is any digit 2-of, 3-of etc..
#  - signed-by: "user1" # a reference to one of the identities defined above.
  - signed-by: "user1"
```


결과
```
message[access denied for [install]: Failed verifying that proposal's creator satisfies local MSP principal during channelless check policy with policy [Admins]: [This identity is not an admin]
```

Admin@mall enroll

| Property            | Value                                                        |
| :------------------ | :----------------------------------------------------------- |
| Issuer              | CN = fabric-ca-server,OU = Fabric,O = Hyperledger,ST = North Carolina,C = US |
| Subject             | CN = Admin@mall,OU = mall+OU = client                        |
| Valid From          | 20 Mar 2020, 12:38 a.m.                                      |
| Valid To            | 20 Mar 2021, 12:43 a.m.                                      |
| Serial Number       | 7A:49:1C:B4:65:A7:1A:5A:38:B1:2C:EA:CB:E9:46:0E:93:26:4B:26 (698127328969952968141986682541766702612938640166) |
| CA Cert             | No                                                           |
| Key Size            | 256 bits                                                     |
| Fingerprint (SHA-1) | 79:D3:A0:52:3E:02:B7:EA:CD:A6:79:E6:CF:06:88:C9:C1:C2:98:19  |
| Fingerprint (MD5)   | 61:BC:9E:85:D6:D3:46:55:77:8C:97:48:77:2C:1C:E3              |
| SANS                |                                                              |

```
message[access denied for [install]: Failed verifying that proposal's creator satisfies local MSP principal during channelless check policy with policy [Admins]: [This identity is not an admin]]
```

peer 구동 시 사용한 MallOrgMSP 파일로 호출
```
name=[lscc], version=[], status=[SUCCESS], message[]
```



peer3.customs.com ? Pass

```
[Channel Channel{id: 6, name: } Sending proposal with transaction: 6f66c7270225ed3f69fb5d47b4b94a510bf1ccf4de9c9a7c8bcab0d78d759fdc to Peer{ id: 2, name: peer3.customs.com, channelName: mychannel, url: grpc://localhost:7051} failed because of: gRPC failure=Status{code=UNKNOWN, description=access denied: channel [] creator org [MallOrgMSP], cause=null}]
```



## 체인코드 invoke

CustomsOrgMSP, peer3.customs.com
proposal 은 success

```
Received invalid transaction event. Transaction ID caaa0664a4abe972e757d3cb7d45155857cfdb7256bda5e6ac07a8ed37fc6e82 status 10
```

MallOrgMSP, peer3.customs.com
proposal 은 success

```
Received invalid transaction event. Transaction ID f4ab7d4223078dd786c5b47425f59d1f10682022d0fc3fc1828f7e9d68131d8e status 10
```

MallOrgMSP, peer0.mall.com

```
success=true, message=, transactionID=9aebd595180a1cf61302971c1b9b32e184379f5527b8c9bb934e994d74c9ae15, payload=mspId:MallOrgMSP, certIssuer:fabric-ca-server)
```

## 체인코드 query
MallOrgMSP, peer0.mall.com
```
success=true, message=, transactionID=b75953461ab1a72434b21877ae3820e54a15ed8070e80e07c00344db0468ba1f, payload=1
```

MallOrgMSP, peer3.customs.com
```
success=true, message=, transactionID=efef4084d4cfb45c274f94d01a8884a432255c1da609c86a5e7ecd21a1198a59, payload=1
```

CustomsOrgMSP, peer3.customs.com
```
success=true, message=, transactionID=96434215bdd562efaf78c9845b1660ebb1d879ea6b186676c0948eb41efe2e14, payload=1)
```



## OrganizationalUnitIdentifiers 

peer0.mall 에 OU 를 설정, 채널생성
```
OrganizationalUnitIdentifiers:
  - Certificate: "cacerts/ca.crt"
    OrganizationalUnitIdentifier: "mall"
```

CustomOrgMSP 를 가지고 MSPID 를 "MallOrgMSP" 로 바꿔서 호출
```
access denied: channel [mychannel] creator org [MallOrgMSP]
```

CustomOrgMSP 를 가지고 MSPID 를 "CjOrgMSP" 로 바꿔서 호출
트랜잭션 성공

peer1.cj 에 OU 설정
CustomOrgMSP 를 가지고 MSPID 를 "CjOrgMSP" 로 바꿔서 호출

```
access denied: channel [mychannel] creator org [CjOrgMSP]
```



## 채널 구성원이 아닌 경우

StoreOrgMSP 를 채널에서 제외하고 호출
```
access denied: channel [mychannel] creator org [StoreOrgMSP]
```

## ACL 
정책 추가??
```yaml
Application: &ApplicationDefaults
    Organizations:

    # Policies defines the set of policies at this level of the config tree
    # For Application policies, their canonical path is
    #   /Channel/Application/<PolicyName>
    Policies: &ApplicationDefaultPolicies
        Readers:
            Type: ImplicitMeta
            Rule: "ANY Readers"
        Writers:
            Type: ImplicitMeta
            Rule: "ANY Writers"
        Admins:
            Type: ImplicitMeta
            Rule: "MAJORITY Admins"
        MyPolicy:
            Type: Signature
            Rule: "OR('MallOrg.member')"
```
peer/Propose 변경
```yaml
Application: &ApplicationDefaults
    ACLs: &ACLsDefault
            # ACL policy for invoking chaincodes on peer
            # peer/Propose: /Channel/Application/Writers
            peer/Propose: /Channel/Application/Mypolicy
```
channel profiles 변경
```yaml
ConsumerChannel:
    Consortium: SampleConsortium
    <<: *ChannelDefaults
    Application:
        <<: *ApplicationDefaults
        ACLs:
            <<: *ACLsDefault
            event/Block: /Channel/Application/Mypolicy
```
하고 다른 MSP 로 query 나 invoke 를 하면
```
Error: error endorsing query: rpc error: code = Unknown desc = failed evaluating policy on signed data during check policy [/Channel/Application/Mypolicy]: [policy /Channel/Application/Mypolicy not found] - proposal response: <nil>
```
MallOrgMSP 로 query 나 invoke 를 하면,,,??????????....,...
```

```