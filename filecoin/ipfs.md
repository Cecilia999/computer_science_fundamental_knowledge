# IPFS - InterPlanetary File System 星际文件系统

### 1. what is IPFS

- The InterPlanetary File System (IPFS) is a protocol and peer-to-peer network for storing and sharing data in a distributed file system 分布式文件系统.

- Content Addressed, Versioned, P2P file system

- IPFS is like a forest of Merkle-trees(哈希树)

### 2. http challenges:

http 作为互联网传输的公认标准在大多数的应用场景已经足够的好了，但是这种 client-server distribution 会面临以下的挑战：

1. hosting and distributing petabyte datasets.
2. computing on large data across organization
3. high-volumn high-definition on-demand or real-time media streams 分发海量高清视频
4. versioning and linking of massive datasets 连接海量数据和版本管理
5. preventing accidental disappearance of important files.防止重要文件丢失

### 3. ipfs background

#### 1. DHT - distributed hash table 分布式哈希表

- KAD -> Kademlia DHT:

  - efficient lookup 查询高效
  - low coordination overhead 同步开销低
  - resistance to various attacks 抗攻击强
  - wide usage in p2p application 应用广

- Coral DSHT
- S/Kedamila DHT

kademlia 机制让 ipfs 节点之间能找到各种需要的数据

#### 2. BitTorrent - Block Exchanges

**key feature**

1.  BitTorrent's data exchange protocol uses a quasi tit-for-tat strategy: rewards nodes who contribute to each other ad punishes nodes whjo only leech others' resources. 奖励多做贡献的节点，惩罚只会索取资源的节点

    > tit-for-tat 以牙还牙以眼还眼

2.  BitTorrent peers track the availability of file pieces, prioritizing sending rarest pieces first. 追踪文件片段的有效性，优先传输稀有资源

3.  对于消耗带宽的共享策略，标准策略 tit-for-tat 比较脆弱

#### 3. git

1. git 模型中有三个对象: **files(blob)， directories(tree) and changes(commit)**
2. objects are contente-addressed, by the cryptographic hash of their contents. 基于对象内容的 hash 来寻址
3. links to other objects are embedded, forming a Merkle DAG. 对象之间的连接是嵌入式的，Merkel dag
4. most versioning metadata (branches, tags, etc) are simply pointer references and inexpensive to create and update.
5. version changes only update references or add objects. 版本变化只用更新引用或增加对象
6. distributing version changes to other users is simply transferrring objects and update remote references. 对于其他用户的分布式版本更新只需要传输文件+更新远程引用

### 4. SFS - self certified filesystems

### 5. ipfs design

> ipfs 融合了 DHT, BitTorrent, Git and SFS 等分布式系统的思想，将这些技术简化，优化，连接。

> ipfs 的所有节点完全平等，没有特殊的节点
> node vs peer:
> 都表示分布式节点，node 表示当前节点，peer 表示对等节点

**ipfs protocol 是由 7 个子协议构成的：**

#### 1. identites - manage node identity generation and verification.

> 管理节点的身份生成和身份验证

#### 2. network - manges connetcion to other peers.

> 管理节点之间的通信

#### 3. rounting - maintains information to locate specific peers and objects.

> 维护数据寻址的结构，e.g. 分布式哈希表 DHT

#### 4. exchanegs - a novel block exchange protocal (BitSwap) that governs efficient block distribution.

> 建立了块交换协议 BitSwap 来管理数据的分发，通过可交易的市场模型，建立激励机制

#### 5. objects - a Merkle DAG of contenct-addressed immutable objects with links.

> 采用 Merkle DAG 连接内容寻址的对象

#### 6. files - versioned file system hierachy inspried by git

> 借鉴了 git 的 version control 模型

#### 7. naming - a self-certifying mutable name system.

### 参考:

https://www.youtube.com/watch?v=cIJVg19RSsQ   
https://www.jianshu.com/p/97b454d81b6e
