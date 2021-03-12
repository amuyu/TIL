

# Test logging
test 할 때 score log 확인하는 방법
`IconIntegrateTestBase._make_init_config` 를 override 해서 사용하면 된다.
`Logger.load_config(conf)` 만 호출해도 되는 듯.. 근데 loglevel `info` 로 해도 debug 메시지가 출력된다.
```
from iconcommons import Logger, IconConfig

default_icon_config = {
    "log": {
        "logger": "test",
        "level": "debug",
        "filePath": "./log/icon_service.log",
        "outputType": "console|file",
        "rotate": {
            "type": "period|bytes",
            "period": "daily",
            "atTime": 1,
            "interval": 1,
            "maxBytes": 10485760,
            "backupCount": 10
        }
    }
}


def log_init():
    conf = IconConfig(str(), default_icon_config)
    conf.load()
    Logger.load_config(conf)

```