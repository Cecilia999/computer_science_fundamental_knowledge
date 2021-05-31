# install onboarding

**首先根目录创建 filecoin**

```mkdir filecoin
mkdir -p /filecoin/data/lotus
mkdir -p /filecoin/data/lotusstorage
mkdir -p /filecoin/data/lotusworker
mkdir -p /filecoin/data/filecoin-proof-parameters mkdir -p /filecoin/logs
```

**操作系统:Ubuntu 18.04**

**基本要求**

macOS or Linux installed. Windows is not yet supported.
8-core CPU and 32 GiB RAM. Models with support for Intel SHA Extensions (AMD since Zen microarchitecture, or Intel since Ice Lake) will significantly speed things up.
Enough space to store the current Lotus chain (preferably on an SSD storage medium). The chain grows at approximately 12 GiB per week. The chain can be also synced from trusted state snapshots and compacted.

**安装 Linux 库**

`sudo apt install mesa-opencl-icd ocl-icd-opencl-dev gcc git bzr jq pkg-config curl clang build-essential hwloc libhwloc-dev wget -y && sudo apt upgrade -y`

**安装 Rustup**  

`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh `

安装完成执行命令:   
`source $HOME/.cargo/env`

**安装 Go**

国外:  
`wget -c https://golang.org/dl/go1.15.5.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local`   
国内:     
`wget -c https://studygolang.com/dl/golang/go1.15.8.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local`

**安装完成后的处理:**  
`vi /etc/profile`   
在最后一行加入:    

`export PATH=$PATH:/usr/local/go/bin source /etc/profile`

**安装 Lotus**   
国内安装 Lotus 须执行下面:

```
go env -w GOPROXY=https://goproxy.cn,direct go env -w GOSUMDB=off
git config --global http.sslVerify false
```

```
export IPFS_GATEWAY=https://proof-parameters.s3.cn-south-1.jdcloud-oss.com/ipfs/ export GOPROXY=https://goproxy.cn
cd /filecoin
git clone https://github.com/filecoin-project/lotus.git
cd lotus
```

编辑文件:

`vi /etc/sudoers`   

修改为:

`Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:/usr/local/go/bin"`  

`make clean all sudo make install`

**启动 Lotus**    
启动 Lotus 前须执行下面，修改 Lotus 存储:

```
export LOTUS_PATH=/filecoin/data/lotus
export LOTUS_STORAGE_PATH=/filecoin/data/lotusstorage
export WORKER_PATH=/filecoin/data/lotusworker
export FIL_PROOFS_PARAMETER_CACHE=/filecoin/data/filecoin-proof-parameters
```

**执行 Lotus 命令:**

```
nohup lotus daemon > /filecoin/logs/lotus.log 2>&1 &
watch lotus sync status
```

**参考**    
[Lotus: install and setup](https://docs.filecoin.io/get-started/lotus/installation/)  
[lotus-miner init 一直卡在 Waiting for confirmation](https://github.com/shannon-6block/lotus-miner/issues/61)  
[Filecoin 代码编译部署常见问题 ](https://zhuanlan.zhihu.com/p/240840245)  
[如何计算 Filecoin 挖矿成本及收益?](https://news.huoxing24.com/20210106104441620102.html)  
[Filecoin 密封流程](https://www.jianshu.com/p/deb34def9ef9)   
