### Three common things
(1) concurrency(control)

(2) no global clock e.g. a.GPS b.TV walls

(3) Independent failures

### Challenges
(1) Heterogeneity a.hardware b.OS c.middleware d.programing language

(2) openness a.interfaces published b.communication protocol

(3) security & "privacy"

(4) scalability a.hardware resources b.software resources c.performance loss

(5) failure handling a.detect b.recover from c.tolerate

(6) concurrency cotrol

(7) transparency(可正向翻译，即透明化；亦可反向翻译，即隐藏)

### Quality
(1) end-user -> quality of service a.reliability (correctness, aviliability) b.performance (respose time) c.security/privacy d.adaptability

(2) service provider -> dependability a.correctness b.performance(throughput) c.fault tolerance d.security

### Web server
(1) C/S

(2) P2P

(3) hybrid

### Model
(1) architecture models

(2) Interaction model a.process b.processes do not share memory c.processes only change messages via communication channels

quality of a chnnel a.network delay(processing delay, transmission delay, queuing delay, propagation delay) b.bandwidth c.Jittering(the difference in network delay at different points in time)

(2.1)Different type of Interaction model:
synchronous distributed system a.upperbound of instruction execution b.upperbound of network delay c.upperbound of clock drift rate(the difference per time unit 每秒与原子钟的差别)
若三个都存在，则称为synchronous distributed system；否则，称为asynchronous distributed system

(3) failure model

(4) security model a.confidentiality(机密性) b.authentication(认证) - clint / server c.access control d.message replay e.DDoS(DDoS:Distributed Denial of Service)

### Representation of a remote object reference
(1) Internet address 32bits

(2) port number 32 bits

(3) time 32bits

(4) object number 32bits

(5) interface of remote object

### RPC exchange protocols
(1) point to point communication a.R(Request/one-way) b.RR(Request - Reply)[RMI(Remote Method Invocation 远程方法调用) vs reverse RMI] c.RRA(Request - Reply - Ackownledge reply)

(2) broadcast

(3) multicast(group communication 组播[点对多点])

(4) anycast

### publish-subscribe paradigm
(1) publishers a.advertisement() b.filter() c.subscribe() d.unsubscribe()

(2) subscribers a.notify()

### System V IPC
(1) MQ(消息队列)
(2) semophore(信号量)
(3) shared memory(共享内存)

### tuple space
Internet of Thing(IoT) <=> ubiquitous network

tuple space is a persistent store

### crytography(密码学)
art/science of scrambling [scramble = encode]

encryption {M}<sub>key</sub> = E(M, key)

decryption M = D({M}<sub>key</sub>, key)

(1) one-way function

(2) non-destructive

symmetric encryption

asymmetric(public/private) encryption

**public key 和 private key是一对，public key加密的msg只能通过private key 来解密，private key加密的msg只能通过private key来解密；且该函数为单向，即private可以推出public，但是public无法推出private；pubilc无法解public本身加密的msg(因为是非对称加密)**

(1) confidential communication between Alice and Bob

**解决方案1和解决方案2都需要一个前提，即Alice和Bob要保证得到的对方的public key是安全且可靠的**

解决方案1： Alice和Bob使用对方的public key，用各自的方法加密，形成共有K<sub>AB</sub>

X = p<sup>x</sup> mod q
Alice K<sub>Apub</sub> = (A, p, q)
      K<sub>Apriv</sub> = (a, p, q)
      A = p<sup>a</sup> mod q

Alice : A, a, p, q, B
Bob   : B, b, p, q, A

Alice : B<sup>a</sup> mod q = (p<sup>b</sup> mod q)<sup>a</sup> mod q = p<sup>ab</sup> mod q = K<sub>AB</sub>

Bob   : A<sup>b</sup> mod q = (p<sup>a</sup> mod q)<sup>b</sup> mod q = p<sup>ab</sup> mod q

解决方案2： Alice使用Bob的public key加密

Alice : K<sub>AB</sub> -> Alice : {K<sub>AB</sub> }KBpub -> Alice send({K<sub>AB</sub> }K<sub>Bpub</sub>, Bob) -> Bob K<sub>AB</sub>  = D({K<sub>AB</sub> }K<sub>Bpub</sub>, K<sub>Bpriv</sub>)

需要解决的不安全的方法：

a. Alice {M}K<sub>AB</sub>  = E(M, K<sub>AB</sub>)

b. Alice send({M}K<sub>AB</sub> , Bob)

c. Bob M = D({M}K<sub>AB</sub> , K<sub>AB</sub> )

(2) Authentication in LAN (Kerberos)

a. everyone(Alice & Bob) -> Sara office for a password

b. Alice E(P<sub>A</sub>) = K<sub>A</sub>

c. Bob E(P<sub>B</sub>) = K<sub>B</sub>

access - based authentication

**K<sub>AB</sub>是Sara给出的**

① Alice : sends request to Sara for downloading files

② M = {{ticket}<sub>K<sub>B</sub></sub>, Alice, K<sub>AB</sub>}<sub>K<sub>A</sub></sub>

③ Alice : {ticket}<sub>K<sub>B</sub></sub>, Alice, K<sub>AB</sub>

④ Alice sends Alice, download file, {ticket}<sub>K<sub>B</sub></sub> to Bob

⑤ Bob ticket = K<sub>AB</sub>, Alice

⑥ Bob sends file to Alice

**问题是其他人可以重复步骤④，重复向Bob发送请求，导致Bob重复向Alice发送文件**

(3) Data authentication on the Internet

digital signature
a.anthenticity(可靠性，确认性，真实性)
b.unforgeability(不可伪造性)
c.non-repudiability(不可否认性)

digest
a. one-way

b. return a fixed number of hits 
不论输入值是多少，输出值的位数总为一个定值，可能为64，也可能为256
（参考checksum）

c. perfect hash function

(a) D<sub>M</sub> = digest(M)

(b) {D<sub>M</sub>}<sub>K<sub>Apriv</sub></sub> = [M]<sub>K<sub>Apriv</sub></sub>

(c) send Bob M,[M]<sub>K<sub>Apriv</sub></sub>

(d) Bob : 

digest(M) = D<sub>M</sub><sup>'</sup>

D([M]<sub>K<sub>Apriv</sub></sub>, K<sub>Apub</sub>) = D<sub>M</sub>

若D<sub>M</sub><sup>'</sup>与D<sub>M</sub>不同，则Document未被更改；若相同，则说明被改变
