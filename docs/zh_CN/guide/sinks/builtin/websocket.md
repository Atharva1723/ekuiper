# Websocket 目标 (Sink)

## 属性

| 属性名称         | 是否必填 | 说明                                       |
|--------------|------|------------------------------------------|
| addr         | 是    | websocket server 的地址，如: 127.0.0.1:8080   |
| path     | 是    | websocket server 的 url path，如: /api/data |
| insecureSkipVerify | 是   | 是否忽略 SSL 验证                              |
| certificationPath  | 是   | websocket 客户端 ssl 验证的 crt 文件路径           |
| privateKeyPath     | 是   | websocket 客户端 ssl 验证的 key 文件路径               |
| rootCaPath         | 是   | websocket 客户端 ssl 验证的 ca 证书文件路径              |

其他通用的 sink 属性也支持，请参阅[公共属性](../overview.md#公共属性)。

## ekuiper 作为 websocket 客户端

当 websocket sink 同时定义了 addr 和 path 后，eKuiper 将作为 websocket 客户端向远端建立 websocket 连接，并将消息通过该连接推送。

## eKuiper 作为 websocket 服务端

当 websocket sink 只定义了 path 且 addr 为空时，eKuiper 将作为 websocket 服务端等待远方建立 websocket 连接，并将消息通过该连接推送。

### 服务器配置

服务器配置在 `etc/kuiper.yaml` 中的 `source` 部分。

```yaml
source:
  ## Configurations for the global websocket server for websocket source
  # HTTP data service ip
  httpServerIp: 0.0.0.0
  # HTTP data service port
  httpServerPort: 10081
  # httpServerTls:
  #    certfile: /var/https-server.crt
  #    keyfile: /var/https-server.key
```

用户可以指定以下属性：

- `httpServerIp`：用于绑定 Websocket 数据服务器的 IP。
- `httpServerPort`：用于绑定 Websocket 数据服务器的端口。
- `httpServerTls`：Websocket 服务器 TLS 的配置。

当任何需要 Websocket 源的规则被启动时，全局服务器的设置会初始化。所有关联的规则被关闭后，它就会终止。

## /tmp/websocket.txt

```json
{
  "id": "redis",
  "sql": "SELECT * from demo_stream where temperature > 50",
  "actions": [
    {
      "log": {},
      "websocket":{
        "addr": "127.0.0.1:8080",
        "password": "/api/data"
      }
    }
  ]
}
```
