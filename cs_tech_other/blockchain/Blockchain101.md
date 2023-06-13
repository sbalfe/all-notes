# Blockchain 101 notes

- Blockchain > the underlying technology that underpins Bitcoin.

- **Decentralized Finance** (**DeFi**)

- Cryptocurrency can be defined as a digital currency that is secured by cryptography
- Progress in blockchain technology almost feels like the internet dot-com boom of the late 1990s.

---

- Also, during the **first few years** of the **2020**s, we will see more production-level usage of blockchain addressing issues such as privacy, decentralized identity, and some progress toward the decentralized web

- scalability and privacy. issues
- blockchain was a **distributed system at its core**

- system that has properties of the **both decentralized** and distributed paradigms

---

- **Distributed systems** are a **computing paradigm** whereby **two or more nodes** work with each other in a **coordinated fashion**

- A **node** can be defined as an individual player in a distributed system

-  A node that **exhibits irrational behavior** is also known as a *Byzantine node* after the **Byzantine Generals** problem.

- Byzantine **node leading to possible data inconsistency**. **L2** is a link that is broken or slow, and this can lead to a partition in the network

![img](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_03.jpg)

---

- The primary challenge of a **distributed system design** is the **coordination between nodes** and fault tolerance

- distributed system should be able to **tolerate** this and **continue to work to achieve the desired result**

- distributed system **cannot have all three of the much-desired properties simultaneously**; that is, **consistency**, **availability**, and **partition** **tolerance**

---

- invention of **Bitcoin in 2008**

- All previous attempts to **create anonymous and decentralized digital currency** were successful to some extent, but they could not solve the problem of **preventing double spending** in a completely trustless or permissionless environment

> Double Spending 
>
> *Double-spending is the risk that a digital currency can be spent twice. It is a **potential problem unique to digital currencies** because digital **information can be reproduced relatively** easily by **savvy individuals who understand the blockchain** network and the **computing power necessary to manipulate it**.*

---

> State machine replication
>
> *In computer science, state machine replication or state machine approach is a **general method for implementing a fault-tolerant** service by **replicating servers and coordinating client interactions with server replicas***

- Bitcoin solves the **SMR problem** (probabilistically) by allowing the replication of blocks

---

- **Accountability** is required to **ensure that cash** is **spendable only once**

-  The **double-spending problem** arises when the **same money can be spent twice**

- **Anonymity** is required to protect users' privacy. With **physical cash**, it is almost **impossible to trace back** spending to the individual who actually **paid the money**, which provides **adequate privacy** should the **consumer choose to hide their identity**. In the digital world, however, providing such a l**evel of privacy is difficult due to inherent personalization**, **tracing**, and **logging mechanisms** in digital payment systems such as credit card payments

![img](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_04.png)

## Blockchain

- Paper on bitcoin > It introduced the term **chain of blocks** > https://bitcoin.org/bitcoin.pdf

- significant **improvement in the performance** of **financial transactions** and **settlements** manifests as highly desirable time-and-cost reductions

> **Technical definition**: 
>
> Blockchain is a 
>
> - peer-to-peer, 
> - distributed ledger 
> - cryptographically secure, append-only, immutable (extremely hard to change), and updateable only via **consensus** or agreement among peers

- peer to peer > this means that there is no central controller in the network

- transactions to be **conducted directly among the peers** without third-party involvement, such as by a bank.

- ledger is **spread across the network** among **all peers in the network**
  - Ledge > a book or other collection of financial accounts.

- Each peer **holds a copy of the complete ledger**.

---

- "cryptographically secure," which means that **cryptography** has been used to **provide security services** that make this **ledger secure against tampering and misuse.**

---

- Append only >  data can only be added to the blockchain in ***time-sequential order***

- almost **impossible to change that data** and it can be **considered practically immutable**

- can be changed in **rare scenarios** wherein **collusion** against the **blockchain network** by bad actors succeeds in gaining more than **51 percent of the power**

---

- Updateable only via **consensus**  > power of **decentralization**.

- To achieve consensus, there are various **consensus facilitation algorithms** that ensure **all parties agree on the final state of the data** on the **blockchain network** and **resolutely agree upon it to be true**.

### Blockchain architecture

#### Blockchain Layers

- Layer of a **distributed peer-to-peer** network running on **top of the internet**

- It is analogous to **SMTP**, **HTTP**, or **FTP** running on top of **TCP**/**IP**:

<img src="https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_05.jpg" alt="img" style="zoom:67%;" />

##### Network

- Usually the **internet** and provides a **base communication layer** for any **blockchain**

##### P2P

- Runs on top of the **Network** layer, which consists of information **propagation protocols** such as **gossip** or **flooding protocols**.

##### Cryptography

- These cryptographic protocols play a vital role in the **integrity of blockchain processes**, secure information dissemination, and **blockchain consensus mechanisms**

- **Public key cryptography** and relevant **components** such as **digital signatures** and **cryptographic hash functions**.

##### Consensus

- Usage of various **consensus mechanisms** to ensure **agreement among different participants** of the **blockchain**.

- Various techniques such as **SMR**, **proof-based consensus mechanisms**, or traditional (from **traditional distributed systems research**) **Byzantine fault-tolerant consensus protocols.**

##### Execution

- Consist of **virtual machines**, **blocks**, **transaction**, and **smart contracts**

- **Executions services on the blockchain** and performs operations such as **value transfer**, **smart contract execution,** and **block generation**.

- Virtual machines such as **Ethereum Virtual Machine** (**EVM**) provide an **execution environment** for **smart contracts** to execute.

##### Applications

- Composed of **smart contracts**, **decentralized applications**, **DAOs**, and **autonomous agents**.

> DAO - A **decentralized autonomous organization**, sometimes called a decentralized autonomous corporation, is an organization represented by rules **encoded as a computer program** that is transparent, controlled by the **organization members** and **not influenced by a central government.**

### Blockchain For Business

- Blockchain can be defined as a platform where **peers can exchange value/e-cash** using **transactions without the need for a centrally trusted** arbitrator
- In **financial trading**, a central clearing house acts as a **trusted third party between two or more trading parties** > bank transfers etc.
- This disintermediation allows **blockchain** to be a **decentralized consensus mechanism** where **no single authority** is in charge of the **database**

- Benefit of **decentralization here**, because if **no banks** or **central clearing houses** are required, then it **immediately leads to cost savings**, **faster transaction speeds,** and more **trust**.

### Generic Elements of Blockchain

![img](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_06.jpg)

> **A Genesis Block is the **name given to the first block a cryptocurrency, such as Bitcoin, ever mined**. ... The layers and deep history of each sequence is one of the things that makes a blockchain-based cryptocurrency so secure.**

##### Address

- Addresses are **unique identifiers** used in a **blockchain transaction** to denote **senders and recipients.** 
- An address is usually a **public key** or **derived from a public key**.

##### Transaction

- A transaction is the **fundamental unit of a blockchain.** A transaction represents a **transfer of value from one address to another**.

##### Blocko

- A block is composed of **multiple transactions** and **other elements**, such as the **previous block hash** (hash pointer), **timestamp**, and **nonce.** 
- A block is composed of a **block header** and a selection of **transactions bundled together and organized logically**

###### Properties of a Block

1. Reference to previous block
   - This reference is the **hash of the header** of the **previous block**
   - A **genesis block** is the first block in the **blockchain** that is **hardcoded** at the time the **blockchain was first started**.
   - The structure of a **block is also dependent** on the **type and design of a blockchain**

2. Nonce 
   - A **number** generated for single use
   - A nonce is **used extensively** in many **cryptographic operations** to provide **replay protection**, **authentication**, and **encryption**
   - In blockchain, it's used in **PoW consensus algorithms** and for **transaction replay protection**

3. Timestamp

   - creation time of the block

4. **Merkle root**

   https://www.geeksforgeeks.org/introduction-to-merkle-tree/

   

   <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/MerkleTree-min-1024x512.png" alt="img" style="zoom: 80%;" />

   

   > This is a **binary merkel tree**, the top hash is a hash of the entire tree. 
   >  
   >
   > - This structure of the tree allows efficient mapping of huge data and small changes made to the data can be easily identified.
   > - If we want to know where data change has occurred then we can check if data is consistent with root hash and we will not have to traverse the whole structure but only a small part of the structure.
   > - The root hash is used as the fingerprint for the entire data.

   - hash of all of the nodes of a Merkle tree
   - In a **blockchain block**, it is the **combined hash of the transactions in the block**
   - Merkle trees are commonly used to **allow efficient verification of transactions**.

   - Merkle root in a **blockchain** is present in the **block header section of a block**, which is the **hash of all transactions in a block**.
   - Verifying only the **Merkle root** is required to **verify all transactions present**.

5. Transactions also container in the block body.

   - A **transaction** is a **record of an event**, for example, the **event of transferring cash** from a **sender's account to a beneficiary's account**
   - Transactions and its **size varies depending** on the **type** and **design** of the **blockchain**

---

<img src="https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_07.jpg" alt="img" style="zoom:67%;" />

- Not all contained always.

##### Peer to Peer

- A peer-to-peer network is a **network topology** wherein all **peers can communicate** with each other and **send and receive messages**.

##### Scripting / Programming language

-  *Scripts* or ***programs* perform various operations** on a **transaction** in order to **facilitate various functions**
- Bitcoin, **transaction scripts** are **predefined in a language** called **Script**
- Script is a **limited language**, in the sense that it only **allows essential operations** that are **necessary for executing transactions**, but it does **not allow for arbitrary program development**.

- Python > executes compiled C code to function basically. 

---

- **Not Turing complete** > " In simple words, a Turing complete language means that it can perform any computation.
  - Bitcoin script does not support loops and branching unlike **Ethereum's solidity**

---

##### Virtual Machine

- This is an **extension** of the **transaction script** introduced previously
- A *virtual machine* allows **Turing complete code** to be **run on a blockchain** (as **smart contracts**)
- Transaction script is **limited in its operation**
- Various blockchains use virtual machines to run programs such as **Ethereum Virtual Machine** (**EVM**) and **Chain Virtual Machine** (**CVM**). 
- EVM is used in the **Ethereum blockchain**, while **CVM** is a virtual machine **developed for and used in an enterprise-grade blockchain** called **"Chain Core."**

##### State machine

- A blockchain can be viewed as a **state transition mechanism** whereby a **state is modified** from its **initial form** to the **next one by nodes** on the **blockchain network** as a result of t**ransaction execution**

##### Smart Contracts

- These programs **run on top of the blockchain** and **encapsulate the business logic** to be **executed when certain conditions** are met.
-  These programs are **enforceable and automatically executable**. 
- The *smart contract* feature is **not available** on **all blockchain platforms**, but it is now becoming a **very desirable feature** due to the **flexibility and power** that it **provides to blockchain applications**

##### Node

- A *node* in a **blockchain network** performs various **functions** depending on the **role that it takes on**
- A node can **propose** and **validate transactions** and **perform mining** to **facilitate consensus** and **secure the blockchain**
-  This goal is achieved by following a **consensus protocol** (most **commonly PoW**)
- Nodes can also perform **other functions** such as **simple payment verification** (lightweight nodes), **validation**, etc.
- Nodes also **perform a transaction signing function**. Transactions are first **created by nodes** and then also **digitally signed** by nodes using **private keys as proof** that they are the **legitimate owner** of the **asset that they wish to transfer** to **someone else on the blockchain** network.

- This asset is usually a **token or virtual currency**, such as **Bitcoin**, but it can also **be any real-world asset** represented on the **blockchain by using tokens**.

---

![img](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_08.png)

- Four node blockchain network

  - Each maintains:
    1. **Chain of blocks**
    2. **Virtual machine**
    3. **State machine**
    4. **Address**

- Further magnify a specific one 

- Magnify again to show the block itself.

  

### How blockchain Works

- Nodes are either ***miners*** who **create new blocks** and **mint cryptocurrency** (coins) or ***block signers*** who **validate** and **digitally sign the transactions**
- A **critical decision** that every blockchain network has to make is to figure out **which node will append the next block to the blockchain**
  - This decision is made using a ***consensus mechanism***. 
    - *The consensus mechanism will be described later in this chapter. For now, we will look at how a blockchain validates transactions and creates and adds blocks to grow the blockchain.*

#### General Scheme of Block Creation

##### Initiate Transaction

- A node starts a **transaction** by first **creating** it and then **digitally signing** it with **its private key**
  - A transaction can **represent various actions in a blockchain**. Most commonly, this is a **data structure** that represents the **transfer of value** between **users on the blockchain network.**
  - Data structure consists of :
    1.  **logic of transfer of value**
    2. **relevant rules**
    3. **source** / **destination** address
    4. Other **validation information** 

- **Transactions** are usually either a **cryptocurrency transfer** (transfer of value) or **smart contract invocation** that can perform any **desired operation**. 
- A **transaction** occurs **between two or more parties.** 

##### Transaction is validated and broadcast

- Transaction is **propogated** $\to$​ *broadcast*
- By using **data-dissemination protocols**, such as ***Gossip protocol***, to other **peers** that **validate the transaction** based on preset **validity criteria**.

##### Find New Block

- When the transaction is **received and validated** by special participants called **miners** on the **blockchain network**, it is **included in a block**, and the **process of mining starts.**

- Here, nodes called **miners race** to **finalize the block** they've created by a **process known as mining.**

##### New block found

- Once a miner **solves a mathematical puzzle** (or fulfils the **requirements** of the **consensus mechanism** implemented in a **blockchain**), the block is considered **"found" and finalized**. At this point, **the transaction is considered confirmed.**

- Usually, in **cryptocurrency blockchains** such as **Bitcoin**, the miner who s**olves the mathematical puzzle** is also **rewarded** with a **certain number of coins** as an **incentive for their effort** and the **resources they spent in the mining process**.

##### Add new block to the blockchain

- Newly created **block is validated**.
- **Transactions** or **smart** **contracts** within it are **executed**, and it is **propagated to other peers**.

- **Peers** also **validate** and **execute the block**. It now becomes **part of the blockchain** (ledger), and the **next block links itself cryptographically** back to **this block**. This **link is called a hash pointer**.

![img](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781839213199/files/Images/B15726_01_09.jpg)



### Benefits, features, and limitations of blockchain

#### Decentralization

- This is a **core concept** and **benefit** of blockchain. 
- There is **no need** for a **trusted third party** or **intermediary to validate transactions**;
-  instead, a **consensus mechanism** is used to **agree** on the **validity** of transactions.

#### Transparency and Trust

- As **blockchains** are **shared** and **everyone can see** what is on the **blockchain**, this allows the **system to be transparent**. 
- As a result, **trust is established**. 
- This is more relevant in scenarios such as the **disbursement of funds** or **benefits** where **personal discretion** in **relation** to **selecting beneficiaries** needs to be **restricted**.

#### Immutability

- Once the data has been **written to the blockchain**, it is **extremely difficult** to **change** it back. 
- It is **not genuinely immutable**, but because **changing data** is so **challenging** and **nearly impossible**, this is seen as a **benefit to maintaining** an **immutable ledger of transactions**.

#### High availability

- As the system is **based on thousands of nodes** in a **peer-to-peer network**, and the data is **replicated** and **updated on every node**, the **system becomes highly available**.
-  Even if **some nodes** leave the network or **become inaccessible**, the **network** as a **whole** **continues** to work, thus **making it highly available**. 
- This **redundancy results in high availability.**

#### Highly Secure

-  All **transactions** on a **blockchain** are **cryptographically** **secured** and thus **provide network integrity.** 

- Any **transactions** **posted** from the **nodes** on the blockchain are **verified** based on a **predetermined** **set of rules**.

-  Only **valid transactions** are **selected** for **inclusion** in a **block**. The blockchain is based on **proven cryptographic technology** that ensures the **integrity and availability of data**. 

- Generally, **confidentiality** is **not** **provided** due to the **requirements of transparency**. 

- This limitation is the **leading** **barrier** to its **adoption** by **financial** **institutions** and other industries that **require** the **privacy** and **confidentiality** of **transactions**.

-  As such, the **privacy** and **confidentiality** of **transactions** on the blockchain are **being researched** very actively, and **advancements** are already **being made.**

-  It could be argued that, in many situations, **confidentiality** is **not** **needed** and **transparency** is **preferred**. 

  > For example, with Bitcoin, **confidentiality is not an absolute requirement**; however, it is desirable in some scenarios. A more recent example is Zcash ([https://z.cash](https://z.cash/)), which provides a platform for conducting anonymous transactions. Other security services, such as non-repudiation and authentication, are also provided by blockchain, as all actions are secured using private keys and digital signatures.

#### Simplification of Current paradigms

- The current **blockchain model** in **many industries**, such as **finance** or **health**, is somewhat **disorganized**. 
- In this model, **multiple entities** maintain their **own databases** and **data sharing** can become very **difficult** due to the **disparate** **nature** of the **systems**. 
- However, as a **blockchain** can serve as a **single shared ledger** among many **interested** **parties**, this can result in **simplifying the model** by **reducing** the **complexity** of **managing** the s**eparate systems** maintained by each entity.

#### Faster dealings

- In the financial industry, especially in **post-trade settlement functions**, blockchain can play a **vital role** by **enabling** the **quick settlement** of trades.
-  **Blockchain** does **not require** a **lengthy process of verification**, **reconciliation**, and **clearance** because a **single** **version** of **agreed**-**upon** **data** is already **available** on a **shared ledger** between financial organizations.

#### Cost saving

- As **no trusted third party** or **clearing house** is required in the **blockchain model**, this can massively **eliminate overhead costs** in the **form of the fees**, which are **paid to such parties**.

#### Platform for smart contracts

- A **blockchain** is a platform on which **programs** can **run** that **execute business** **logic** on behalf of the users. 
- This is a very useful feature but **not all blockchains** have a mechanism to execute ***smart contracts***; however, this is a very **desirable** feature. 
- It is available on **newer blockchain platforms** such as **Ethereum** and **MultiChain**, but not on Bitcoin.

> Blockchain technology provides a **platform for running smart contracts**. These are **automated, autonomous programs that reside** on the **blockchain network** and encapsulate the **business logic** and **code needed to execute a required function** when certain **conditions are met**. For example, think about an **insurance contract** where a claim is paid to the traveller if the flight is cancelled. In the real world, this process normally takes a significant amount of time to make the claim, verify it, and pay the insurance amount to the claimant (traveller). **What if this whole process were automated with cryptographically-enforced trust, transparency, and execution so that as soon as the smart contract received a feed that the flight in question has been cancelled, it automatically triggers the insurance payment to the claimant? If the flight is on time, the smart contract pays itself.**
>
> This is indeed a revolutionary feature of blockchain, as it provides flexibility, speed, security, and automation for real-world scenarios that can lead to a completely trustworthy system with significant cost reductions. Smart contracts can be programmed to perform any actions that blockchain users need and according to their specific business requirements.

---

#### Smart property

- It is possible to link a **digital or physical asset** to the **blockchain** in such a secure and precise manner that it cannot be claimed by anyone else. You are in **full control of your asset**, and it **cannot be double-spent** or **double-owned**. Compare this with a **digital music file**, for example, which can be **copied many times** without any controls. While it is true that many **Digital Rights Management** (**DRM**)schemes are being used currently along with **copyright laws**, **none of them are enforceable** in the way a **blockchain-based DRM can be**. Blockchain can provide **digital rights management** functionality in such a way that it can be **enforced fully**. There are **famously broken DRM schemes** that looked great in theory but were hacked due to one limitation or another. One example is the Oculus hack: http://www.wired.co.uk/article/oculus-rift-drm-hacked. Another example is the **PS3 hack**; also, copyrighted digital music, films, and e-books are **routinely shared on the internet without any limitations**. We have had **copyright protection in place for many years**, but **digital piracy** refutes all attempts to fully enforce the law. On a blockchain, however, if you own an asset, no one else can claim it unless you decide to transfer it. This feature has far-reaching implications, especially in DRM and e-cash systems where double-spend detection is a crucial requirement. The double-spend problem was first solved without the requirement of a trusted third party in Bitcoin.

- Summary: Use blockchain to buy shit like movies and it enforces security via cryptographically secure methods to enforce laws and prevent double spending to name a few.

> Issue with blockchain is it must be **accessible, robust and useful**, which is the strive at the moment

### Sensitive Blockchain Problems

#### Scalability

- Currently, blockchain networks are **not as scalable as**, for example, **current financial networks.** This is a **known area of concern** and a **very ripe area for research.**

#### Adoption

- Often, blockchain is seen as a **nascent technology**. Even though this **perspective** is **rapidly changing**, there is still a long way to **go before the mass adoption** of this **technology.** The challenge here is to **allow blockchain networks** to be **easier to use** so that **adoption can increase**. In addition, several other challenges such as scalability (introduced previously) exist, which must be solved in order to increase adoption.
- *summary*: make it easy to use the blockchain 

#### Regulation

- Due to its **decentralized nature,** regulation is **almost impossible** on **blockchain.** This is sometimes seen as a **barrier toward adoption** because, traditionally, due to the **existence of regulatory authorities**, consumers have a **certain level of confidence** that if something **goes wrong** they can **hold someone accountable**. However, in **blockchain networks**, **no such regulatory authority and control exists**, which is an **inhibiting factor** for many consumers.
- *Summary*: defi means no one can be held accountable for error if a transaction goes wrong for example, or smart contract incorrect.

#### Relatively immature technology

- As compared to **traditional IT systems** that **have benefited from decades** of **research**, **blockchain** is still a **new technology** and requires a **lot of research** to achieve **maturity**.
- summary: new technology, not a fully researched field.

#### Privacy and confidentiality

- Privacy is a **concern** on **public blockchains** such as Bitcoin where **everyone can see** every single transaction. This **transparency** is **not desirable** in **many industries** such as the **financial, law, or medical sectors**. This is also a known **concern** and a l**ot of valuable research** with some **impeccable solutions has already been developed**. However, **further research** is still required to **drive the mass adoption of blockchain.**
- Summary: every can see the blockchain, so transactions cant be hidden.

## Types of Blockchain

### Distributed Ledgers

-  It should be noted that a ***distributed ledger*** is a broad term describing **shared databases**; hence, **all blockchains technically fall** under the **umbrella of shared** databases or **distributed ledgers**. Although all blockchains are fundamentally distributed ledgers, all distributed ledgers are not necessarily blockchains.
- Summary: all blockchains have this attribute not all DL are blockchains.

#### Blockchain vs distributed ledger

- A critical difference between a **distributed ledger** and a **blockchain** is that a **distributed ledger** does **not necessarily consist of blocks** of **transactions** to keep the **ledger growing**. Rather, a **blockchain** is a **special type of shared database** that is **comprised of blocks of transactions**. An example of a **distributed ledger** that does not use **blocks of transactions is R3's Corda** ([https://www.corda.net](https://www.corda.net/)). Corda is a **distributed ledger** that is developed to **record and manage agreements** and is especially **focused on the financial services industry**. On the other hand, more widely known **blockchains** like **Bitcoin** and **Ethereum** make use of **blocks** to **update** the **shared database.**
- summary: ledger doesnt necessarily store blocks of transactions but blockchain dose.

---

- As the name suggests, a **distributed ledger** is **distributed among its participants** and **spread across multiple sites or organizations**. This type of ledger can be either **private or public**. The fundamental idea here is that, unlike many other blockchains, the **records are stored contiguously** instead of **being sorted into blocks**. This concept is **used in Ripple**, which is a blockchain- and cryptocurrency-based global payment network.
- store records in the shared database contiguously for distributed ledgers, used in XRP.

#### Distributed Ledger Technology

- It should be noted that over the last few years, the terms **distributed ledger** or **DLT** have grown to be commonly used to **describe blockchain in the finance industry.**
-  Sometimes, blockchain and **DLT are used interchangeably**. Though this is **not entirely accurate**, it is how the **term has evolved recently**, especially in the **finance sector.** 
- In fact, **DLT is now a very active** and **thriving area of research** in the **financial sector**. From a **financial sector point of view**, DLTs are **permissioned blockchains** that are **used by consortiums.** 
- **DLTs** usually serve as a **shared database**, with all **participants known and verified.** They **do not have a cryptocurrency** and **do not require mining** to secure the ledger.
- summary: DLT and blockchain are used as similar terms, but the research of DLT in finance is seen as permissioned blockchains used by consortiums (*an association, typically of several companies.*)

> At a broader level, DLT is an **umbrella term that represents Distributed Ledger Technology** as a **whole,** comprising of **blockchains and distributed ledgers** of **different types.**

### Public blockchains

- As the name suggests, public blockchains are **not owned by anyone.** They are open to the public, and **anyone can participate** as a **node in the decision-making process**. Users **may or may not be rewarded** for **their participation**. All users of these **"permission less"** or **"un-permissioned"** ledgers **maintain a copy of the ledger** on their **local nodes** and use a **distributed consensus mechanism** to **decide** the **eventual state of the ledger.** **Bitcoin** and **Ethereum** are **both considered public blockchains**.

### Private Blockchains

- As the name implies, private blockchains are just that—private. That is, **they are open only to a consortium or group of individuals or organizations** who have decided to **share the ledger among themselves**. There are **various blockchains** now available in this category, such as **Kadena and Quorum**. Optionally, **both of these blockchains** can also **run in public mode if required**, but their **primary purpose** is to provide a **private blockchain.**

### Semi Private Blockchains

- With **semi-private blockchains**, part of the **blockchain is private** and part of it is **public**. Note that this is still just a **concept today**, and no real-world proofs of concept have yet been developed. With a semi-private blockchain, the **private** part is **controlled by a group of individuals**, while the **public part is open for participation by anyone.**
- This hybrid model can be used in scenarios where the **private part of the blockchain** remains **internal and shared among known participants**, while the public part of **the blockchain can still be used by anyone**, optionally allowing **mining to secure the blockchain.** 
- This way, the blockchain as a whole can be secured using PoW, thus providing **consistency and validity** for both the private and public parts. This type of blockchain can also be called a **"semi-decentralized" model**, where it is **controlled by a single entity** but still **allows for multiple users** to **join the network by following appropriate procedures.**

### Sidechains

- More precisely known as **"pegged sidechains,"** this is a concept whereby coins can be **moved from one blockchain** to **another and then back again**. Typical uses include the **creation of new altcoins** (alternative cryptocurrencies) whereby **coins are burnt** as a **proof of an adequate stake**. "**Burnt**" or "**burning** the coins" in this context means that the **coins are sent to an address** that is **un-spendable**, and this **process makes the "burnt" coins irrecoverable**. This mechanism is used to **bootstrap a new currency** or **introduce scarcity**, which **results** in the **increased value of the coin**.
- This mechanism is also called **"Proof of Burn"** and is used as an alternative method for **distributed consensus** to **PoW and Proof of Stake (PoS)**. The example provided previously for **burning coins** applies to a **one-way pegged sidechain**. The second type is called a **two-way pegged sidechain**, which allows the **movement of coins** from the **main chain to the sidechain** and **back to the main chain when required**
- This process enables the **building of smart contracts** for the **Bitcoin network**. **Rootstock** is one of the leading examples of a **sidechain**, which enables **smart contract development** for **Bitcoin using this paradigm**. It works by allowing a **two-way peg for the Bitcoin blockchain**, and this results in much faster throughput.
- summary: moving coins between different blockchains, requires a proof of burn which means to send coins to an unspendable address. Used to create smart contracts to work on bitcoin. https://coinmarketcap.com/alexandria/glossary/proof-of-burn-pob, basically prooving it can participate by virtual mining concept. using less energy.

### Permissioned Ledger

- A *permissioned ledger* is a blockchain where **participants of the network** are already **known and trusted**. Permissioned ledgers do **not need to use a distributed** **consensus mechanism**; instead, an **agreement protocol** is used to **maintain a shared version of the truth** about the **state of the records** on the **blockchain**. In this case, for **verification of transactions** on the **chain**, all **verifiers are already preselected** by a **central authority** and, typically, there is **no need for a mining mechanism**.

  By definition, there is also **no requirement** for a **permissioned blockchain** to be **private**, as it can be a **public blockchain** but with **regulated access control.** For example, Bitcoin can **become a permissioned ledger** if an **access control layer** is introduced on top of it that **verifies the identity** of a **user and then allows access** to the **blockchain**.

- Summary: verification is pre approved like signing up and giving your passport for example, so no distributed consensus mechanism is required, just an agreement protocol to maintain the shared truth.

### Shared ledger

- This is a generic term that is used to **describe any application** or **database** that is **shared** by the **public or a consortium**. Generally, **all blockchains** fall into the **category of a shared ledger**.
