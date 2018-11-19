---
layout: post
title:  "比特币白皮书-中英文对照"
date:   2018-11-19 11:06:05
categories: 技术推广
tags: 比特币 白皮书
author: 大鱼
---

* content
{:toc}


![bitcoin1.png](/images/bitcoin1.png)

## 作者

作者： 中本聪

邮箱： satoshin@gmx.com

网址： www.bitcoin.org

翻译

作者：生猛老会计（杨怀玉）

链接：https://www.jianshu.com/p/126313779582

來源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。


## 摘要(Abstract)

一个完全地（使用）点对点的改进版电子现金，将支持一方直接发送给另外一方的在线支付方式，而无需通过金融机构。“数字签名”提供了部分解决方案，但如果还是需要一个被信任的第三方来防止“双重支付”，则丧失了其关键价值。我们提出的 “双重支付”问题解决方案是使用“点对点”网络。该网络使用“hashing(哈希)” 将交易打上“时间戳”，以此将所有交易合并为一个不断延展的基于哈希的“工作量证明”链条，构成（交易）记录。除非重建“工作量证明”，该记录不可能被篡改。最长的链条不仅作为被目击事件序列的证明，还（表明）该证明来自最大规模的CPU算力池。只要被节点所控制的大多数CPU算力没有协同一致攻击网络，那么他们将生成最长的链条，超越攻击者。此网络本身只需极少的基础设施。信息尽最大努力进行广播，节点可以随意离开或重新加入网络，其离线期间所发生（的所有交易），由其（补充）接收最长的“工作量证明”链条即可验证。

> A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.



## 1.导言(Introduction)

互联网商业，逐渐发展成为几乎完全借助作为被信任第三方的金融机构来处理电子支付业务。然而，该系统即使目前运转良好，足以应付大多数交易业务，但它还是受困于其固有的“基于信任模式”缺陷的问题。（互联网商业中，）“完全不可逆交易” 并非真正可行，原因是会导致金融机构不能避免地卷入争端的调解。这种调解成本导致的交易成本增加，限制了可实现交易的最小规模，同时断绝了日常小额交易的可能性，而且还有一种更为广义的成本，即为不可逆服务而设计出来的不可逆支付能力将会减弱。伴随着（交易）撤销可能性的是信任需求扩张。（由于交易可能单方面撤销，）商人们必须对他们的顾客保持戒备，由此，为了获取更多的信息不断烦扰顾客，（即使该信息量）远超他们所需。甚至无可奈何地接受一定程度上的欺诈。（现实中），人们使用物理货币时，这些成本与支付的不确定能够避免，但（在互联网商业中），没有商家会通过一种没有信任方的信息渠道进行支付。
因此，一种以建立在密码学基础上的证明来替代信任的电子支付系统，就很有必要，该系统允许任何有交易意愿的双方直接相互交易，而不需要一个被信任的第三方。在计算上不可能撤销的交易将保护卖方不被诈骗，常规的第三方中间商亦能够轻松地实现对买方的保护。这篇论文里，我们提出一种双重支付问题解决方案，就是使用一种点对点分布式时间戳服务器，以生成可计算的、按时间的前后顺序排列的交易序列证明。只要诚实的节点共同控制的CPU算力，比协同操作的攻击者节点集团更多，这个系统就是安全的。

> Commerce on the Internet has come to rely almost exclusively on financial institutions serving as trusted third parties to process electronic payments. While the system works well enough for most transactions, it still suffers from the inherent weaknesses of the trust based model. Completely non-reversible transactions are not really possible, since financial institutions cannot avoid mediating disputes. The cost of mediation increases transaction costs, limiting the minimum practical transaction size and cutting off the possibility for small casual transactions, and there is a broader cost in the loss of ability to make non-reversible payments for non-reversible services. With the possibility of reversal, the need for trust spreads. Merchants must be wary of their customers, hassling them for more information than they would otherwise need.A certain percentage of fraud is accepted as unavoidable. These costs and payment uncertainties can be avoided in person by using physical currency, but no mechanism exists to make payments over a communications channel without a trusted party.
 What is needed is an electronic payment system based on cryptographic proof instead of trust,allowing any two willing parties to transact directly with each other without the need for a trusted third party. Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. In this paper, we propose a solution to the double-spending problem using a peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.


## 2.交易(Transactions)

我们规定电子货币等同于数字签名链。每一位所有者转移其电子货币给下一位，通过数字签署一个哈希，这个哈希是前一个交易的，和下一个所有者的公钥，和加在电子货币的末端。收款人能够通过核对签名来证明该链所有权。

> We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.


![bitcoin-whitepaper-transaction1.png](/images/bitcoin-whitepaper-transaction1.png)

![bitcoin-whitepaper-transaction2.png](/images/bitcoin-whitepaper-transaction2.png)



问题当然就是收款人不能核实某个（货币）所有者没有“双重支付”该币。普通的解决方案是引入一个被信任的中心权威（机构），或铸币厂，以核对每一笔交易是否双重支付。每笔交易完成后，货币必须回收到铸币厂以发行新币，而且仅铸币厂发行的货币能够被信任不会“双重支付”。此方案的问题是整个货币系统的命运依赖于公司来运行铸币厂，导致每一笔交易不得不通过他们，就好象一家银行。我们需要一种方式，来帮助收款人知晓前任所有者没有签署任何更早的交易。为了我们的目的，最早的一笔交易是重要的，这样我们不必担心后来者试图“双重支付”。证实一笔交易是否存在的唯一方式，就是使得所有的交易都是可知的。在基于铸币厂的模式里，铸币厂是所有交易的知情者，而且清楚哪一笔最先到达。没有一个被信任方情况下，要达到这一点，交易必须公开广播，如此，则我们需要一个系统，该系统可使参与者对他们接收到（交易）顺序的单一历史达成共识。收款人需要证明每一笔交易的时点，大多数节点承认该交易为最先被接收的交易。

> The problem of course is the payee can't verify that one of the owners did not double-spend the coin. A common solution is to introduce a trusted central authority, or mint, that checks every transaction for double spending. After each transaction, the coin must be returned to the mint to issue a new coin, and only coins issued directly from the mint are trusted not to be double-spent. The problem with this solution is that the fate of the entire money system depends on the company running the mint, with every transaction having to go through them, just like a bank.
 We need a way for the payee to know that the previous owners did not sign any earlier transactions. For our purposes, the earliest transaction is the one that counts, so we don't care about later attempts to double-spend. The only way to confirm the absence of a transaction is to be aware of all transactions. In the mint based model, the mint was aware of all transactions and decided which arrived first. To accomplish this without a trusted party,transactions must be publicly announced [1], and we need a system for participants to agree on a single history of the order in which they were received. The payee needs proof that at the time of each transaction, the majority of nodes agreed it was the first received.



## 3.时间戳服务器(Timestamp Server)

我们提出的解决方案始于一种时间戳服务器。时间戳服务器通过从被时间戳记的项目区块中提取哈希并广泛地传播该哈希来运转，类似在报纸上（广告）或在全球新闻网络发邮件。该时间戳验证数据在某一时点确实存在，显然，目的是加入到哈希中。每一个时间戳都包括前一个被加入到哈希中的时间戳，形成一个链条，因为每一条追加的时间戳补充其前一个时间戳。

> The solution we propose begins with a timestamp server. A timestamp server works by taking a hash of a block of items to be timestamped and widely publishing the hash, such as in a newspaper or Usenet post [2-5]. The timestamp proves that the data must have existed at the time, obviously, in order to get into the hash. Each timestamp includes the previous timestamp in its hash, forming a chain, with each additional timestamp reinforcing the ones before it.


![bitcoin-whitepaper-timestamp1.png](/images/bitcoin-whitepaper-timestamp1.png)

## 4.工作量证明(Proof-of-Work)

要在点对点的基础上构建一个分布式时间戳服务器，我们将需要用到与亚当•贝克创造的“哈希现金”类似的“工作量证明”系统，而不是报纸或全球新闻网络邮件。在进行哈希计算时，该“工作量证明”引入对于某一个值的检索工作，比如运行SHA-256，该哈希值从某一数量的0字符开始。其（检索工作）所需的平均工作量是所需0字符数量的指数，而（检索工作结果）则能通过仅仅执行一次哈希计算来检验。对于我们的时间戳网络，我们在区块中（增补）某一随机数，（通过哈希计算，搜索）给定的区块哈希值所需的零字符串，直到找到一个值为止，以构建“工作量证明”机制。只要CPU（尽可能多的）算力被消耗在满足“工作量证明”机制上，则区块不能被修改，除非重新进行（所有）工作。由于下一区块链接其后，修改区块的工作量必须包括重建其后面的所有区块。

> To implement a distributed timestamp server on a peer-to-peer basis, we will need to use a proof-of-work system similar to Adam Back's Hashcash [6], rather than newspaper or Usenet posts.The proof-of-work involves scanning for a value that when hashed, such as with SHA-256, the hash begins with a number of zero bits. The average work required is exponential in the number of zero bits required and can be verified by executing a single hash. For our timestamp network, we implement the proof-of-work by incrementing a nonce in the block until a value is found that gives the block's hash the required zero bits. Once the CPU effort has been expended to make it satisfy the proof-of-work, the block cannot be changed without redoing the work. As later blocks are chained after it, the work to change the block would include redoing all the blocks after it.


![bitcoin-whitepaper-pow1.png](/images/bitcoin-whitepaper-pow1.png)

“工作量证明”机制也解决了代理作出大多数判断的人选确定问题。如果该“大多数”建立在“一个IP地址一张选票”的基础上，它将被破坏，因为每一个人都能够分配到很多IP。“工作量证明”实质上是“一个CPU一张选票”。最长的链条代表大多数判断，因为该链条拥有尝试投入进来的最大量“工作量证明”。如果CPU算力的大多数由诚实的节点控制，诚实的链条将以超过其他与之竞争链条的速度快速生长。为改变一个已完成区块，一个攻击者将被迫重建区块“工作量证明”，以及所有后接区块，然后追上，超过诚实节点的工作量。我们稍后将提及，由于随后区块持续增加，一个稍慢的攻击者追上的概率以指数级减少。随着时间的推移，计算机硬件速度的持续加快和运行节点的兴趣变化无常，为了应对上述情况，“工作量证明”机制的难度通过一种以设定每一小时产生区块的平均数量为目标的动态平均来调节，如果（区块）增长太快，难度将增加。

> The proof-of-work also solves the problem of determining representation in majority decision making. If the majority were based on one-IP-address-one-vote, it could be subverted by anyone able to allocate many IPs. Proof-of-work is essentially one-CPU-one-vote. The majority decision is represented by the longest chain, which has the greatest proof-of-work effort invested in it. If a majority of CPU power is controlled by honest nodes, the honest chain will grow the fastest and outpace any competing chains. To modify a past block, an attacker would have to redo the proof-of-work of the block and all blocks after it and then catch up with and surpass the work of the honest nodes. We will show later that the probability of a slower attacker catching up diminishes exponentially as subsequent blocks are added.
To compensate for increasing hardware speed and varying interest in running nodes over time, the proof-of-work difficulty is determined by a moving average targeting an average number of blocks per hour. If they're generated too fast, the difficulty increases.

## 5.网络(Network)

运行网络的步骤如下所示：
1）新的交易向所有的节点广播。
2）每一个节点将新的交易纳入一个区块。
3）每一个节点努力为自己区块寻找一个具有一定难度的“工作量证明”。
4）当一个节点找到了“工作量证明”，它向所有节点广播该区块。
5）当且仅当区块里面的所有交易有效且之前从未发生时，（其他的）节点才接受该区块。
6）节点通过追加链条中的下一区块的工作以表示其对区块的接受，使用该被接受区块的哈希作为（下一区块的）前置哈希。
节点总是把最长的链条视为正确，并将持续延长该链条。如果两个节点在同一时点广播的下一区块描述并不一致，一些节点会率先接受其中一个或另一个区块。在此情况下，他们工作于率先被他们接受的区块，但保存另外分叉以防其变得更长。当下一个“工作量证明”被找到，以及一个分叉变得更长时，这种“平局”将被打破；工作在其他分叉上的节点最后将转到这个较长的区块。
新交易的广播并非一定需要到达所有的节点。只要他们到达多数节点，他们将在短时间内被纳入一个区块。区块广播亦对缺失的信息具有容错功能。如果一个节点没有收到某个区块，它将在接收下一区块，且意识到缺失了一个区块时，提出（相应）请求。

> The steps to run the network are as follows:
1) New transactions are broadcast to all nodes.
2) Each node collects new transactions into a block.
3) Each node works on finding a difficult proof-of-work for its block.
4) When a node finds a proof-of-work, it broadcasts the block to all nodes.
5) Nodes accept the block only if all transactions in it are valid and not already spent.
6) Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.
Nodes always consider the longest chain to be the correct one and will keep working on extending it. If two nodes broadcast different versions of the next block simultaneously, some nodes may receive one or the other first. In that case, they work on the first one they received, but save the other branch in case it becomes longer. The tie will be broken when the next proof-of-work is found and one branch becomes longer; the nodes that were working on the other branch will then switch to the longer one.
New transaction broadcasts do not necessarily need to reach all nodes. As long as they reach many nodes, they will get into a block before long. Block broadcasts are also tolerant of dropped messages. If a node does not receive a block, it will request it when it receives the next block and realizes it missed one.


## 6.激励机制(Incentive)

根据规则，区块里的第一笔交易是一笔特殊交易，该交易产生一枚归属于区块创造者的新币。 这将增加节点支持网络的激励，并且提供一种方式来开始货币分配并进入流通，因此这是一种没有中心机构的发行方式。这种新货币数额持续稳定的增长类似于黄金矿工耗费资源来增加黄金流通。. 就我们来说，这类资源就是被耗费CPU时间和电力。
此类激励亦可见于交易费用。如果一笔交易的输出值低于其输入值， 其差额就是交易费，用以增加控制交易区块的激励价值。一旦一个预先设定的货币数量已经（全部）进入流通，该激励则全部转变为以交易费用（激励），并可完全地免除通货膨胀。
该激励还能有助于促使节点保持诚实。如果一个贪婪的攻击者有能力比诚实节点组织更多CPU算力，他将被迫进行选择，是通过欺诈以偷回其支付的款项（译者注：即双重支付攻击），还是通过（获取）生成的新货币。他应当会发现，按照规则行事更加有利可图，这样的规则有利于他比其他联合起来的每一个人获取更多的新货币，亦优于破坏系统以及损害自己拥有财富的有效性。

> By convention, the first transaction in a block is a special transaction that starts a new coin owned by the creator of the block. This adds an incentive for nodes to support the network, and provides a way to initially distribute coins into circulation, since there is no central authority to issue them. The steady addition of a constant of amount of new coins is analogous to gold miners expending resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.
The incentive can also be funded with transaction fees. If the output value of a transaction is less than its input value, the difference is a transaction fee that is added to the incentive value of the block containing the transaction. Once a predetermined number of coins have entered circulation, the incentive can transition entirely to transaction fees and be completely inflation free.
The incentive may help encourage nodes to stay honest. If a greedy attacker is able to assemble more CPU power than all the honest nodes, he would have to choose between using it to defraud people by stealing back his payments, or using it to generate new coins. He ought to find it more profitable to play by the rules, such rules that favour him with more new coins than everyone else combined, than to undermine the system and the validity of his own wealth.

## 7.恢复磁盘空间(Reclaiming Disk Space)

一旦一枚货币中的最后一笔交易被纳入了足够多的区块中，则能丢弃之前那些失效的交易以节省磁盘空间。为确保同时不破坏区块哈希，交易被随机散列（are hashed）在一棵 “梅克尔树（Merkle Tree）”上，仅仅将其“根节点（root）”置入该区块哈希中。只需剪除此树的其他分枝，先前的区块则能够被压缩。内置的哈希无需保存。

> Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space. To facilitate this without breaking the block's hash, transactions are hashed in a Merkle Tree [7][2][5], with only the root included in the block's hash. Old blocks can then be compacted by stubbing off branches of the tree. The interior hashes do not need to be stored.

![bitcoin-whitepaper-space1.png](/images/bitcoin-whitepaper-space1.png)

一个不含交易（信息）的区块首部大约是80 字节（bytes）。如果你希望在每10分钟左右生成一个区块，则每年约（需空间）4.2MB (80B×6×24×365=4.2MB)。鉴于计算机系统在2008年通常以随机附带2GB存贮空间出售，而且摩尔定律（Moore’s Law）预示现在的增长速度是1.2GB每年，则即使区块首部必须永久保存，存贮（空间）亦不成问题。

> A block header with no transactions would be about 80 bytes. If we suppose blocks are generated every 10 minutes, 80 bytes * 6 * 24 * 365 = 4.2MB per year. With computer systems typically selling with 2GB of RAM as of 2008, and Moore's Law predicting current growth of 1.2GB per year, storage should not be a problem even if the block headers must be kept in memory.


## 8.简化的支付验证(Simplified Payment Verification)

在不运行覆盖全网全部节点情况下，支付验证也是可能的。用户仅需保留一个最长“工作量证明”链条的区块首部备份，由此，他能够通过质询网络中节点，直到他确信拥有最长的区块链，从而达到通过“梅克尔树”分支将交易连接到被打上了时间戳的区块的目的。他无法自行检查交易，但通过连接到链条的某一位置，他能够看到一个网络节点已经接受了该笔交易，而且，在之后的追加区块，进一步证实了这个网络已经接受了该笔交易。

> It is possible to verify payments without running a full network node. A user only needs to keep a copy of the block headers of the longest proof-of-work chain, which he can get by querying network nodes until he's convinced he has the longest chain, and obtain the Merkle branch linking the transaction to the block it's timestamped in. He can't check the transaction for himself, but by linking it to a place in the chain, he can see that a network node has accepted it, and blocks added after it further confirm the network has accepted it.


![bitcoin-whitepaper-verification1.png](/images/bitcoin-whitepaper-verification1.png)

此时，只要诚实节点控制网络，则验证是可靠的，但是，如果该网络被攻击者压服，则非常容易遭受攻击。虽然网络节点能够自己验证其交易，只要该攻击者能够继续压制该网络，简易方法将被攻击者伪造的交易所愚弄。必须采取一种针对性保护策略，以接收当网络节点发现无效区块时发出的报警信号，并提示用户软件下载整个区块及被报警交易，以确认其不一致。出于更加自主的安全保障和更快的验证考虑，接受大量日常频繁支付的商业机构很可能仍然希望运行他们自己的节点。

> As such, the verification is reliable as long as honest nodes control the network, but is more vulnerable if the network is overpowered by an attacker. While network nodes can verify transactions for themselves, the simplified method can be fooled by an attacker's fabricated transactions for as long as the attacker can continue to overpower the network. One strategy to protect against this would be to accept alerts from network nodes when they detect an invalid block, prompting the user's software to download the full block and alerted transactions to confirm the inconsistency. Businesses that receive frequent payments will probably still want to run their own nodes for more independent security and quicker verification.


## 9.价值的合并和分割(Combining and Splitting Value)

尽管处理单个货币是可以的，但为了一次转让中的每一个货币而将一笔交易分割开来，将会变得很不便利。为允许价值进行分割和合并，交易包括多次输入及输出。通常地，要么是一个来自前一笔更大交易的单一输入，要么是合并了更小金额（交易）的多次输入，输出则至多只有两笔：一笔用来支付；另一笔找零（如果有的话，全部退回购买者）。

> Although it would be possible to handle coins individually, it would be unwieldy to make a separate transaction for every cent in a transfer. To allow value to be split and combined, transactions contain multiple inputs and outputs. Normally there will be either a single input from a larger previous transaction or multiple inputs combining smaller amounts, and at most two outputs: one for the payment, and one returning the change, if any, back to the sender.

![bitcoin-whitepaper-value1.png](/images/bitcoin-whitepaper-value1.png)

这时需要注意的是输出端，那里一笔交易依赖于多个交易，这些多个交易又依赖于更多的交易，这并无问题。从来不需要提取一个完全独立的交易历史备份。

> It should be noted that fan-out, where a transaction depends on several transactions, and those transactions depend on many more, is not a problem here. There is never the need to extract a complete standalone copy of a transaction's history.


## 10.隐私(Privacy)

传统的银行业务模式通过限制关联方获取数据入口和被信任的第三方来构建隐私等级。公开广播所有交易的必要性阻碍了这种方法施行，但是隐私还是可以通过在其他地方隔断信息流来保证：使用匿名持有的公钥。公众可以看见某人支付某金额给另外一人，但没有信息将此交易连接到任何人。这与股票交易所发布信息的方式类似，那里个人交易的时间和规模，即“报价”，是公开的，但不会告诉谁是交易方。

> The traditional banking model achieves a level of privacy by limiting access to information to the parties involved and the trusted third party. The necessity to announce all transactions publicly precludes this method, but privacy can still be maintained by breaking the flow of information in another place: by keeping public keys anonymous. The public can see that someone is sending an amount to someone else, but without information linking the transaction to anyone. This is similar to the level of information released by stock exchanges, where the time and size of individual trades, the "tape", is made public, but without telling who the parties were.

![bitcoin-whitepaper-privacy1.png](/images/bitcoin-whitepaper-privacy1.png)

作为一个补充防火墙，一对新的钥匙被用于每一笔交易，以阻止他们连接到一个普通的所有者。有些连接还是不可避免，原因是多次输入的交易，必然会泄露出他们的输入来自相同的所有者。真正的危险是如果所有者的私钥被泄露，连接将会泄露属于此同一所有者的其他交易。

> As an additional firewall, a new key pair should be used for each transaction to keep them from being linked to a common owner. Some linking is still unavoidable with multi-input transactions, which necessarily reveal that their inputs were owned by the same owner. The risk is that if the owner of a key is revealed, linking could reveal other transactions that belonged to the same owner.


## 11.计算( Calculations)

我们设想一个场景：一个攻击者试图比诚实链条更快地生成一个替代链条。即使其技术高超，还是无法做到突然侵入系统随心所欲地篡改，诸如无中生有地创造价值或者掠夺从未归属于他的财富等。节点将不会接受一笔无效的交易作为支付，而且，诚实的节点亦将不接受包含该无效交易的区块。一位攻击者仅能试图篡改一个他自己的交易，以收回最近消费的金钱。诚实链条和攻击者链条之间的竞赛可以描述为“二叉树随机漫步（Binomial Random Walk）”。成功事件为诚实链条延展一个区块，则增加领先，是为“+1”；失败事件则是攻击者的链条延展一个区块，则缩减差距，是为“-1”。攻击者从一笔给定的亏损迎头赶上的概率与“赌徒破产问题”类似。假设一个赌徒，拥有无限透支信用，（他）从一笔亏损开始，可以进行无限次数的测试以试图达到盈亏平衡点。我们能够计算他可能达到盈亏平衡点，或者攻击者赶上诚实链条的概率，如下所示：

p=诚实节点找到下一区块的概率

q=攻击者找到下一区块的概率

qz=攻击者在Z区块后赶上的概率

![bitcoin-whitepaper-cal1.png](/images/bitcoin-whitepaper-cal1.png)

> We consider the scenario of an attacker trying to generate an alternate chain faster than the honest chain. Even if this is accomplished, it does not throw the system open to arbitrary changes, such as creating value out of thin air or taking money that never belonged to the attacker. Nodes are not going to accept an invalid transaction as payment, and honest nodes will never accept a block containing them. An attacker can only try to change one of his own transactions to take back money he recently spent.
The race between the honest chain and an attacker chain can be characterized as a Binomial Random Walk. The success event is the honest chain being extended by one block, increasing its lead by +1, and the failure event is the attacker's chain being extended by one block, reducing the gap by -1.
The probability of an attacker catching up from a given deficit is analogous to a Gambler's Ruin problem. Suppose a gambler with unlimited credit starts at a deficit and plays potentially an infinite number of trials to try to reach breakeven. We can calculate the probability he ever reaches breakeven, or that an attacker ever catches up with the honest chain, as follows [8]:
p = probability an honest node finds the next block
q = probability the attacker finds the next block
qz = probability the attacker will ever catch up from z blocks behind


我们给定一个假设，p>q, 随着攻击者不得不赶上的区块数量不断增长，其概率呈指数级下降。由于胜算与其相悖，如果他无法幸运地在早期取得突飞猛进，则将被远远地甩在后面，他篡改（的可能性）将变得微乎其微。我们现在考虑在充分确定付款人不能修改交易之前，此笔新交易的收款人需要等待多长时间。我们假设这位付款人是攻击者，他想让收款人相信当时已经支付了，过一段时间后，将其改换为支付给他自己。事情发生时，会惊动收款人，但这个付款人希望是为时已晚。收款人生成一对新的密钥，将公钥给予付款人，并在签署前预留较短时间。如此将阻止（下述事情）发生：付款人通过连续不断地工作，提前准备区块链，直到他非常幸运地到达足够远的前方，（即其区块链长度超过诚实区块链长度），然后执行该交易。一旦该交易发送，不诚实的付款人在一条包含一项他的交易替代版本的并行的链条上秘密开始工作。收款人等到该笔交易已经纳入了一个区块，且已有 Z个区块连接在后。他并不知道攻击者已经制造增长（区块）的确切数量，但假设诚实区块在每一区块上预期耗费的时间的均等，攻击者区块潜在增长将呈“泊松分布”，期望值为：

![bitcoin-whitepaper-cal2.png](/images/bitcoin-whitepaper-cal2.png)


> Given our assumption that p > q, the probability drops exponentially as the number of blocks the attacker has to catch up with increases. With the odds against him, if he doesn't make a lucky lunge forward early on, his chances become vanishingly small as he falls further behind.
We now consider how long the recipient of a new transaction needs to wait before being sufficiently certain the sender can't change the transaction. We assume the sender is an attacker who wants to make the recipient believe he paid him for a while, then switch it to pay back to himself after some time has passed. The receiver will be alerted when that happens, but the sender hopes it will be too late.
The receiver generates a new key pair and gives the public key to the sender shortly before signing. This prevents the sender from preparing a chain of blocks ahead of time by working on it continuously until he is lucky enough to get far enough ahead, then executing the transaction at that moment. Once the transaction is sent, the dishonest sender starts working in secret on a parallel chain containing an alternate version of his transaction.
The recipient waits until the transaction has been added to a block and z blocks have been linked after it. He doesn't know the exact amount of progress the attacker has made, but assuming the honest blocks took the average expected time per block, the attacker's potential progress will be a Poisson distribution with expected value:

为了获取攻击者追赶上的概率，我们将攻击者取得进展区块数量的泊松分布的概率密度，与在该数量下攻击者依然能够追赶上的概率相乘：

![bitcoin-whitepaper-cal3.png](/images/bitcoin-whitepaper-cal3.png)


> To get the probability the attacker could still catch up now, we multiply the Poisson density for each amount of progress he could have made by the probability he could catch up from that point:


转换为以下形式，以避免无穷数列求和……

![bitcoin-whitepaper-cal4.png](/images/bitcoin-whitepaper-cal4.png)

转换为C语言代码:


```js
#include <math.h>
double AttackerSuccessProbability(double q, int z)
{
  double p = 1.0 - q;
  double lambda = z * (q / p);
  double sum = 1.0;
  int i, k;
  for (k = 0; k <= z; k++)
  {
    double poisson = exp(-lambda);
    for (i = 1; i <= k; i++)
    poisson *= lambda / i;
    sum -= poisson * (1 - pow(q / p, z - k));
  }
  return sum;
}
```

运行结果，我们能够看到概率 Z 值呈指数级下降。求解，令 P 少于0.1%时的Z值……

q=0.1

z=0 P=1.0000000

z=1 P=0.2045873

z=2 P=0.0509779

z=3 P=0.0131722

z=4 P=0.0034552

z=5 P=0.0009137

z=6 P=0.0002428

z=7 P=0.0000647

z=8 P=0.0000173

z=9 P=0.0000046

z=10 P=0.0000012

q=0.3

z=0 P=1.0000000

z=5 P=0.1773523

z=10 P=0.0416605

z=15 P=0.0101008

z=20 P=0.0024804

z=25 P=0.0006132

z=30 P=0.0001522

z=35 P=0.0000379

z=40 P=0.0000095

z=45 P=0.0000024

z=50 P=0.0000006

P < 0.001

q=0.10 z=5

q=0.15 z=8

q=0.20 z=11

q=0.25 z=15

q=0.30 z=24

q=0.35 z=41

q=0.40 z=89

q=0.45 z=340

## 12.结论(Conclusion)

我们提出一个无需借助信任的电子交易系统。我们一开始，（即介绍了）基于数字签名生成货币的普通框架，（该框架）提供强有力的所有权控制，但无法完全防止“双重支付”。 为解决此问题，我们提出一种点对点网络，（该网络）运用“工作量证明”来记录一个公共的交易历史，对攻击者来说，如果（该点对点网络的）大多数CPU算力由诚实节点控制的，则篡改（记录的行为）很快变成在计算上是不可实现的。该网络因其简单随意的拓扑结构而粗壮皮实。节点只需很少的协调即可协同工作。他们无需辨认（身份），因为（交易）信息无需路由（routed）到某些特定区域，仅需尽最大努力传播即可。节点能够随时离开和重新接入网络，其离线期间所发生所有交易，由其（补充）接收“工作量证明”链条即可验证。他们以CPU 算力投票，并以努力延展有效区块和拒绝在无效区块后延展方式来表达他们的意见，即是否接受某区块的意见。通过这种共识机制，任何必需的规则和激励都能够被强制执行。

> We have proposed a system for electronic transactions without relying on trust. We started with the usual framework of coins made from digital signatures, which provides strong control of ownership, but is incomplete without a way to prevent double-spending. To solve this, we proposed a peer-to-peer network using proof-of-work to record a public history of transactions that quickly becomes computationally impractical for an attacker to change if honest nodes control a majority of CPU power. The network is robust in its unstructured simplicity. Nodes work all at once with little coordination. They do not need to be identified, since messages are not routed to any particular place and only need to be delivered on a best effort basis. Nodes can leave and rejoin the network at will, accepting the proof-of-work chain as proof of what happened while they were gone. They vote with their CPU power, expressing their acceptance of valid blocks by working on extending them and rejecting invalid blocks by refusing to work on them. Any needed rules and incentives can be enforced with this consensus mechanism.

## 参考文献(References)

[1] W. Dai, "b-money," http://www.weidai.com/bmoney.txt, 1998.

[2] H. Massias, X.S. Avila, and J.-J. Quisquater, "Design of a secure timestamping service with minimal trust requirements," In 20th Symposium on Information Theory in the Benelux, May 1999.

[3] S. Haber, W.S. Stornetta, "How to time-stamp a digital document," In Journal of Cryptology, vol 3, no 2, pages 99-111, 1991.

[4] D. Bayer, S. Haber, W.S. Stornetta, "Improving the efficiency and reliability of digital time-stamping," In Sequences II: Methods in Communication, Security and Computer Science, pages 329-334, 1993.

[5] S. Haber, W.S. Stornetta, "Secure names for bit-strings," In Proceedings of the 4th ACM Conference on Computer and Communications Security, pages 28-35, April 1997.

[6] A. Back, "Hashcash - a denial of service counter-measure," http://www.hashcash.org/papers/hashcash.pdf, 2002.

[7] R.C. Merkle, "Protocols for public key cryptosystems," In Proc. 1980 Symposium on Security and Privacy, IEEE Computer Society, pages 122-133, April 1980.

[8] W. Feller, "An introduction to probability theory and its applications," 1957.

## 编者

如果我们关注一下白皮书的参考文献，就不难发现，其实比特币并不是凭空出现的，在她之前已经有很多探路者，也正是因为这些失败的探路者给中本聪留下了宝贵的经验，同时比特币被公认为是区块链1.0，以区块链1.0为基础，2.0,3.0……正在被创造，所以区块链正在成长之中，任何人都能够为区块链的发展舔砖加瓦。

想要了解比特币历史的朋友可以参考阅读：[比特币历史](/_posts/2018-11-19-bitcoin-history-1.md)

想要深度阅读比特币白皮书的朋友可以参考阅读：[精读比特币白皮书]()

读者也可以下载：[比特币白皮书-原稿](/docs/bitcoin.pdf)
