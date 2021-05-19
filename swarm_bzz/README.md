# Swarm Bzz 部署

### 1. bee start --config /etc/bee/bee.yaml启动报错

``` 
bee start --config /etc/bee/bee.yaml
```

得到 error

```
INFO[2021-05-19T11:45:19+08:00] could not connect to backend at https://rpc.slock.it/goerli. In a swap-enabled network a working blockchain node (for goerli network in production) is required. Check your node or specify another node using --swap-endpoint.
Error: get chain id: Post "https://rpc.slock.it/goerli": dial tcp 87.117.121.163:443: i/o timeout
```

原因：
地址失效，需要修改启动参数中的 swap-endpoint 地址

解决方法：
https://blog.csdn.net/Z1404686551/article/details/116981914
