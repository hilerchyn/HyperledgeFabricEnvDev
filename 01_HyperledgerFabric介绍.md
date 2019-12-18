
# 介绍


一般而言，区块链是一个在分布式网络中由对等节点来维护的不可变的交易账簿。这些对等的每个节点，通过申请已经通过共识协议验证的交易来获取并维护账簿的副本，一些块（怎样的数据/交易视为一个块呢？）为一组作为交易，而这些块中的每一个块都包含一个绑定到前一个块的哈希值（这样通过哈希值串联在一起就像是一个链条，所以形象的称之为区块链block chain）。


第一个也是最广为人知的区块链应用是加密货币比特币，后来有了很多效仿它的加密货币。另一种加密货币，以太币采用了不同的方式，在集成了比特币很多相同特征的同时，为了创建一个分布式应用的平台还增加了 <b>*智能合约*</b>  功能。

随着比特币、以太币人气的提高，以及一些衍生技术的出现，更具有创新性的企业有愿意使用区块链底层技术、分布式账簿和分布式应用平台的用例也在增加。然而，许多企业用例对性能有要求，但无许可机制的区块链技术目前还达不到交付要求。另外，在很多使用案例中，标识参与者是一个硬性要求。例如在金融交易中，必须遵守“知道你的客户”和“反洗钱”的法规。


为了企业使用，我们得考虑以下几点需求：

+ 参与者必须是被标识/可标识的

+ 加入网络必须获得许可

+ 高性能的交易吞吐量

+ 低延迟的交易确认

+ 业务相关的交易和数据的隐私性和机密性


虽然早期的区块链平台正在适配企业使用，然而 Hyperledger Fabric 从一开始就是为企业使用而设计的。随后的章节描述了 Hyperledger Fabric (Fabric) 如何与其他区块链平台区分开来的，并描述了其架构决策的动机。

# Hyperledger Fabric

Hyperledger Fabric is an open source enterprise-grade permissioned distributed ledger technology (DLT) platform, designed for use in enterprise contexts, that delivers some key differentiating capabilities over other popular distributed ledger or blockchain platforms.
One key point of differentiation is that Hyperledger was established under the Linux Foundation, which itself has a long and very successful history of nurturing open source projects under open governance that grow strong sustaining communities and thriving ecosystems. Hyperledger is governed by a diverse technical steering committee, and the Hyperledger Fabric project by a diverse set of maintainers from multiple organizations. It has a development community that has grown to over 35 organizations and nearly 200 developers since its earliest commits.

Fabric has a highly modular and configurable architecture, enabling innovation, versatility and optimization for a broad range of industry use cases including banking, finance, insurance, healthcare, human resources, supply chain and even digital music delivery.
Fabric is the first distributed ledger platform to support smart contracts authored in general-purpose programming languages such as Java, Go and Node.js, rather than constrained domain-specific languages (DSL). This means that most enterprises already have the skill set needed to develop smart contracts, and no additional training to learn a new language or DSL is needed.
The Fabric platform is also permissioned, meaning that, unlike with a public permissionless network, the participants are known to each other, rather than anonymous and therefore fully untrusted. This means that while the participants may not fully trust one another (they may, for example, be competitors in the same industry), a network can be operated under a governance model that is built off of what trust does exist between participants, such as a legal agreement or framework for handling disputes.
One of the most important of the platform’s differentiators is its support for pluggable consensus protocols that enable the platform to be more effectively customized to fit particular use cases and trust models. For instance, when deployed within a single enterprise, or operated by a trusted authority, fully byzantine fault tolerant consensus might be considered unnecessary and an excessive drag on performance and throughput. In situations such as that, a crash fault-tolerant (CFT) consensus protocol might be more than adequate whereas, in a multi-party, decentralized use case, a more traditional byzantine fault tolerant (BFT) consensus protocol might be required.
Fabric can leverage consensus protocols that do not require a native cryptocurrency to incent costly mining or to fuel smart contract execution. Avoidance of a cryptocurrency reduces some significant risk/attack vectors, and absence of cryptographic mining operations means that the platform can be deployed with roughly the same operational cost as any other distributed system.
The combination of these differentiating design features makes Fabric one of the better performing platforms available today both in terms of transaction processing and transaction confirmation latency, and it enables privacy and confidentiality of transactions and the smart contracts (what Fabric calls “chaincode”) that implement them.
Let’s explore these differentiating features in more detail.
Modularity
Hyperledger Fabric has been specifically architected to have a modular architecture. Whether it is pluggable consensus, pluggable identity management protocols such as LDAP or OpenID Connect, key management protocols or cryptographic libraries, the platform has been designed at its core to be configured to meet the diversity of enterprise use case requirements.
At a high level, Fabric is comprised of the following modular components:
A pluggable ordering service establishes consensus on the order of transactions and then broadcasts blocks to peers.
A pluggable membership service provider is responsible for associating entities in the network with cryptographic identities.
An optional peer-to-peer gossip service disseminates the blocks output by ordering service to other peers.
Smart contracts (“chaincode”) run within a container environment (e.g. Docker) for isolation. They can be written in standard programming languages but do not have direct access to the ledger state.
The ledger can be configured to support a variety of DBMSs.
A pluggable endorsement and validation policy enforcement that can be independently configured per application.
There is fair agreement in the industry that there is no “one blockchain to rule them all”. Hyperledger Fabric can be configured in multiple ways to satisfy the diverse solution requirements for multiple industry use cases.
Permissioned vs Permissionless Blockchains
In a permissionless blockchain, virtually anyone can participate, and every participant is anonymous. In such a context, there can be no trust other than that the state of the blockchain, prior to a certain depth, is immutable. In order to mitigate this absence of trust, permissionless blockchains typically employ a “mined” native cryptocurrency or transaction fees to provide economic incentive to offset the extraordinary costs of participating in a form of byzantine fault tolerant consensus based on “proof of work” (PoW).
Permissioned blockchains, on the other hand, operate a blockchain amongst a set of known, identified and often vetted participants operating under a governance model that yields a certain degree of trust. A permissioned blockchain provides a way to secure the interactions among a group of entities that have a common goal but which may not fully trust each other. By relying on the identities of the participants, a permissioned blockchain can use more traditional crash fault tolerant (CFT) or byzantine fault tolerant (BFT) consensus protocols that do not require costly mining.
Additionally, in such a permissioned context, the risk of a participant intentionally introducing malicious code through a smart contract is diminished. First, the participants are known to one another and all actions, whether submitting application transactions, modifying the configuration of the network or deploying a smart contract are recorded on the blockchain following an endorsement policy that was established for the network and relevant transaction type. Rather than being completely anonymous, the guilty party can be easily identified and the incident handled in accordance with the terms of the governance model.
Smart Contracts
A smart contract, or what Fabric calls “chaincode”, functions as a trusted distributed application that gains its security/trust from the blockchain and the underlying consensus among the peers. It is the business logic of a blockchain application.
There are three key points that apply to smart contracts, especially when applied to a platform:
many smart contracts run concurrently in the network,
they may be deployed dynamically (in many cases by anyone), and
application code should be treated as untrusted, potentially even malicious.
Most existing smart-contract capable blockchain platforms follow an order-execute architecture in which the consensus protocol:
validates and orders transactions then propagates them to all peer nodes,
each peer then executes the transactions sequentially.
The order-execute architecture can be found in virtually all existing blockchain systems, ranging from public/permissionless platforms such as Ethereum (with PoW-based consensus) to permissioned platforms such as Tendermint, Chain, and Quorum.
Smart contracts executing in a blockchain that operates with the order-execute architecture must be deterministic; otherwise, consensus might never be reached. To address the non-determinism issue, many platforms require that the smart contracts be written in a non-standard, or domain-specific language (such as Solidity) so that non-deterministic operations can be eliminated. This hinders wide-spread adoption because it requires developers writing smart contracts to learn a new language and may lead to programming errors.
Further, since all transactions are executed sequentially by all nodes, performance and scale is limited. The fact that the smart contract code executes on every node in the system demands that complex measures be taken to protect the overall system from potentially malicious contracts in order to ensure resiliency of the overall system.
A New Approach
Fabric introduces a new architecture for transactions that we call execute-order-validate. It addresses the resiliency, flexibility, scalability, performance and confidentiality challenges faced by the order-execute model by separating the transaction flow into three steps:
execute a transaction and check its correctness, thereby endorsing it,
order transactions via a (pluggable) consensus protocol, and
validate transactions against an application-specific endorsement policy before committing them to the ledger
This design departs radically from the order-execute paradigm in that Fabric executes transactions before reaching final agreement on their order.
In Fabric, an application-specific endorsement policy specifies which peer nodes, or how many of them, need to vouch for the correct execution of a given smart contract. Thus, each transaction need only be executed (endorsed) by the subset of the peer nodes necessary to satisfy the transaction’s endorsement policy. This allows for parallel execution increasing overall performance and scale of the system. This first phase also eliminates any non-determinism, as inconsistent results can be filtered out before ordering.
Because we have eliminated non-determinism, Fabric is the first blockchain technology that enables use of standard programming languages. In the 1.1.0 release, smart contracts can be written in either Go or Node.js, while there are plans to support other popular languages including Java in subsequent releases.
Privacy and Confidentiality
As we have discussed, in a public, permissionless blockchain network that leverages PoW for its consensus model, transactions are executed on every node. This means that neither can there be confidentiality of the contracts themselves, nor of the transaction data that they process. Every transaction, and the code that implements it, is visible to every node in the network. In this case, we have traded confidentiality of contract and data for byzantine fault tolerant consensus delivered by PoW.
This lack of confidentiality can be problematic for many business/enterprise use cases. For example, in a network of supply-chain partners, some consumers might be given preferred rates as a means of either solidifying a relationship, or promoting additional sales. If every participant can see every contract and transaction, it becomes impossible to maintain such business relationships in a completely transparent network – everyone will want the preferred rates!
As a second example, consider the securities industry, where a trader building a position (or disposing of one) would not want her competitors to know of this, or else they will seek to get in on the game, weakening the trader’s gambit.
In order to address the lack of privacy and confidentiality for purposes of delivering on enterprise use case requirements, blockchain platforms have adopted a variety of approaches. All have their trade-offs.
Encrypting data is one approach to providing confidentiality; however, in a permissionless network leveraging PoW for its consensus, the encrypted data is sitting on every node. Given enough time and computational resource, the encryption could be broken. For many enterprise use cases, the risk that their information could become compromised is unacceptable.
Zero knowledge proofs (ZKP) are another area of research being explored to address this problem, the trade-off here being that, presently, computing a ZKP requires considerable time and computational resources. Hence, the trade-off in this case is performance for confidentiality.
In a permissioned context that can leverage alternate forms of consensus, one might explore approaches that restrict the distribution of confidential information exclusively to authorized nodes.
Hyperledger Fabric, being a permissioned platform, enables confidentiality through its channel architecture. Basically, participants on a Fabric network can establish a “channel” between the subset of participants that should be granted visibility to a particular set of transactions. Think of this as a network overlay. Thus, only those nodes that participate in a channel have access to the smart contract (chaincode) and data transacted, preserving the privacy and confidentiality of both.
To improve upon its privacy and confidentiality capabilities, Fabric has added support for private data and is working on zero knowledge proofs (ZKP) available in the future. More on this as it becomes available.
Pluggable Consensus
The ordering of transactions is delegated to a modular component for consensus that is logically decoupled from the peers that execute transactions and maintain the ledger. Specifically, the ordering service. Since consensus is modular, its implementation can be tailored to the trust assumption of a particular deployment or solution. This modular architecture allows the platform to rely on well-established toolkits for CFT (crash fault-tolerant) or BFT (byzantine fault-tolerant) ordering.
Fabric currently offers two CFT ordering service implementations. The first is based on the etcd library of the Raft protocol. The other is Kafka (which uses Zookeeper internally). For information about currently available ordering services, check out our conceptual documentation about ordering.
Note also that these are not mutually exclusive. A Fabric network can have multiple ordering services supporting different applications or application requirements.
Performance and Scalability
Performance of a blockchain platform can be affected by many variables such as transaction size, block size, network size, as well as limits of the hardware, etc. The Hyperledger community is currently developing a draft set of measures within the Performance and Scale working group, along with a corresponding implementation of a benchmarking framework called Hyperledger Caliper.
While that work continues to be developed and should be seen as a definitive measure of blockchain platform performance and scale characteristics, a team from IBM Research has published a peer reviewed paper that evaluated the architecture and performance of Hyperledger Fabric. The paper offers an in-depth discussion of the architecture of Fabric and then reports on the team’s performance evaluation of the platform using a preliminary release of Hyperledger Fabric v1.1.
The benchmarking efforts that the research team did yielded a significant number of performance improvements for the Fabric v1.1.0 release that more than doubled the overall performance of the platform from the v1.0.0 release levels.
Conclusion
Any serious evaluation of blockchain platforms should include Hyperledger Fabric in its short list.
Combined, the differentiating capabilities of Fabric make it a highly scalable system for permissioned blockchains supporting flexible trust assumptions that enable the platform to support a wide range of industry use cases ranging from government, to finance, to supply-chain logistics, to healthcare and so much more.
More importantly, Hyperledger Fabric is the most active of the (currently) ten Hyperledger projects. The community building around the platform is growing steadily, and the innovation delivered with each successive release far out-paces any of the other enterprise blockchain platforms.
Acknowledgement
The preceding is derived from the peer reviewed “Hyperledger Fabric: A Distributed Operating System for Permissioned Blockchains” - Elli Androulaki, Artem Barger, Vita Bortnikov, Christian Cachin, Konstantinos Christidis, Angelo De Caro, David Enyeart, Christopher Ferris, Gennady Laventman, Yacov Manevich, Srinivasan Muralidharan, Chet Murthy, Binh Nguyen, Manish Sethi, Gari Singh, Keith Smith, Alessandro Sorniotti, Chrysoula Stathakopoulou, Marko Vukolic, Sharon Weed Cocco, Jason Yellick