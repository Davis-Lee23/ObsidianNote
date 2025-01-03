
被SIGMOD录用
# Abstract（作者精简版）
Motivation：键值存储技术与区块链技术的简单堆叠忽略了区块链语义，使得区块链存储面临较大的读写放大问题。此外，随着以太坊网络的扩展，规模巨大的数据量进一步加剧了存储负担。目前多数研究都集中在分片、数据归档、去中心化的分布式存储等方面减轻存储层的负担，而忽略了以太坊语义与存储引擎特性之间的不兼容性。

Method：本论文创新性地提出了基于区块链数据语义的键值存储引擎设计，它负责提取区块链协议层中的区块数据语义并将它们传递到底层的键值存储引擎中。
详细的说：首先，基于以太坊区块链语义，ChainKV将不同类型的数据分别存储在KV存储器的多个存储区域中，以缓解读/写放大问题。其次，借鉴认证数据结构（ADS）中的验证过程机制，提出了一种新的ADS数据转化器，以利用ADS持久化时的数据局部性。此外，采用一种新的空间博弈缓存策略，协调两个独立存储区域该高速缓存空间管理。最后，提出了一种可选的轻量级节点崩溃恢复机制，以消除以太坊协议和存储引擎之间的功能冗余。

Result：在超过460M个区块的数据集下，实验结果表明本论文提出的方法可以在现有Ethereum系统的基础上分别提升1.99倍同步性能和4.20倍的交易处理速度。


# Introduction
+ 第一段：介绍区块链应用，公链吞吐量低，这不仅是共识算法的原因，还有底层存储引擎的性能限制（处理交易的速度），随着历史数据的积累，存储层的负担越来越严重。
+ 第二段：引出以太坊和基于LSM-tree的KV存储。随着去看的开采，性能相比早期高出数百倍。
+ 第三段：通过一些实验和对以太坊工作流程的分析，发现以太坊严重的IO开销来自两方面：①目前以太坊主要关注数据可用性，缺乏对数据的细粒度管理 ②持久化MPT节点的方法放弃了数据局部性，引入了严重的读写放大
+ 第四段：综上所述，以太坊存储负担严重的根本原因在于区块链数据语义层与底层存储引擎层之间缺乏连接和协调。介绍了三类过去的研究方向：一是Partial storage，二是Sharding，三是高性能ADS。都没有考虑到语义。
+ 第五段：提出了ChainKV，各种七七八八的新东西。
+ 第六段：实现了基于Geth的ChainKV原型
+ 贡献总结：1、3、5比较有意义。
	1. 这是首个从数据流角度弥合以太坊协议层和存储层之间的语义差距来解决I/O瓶颈的方法。
	2. 分析了以太坊的IO瓶颈来源
	3. 我们提出了一个新的存储引擎，ChainKV，通过隔离不同数据类型的管理来弥合以太坊数据层与其底层存储层之间的语义鸿沟。
	4. 我们提出了一种先进的ADS、前缀MPT和空间博弈缓存机制来探索查询过程的时空局部性。我们还提出了一个可选的轻量级崩溃恢复机制，以提高数据同步的性能。
	5. 开源了ChainKV样例


# Background
简要介绍了以太坊基础，MPT结构和功能，以太坊用的存储引擎。

### 以太坊系统
可以视作完全备份的去中心化数据库，每个节点都记录了所有交易历史。每个块都包含事务列表和前一个块的哈希。节点可以分为full nodes和light nodes。前者啥都有用来确保数据一致性，后者仅存储区块头，不具备验证能力，本文节点都是全节点。

以太坊数据级别的工作流程如下：
	首先，通过MPT检查交易发起者账户，确保交易合法。
	其次，按顺序执行所有事务，并通过更新MPT完成状态迁移
	然后，。。
	最后，。。
	此外，还介绍了两种类型的查询

### MPT Merkle Patricia Trie 梅克尔帕特里夏树
Merkle Patricia Tree（又称为Merkle Patricia Trie）是一种经过改良的、融合了默克尔树和前缀树两种树结构优点的数据结构，是以太坊中用来组织管理账户数据、生成交易集合哈希的重要数据结构。

MPT是一种数据结构，负责汇总和验证区块链数据的存在性和完整性。有三种节点。

Persistence。MPT是保存在内存的数据结构，由于存储了所有账户信息，因此要定期持久化到底层存储引擎。

查询。结合图和三种类型的节点即可

### 基于lsm树的KV存储
有三个主要的组件：the memory component, the disk component, and the write-ahead log (WAL) component

这段介绍了三个组件，也有图解，还有压缩的示例

# Motivation
本部分详细的介绍了以太坊的存储瓶颈，首先描述重放过程中巨大的IO负担，然后说出俩观察。

### 以太坊的IO瓶颈
在过去以太坊吞吐量低主要归结于两个方面，一是PoW共识定义了全局哈希率的难度，限制了区块创建速率。**二是区块打包的交易数量受矿工处理区块的速率限制**。第一个问题随着PoW变成PoS被解决了，第二个问题成为关键问题，即处理块的性能成为制约吞吐量的瓶颈。

而其中又有两步很吃IO，①验证并执行块中的事务 ②将所有更新保存到存储引擎中

①需要大量的MPT检索操作。这个操作占了处理快生命周期的70%。图3a从磁盘IO与SST请求时间两个角度来分析处理单个块的IO负担。随着开采区块的增加，MPT的规模不断扩大，存储引擎的读取性能也越来越低，这进一步加重了存储引擎的负担。将来处理一个块可能要百倍以上

②负责持久化存储更新。由于基于lsm树的KV存储还不赖，这部分开销仅占20%。但是，考虑到存储引擎的写性能与数据大小成反比，这个开销也是不能忽视的。

### 以太坊的IO特性
我们已经知道存储引擎的固有特性会放大来自上层事务的I/O需求。接下来要阐述两个观察。

#### 观察一：不同类型的区块链数据在磁盘上混合在一起。
SST文件混合存储状态数据与非状态数据，这忽略了以太坊语义，导致性能低下，原因有俩：
①是忽略数据访问模式 ②搜索空间放大。

并做了初步实验图三c和d，验证了分开的数据效率更高


#### 观察二：内存中的MPT访问显示出很强的局部性，但在磁盘上不会发生相同的情况

观察一验证了状态与非状态数据分开存储来进行查询效率会提高，但查询单个账户仍是低效的。
根本原因是MPT和存储引擎之间缺乏语义连接，因此磁盘上的MPT检索进程无法利用MPT驻留在内存中所呈现的局部性(块缓存失效)。

当访问缓存的MPT时，有两个特征。
1. 必须自顶向下访问，若当前请求的KV对时节点n，后续的数据必须是n的子节点，这具有空间局部性。
2. MPT中的高级节点，例如根节点，更可能被访问，这些高层节点表现出较高的时间局部性。然而，在MPT持续过程中，所有节点都被转换成KV对，其中所有KV对的密钥都表现出随机和离散的特征。原本从逻辑上的连续查询被离散分布在了磁盘不同位置。

总之，目前还没有机制来协调MPT的持久性和底层存储引擎的IO特征，从而将逻辑连续性从内存传输到磁盘。理想的做法是将分支内或同一级别的节点存储在磁盘上的相邻位置，以便在单个I/O操作中获取多个KV对。