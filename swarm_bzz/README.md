# Swarm Bzz 部署

### 1. bee start --config /etc/bee/bee.yaml 启动报错 1

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

### 2. bee start --config /etc/bee/bee.yaml 卡在 waiting for chequebook deployment in transaction

使用命令行

```
nohup bee start --config /etc/bee/bee.yaml &
```

### 3. bee 的一些命令

1. 下载 bee-clef 签名工具  
   ```
   wget https://github.com/ethersphere/bee-clef/releases/download/v0.4.10/bee-clef_0.4.10_amd64.deb
   ```
2. 下载 bee
   
   ```
   wget https://github.com/ethersphere/bee/releases/download/v0.5.3/bee_0.5.3_amd64.deb
   ```
3. 安装
   ```
   sudo dpkg -I bee-clef_0.4.10_amd64.deb
   sudo dpkg -I bee_0.5.3_amd64.deb
   ```
4. 启动签名服务
   ```
   sudo bee-clef-service start
   ```
5. 启动 bee
   ```
   sudo bee start —config /etc/bee/bee.yaml
   ```
6. 检查节点连通性
   ```
   curl http://localhost:1633
   ```
7. 查看链接的节点数
   ```
   curl -s http://localhost:1635/peers | jq '.peers | length'
   ```
8. 查看网络拓扑
   ```
   curl -X GET http://localhost:1635/topology | jq
   ```
9. 查询余额
   ```
   curl localhost:1635/chequebook/balance | jq
   ```
10. 查看有没有票
    ```
    curl localhost:1635/chequebook/cheque | jq
    ```
11. 检查未兑现的支票
    ```
    wget -O cashout.sh https://gist.githubusercontent.com/ralph-pichler/3b5ccd7a5c5cd0500e6428752b37e975/raw/7ba05095e0836735f4a648aefe52c584e18e065f/cashout.sh
    chmod +x cashout.sh
    ./cashout.sh
    ```
12. 兑现支票
    ```
    ./cashout.sh cashout-all 5
    ```
13. 领 gETH 的水龙头  
    https://faucet.goerli.mudit.blog/  
14. 查看自己的钱包地址
    ```
    curl -s localhost:1635/addresses | jq .ethereum
    ```
15. 查看自己的支票合约账本地址
    ```
    curl -s http://localhost:1635/chequebook/address | jq .chequebookaddress
    ```

### 4. 查看钱包账户
   获得钱包地址
   ```
   sudo bee-get-addr
   ```
   ```
   https://goerli.etherscan.io/address/钱包地址
   ```

### 参考教程：

* 官方文档：https://docs.ethswarm.org/docs/installation/quick-start#macos-1  
* https://www.yuque.com/daxiansheng-ohldj/ilm2lv/nccrxg
* https://niutan.com/25823.html
