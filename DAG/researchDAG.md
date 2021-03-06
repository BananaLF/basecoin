## 目的

以DAG为技术基础，构造区块链3.0系统。

## 名词
区块等同于交易


地址等同于账户，DAG用户可以拥有多个账户地址，类似于比特币

## 账户与交易

	一个地址就是一个账户，每个账户都有自己的交易链

### 1. 账户组织
账户组织成类以太坊世界树形式，可以根据账户地址快速定位到账户信息。
账户主要信息包括用户地址，用户Token余额

账户组织为一条链,每个账户的账本为一条链，记录该账户所有相关的交易/约记录。
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic1.png?raw=true)

### 2. 创世区块
创世区块预分配交易到指定账户
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic2.png?raw=true)

### 3. 用户交易链
一笔从a发送到b的交易划分为发送部分和接收部分，分别由a, b签名，发送到a, b各自对应的交易链上。

a发送一笔发送交易块到整个网络中，b收到发送块后，发送一笔接受交易块（表示我确认收到款）到整个网络中。如果未出现fork的情况下，所有节点更新a和b链状态。

![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic3.png?raw=true)

![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic6.png?raw=true)

### 4. 新增账户
当一笔交易发送到一个新地址的时候，就会新增一个对应账户的交易链
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic4.png?raw=true)

### 5. 签名验证
- 单一签名：

 + a发送一笔token到b, 则a对发送交易签名， b对接收交易签名，各自附加到对应交易链

- 多重签名：
 + <font color=#ff0000 size=3>待讨论</font>

### 6. 代表
账户持有人有能力选择代表为其进行投票。账户持有人有权 随时重新分配共识给任何账户。变更交易通过从旧代表中减去投票权重并将权重添加到新代表处来改变账户的代表。这笔交易中没有资金被转移，代表也没有账户 资金的消费权。代表的权重就是选择他作为代表的所有账户的金额。代表的出现是帮助账户进行验证交易的正确性。如当出现双花攻击时,代表会检测出账户链出现fork，然后代表委员会进行投票，表决出错误的fork链。
		
## 网络组织
### 1. 节点类型
* 代表节点: 记录和观察整个网络交易，裁定双花交易。

* 普通节点: 记录和观察整个网络交易。

* 轻节点:  观察它兴趣的账户

* 引导节点:提供部分或整体账本。用于向新加入网络节点提供账本数据。

### 2. 传输协议
* 交易的广播通过UDP协议

* 投票的广播通过UDP协议

* 引导节点的数据传输通过TCP协议

## 分叉与双花

### 1. 分叉/双花场景

描述：用户的交易链上产生了两个交易指向相同前继的场景，导致交易链出现分叉

原因：在于用户自身构造了双花交易
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic5.png?raw=true)

### 2. 双花处理
核心思路是通过代表投票。<font color=#ff0000>细节待讨论</font>

## 激励机制
我认为btc的挖矿是将整个系统状态的更新交给矿工，当矿工成功挖到矿后，通过交易费的形式奖励矿工，来达到激励矿工挖矿，从而整个区块链正常的运行下去。而在DAG中最新的区块池没有一个固定的状态，要达到一个稳定的状态，需要代表节点干预。


DAG的激励机制可以通过账户选择多个代表时，需要支付代表节点验证fee。为了防止代表作恶，代表节点需要进行资产抵押，普通账户可以检举代表节点作弊行为，通过全网代表委员会投票进行表决。


<font color=#ff0000>待讨论</font>
## 分层设计

![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic7.png?raw=true)

<font color=#ff0000>待讨论</font>
## 攻击防御
### 1. ddos攻击
在nano中创建一笔交易需要进行pow计算，通俗的来讲需要创建一笔交易块需要几秒的时间。在短时间内无法计算出大量的交易。
但是对于预先pow计算出大量交易，nano给出的解释是节点这种攻击会只会在短时间内出现，节点会尽快的处理交易，并不会出现问题。

### 2. 双花攻击
双花攻击一般是和ddos攻击一起使用，在一段时间内网络上出现大量的双花交易，这会导致大量的分叉，这时大部分压力都在代表节点中。因此对于代表节点来说，需要对于分叉的交易进行延迟处理（拒绝处理，拒绝处理的原因在与出现双花只能是用户主动发起双花如果出现双花，代表节点有理由拒绝处理该用户的交易）。

<font color=#ff0000>待讨论</font>
