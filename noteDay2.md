(4) Authentication on the Internet

a. Alice's bank account certificate
1. Certificate type : Account number
2. Name : Alice
3. Account : 6262626
4. Certifying authority: Bob's Bank
5. Signature : {Digest(field2 + field3)}<sub>K<sub>Bpriv</sub></sub>

b. Public-key certificate
1. Certificate type : Public Key
2. Name : Bob's Bank
3. Public key : K<sub>B<sub>pub</sub></sub>
4. Certifying authority: Fred - The Bankers Federation
5. Signature : {Digest(field2 + field3)}<sub>K<sub>Fpriv</sub></sub>

(5) access control

a. ACL(access control list) - centralized solution:
1. who are you
2. resource location
3. intended operation

b. capability - based access control
1. resource ID
2. permission
3. {H(1+2)}<sub>K<sub>Spriv</sub></sub>

Alice show Bob the followings : "r" from "exam.pdf" & resource ID + permission + digital signature

Bob : H(1+2) = d' & D([d]<sub>K<sub>Spriv</sub></sub>, K<sub>Spub</sub>) -> compare d' and d, check permission

1. resource ID
2. permission
3. password

Alice show Bob the followings : "r" from "exam.pdf" & password

Bob : Which file? -> Alice's password is one of the password of any capabilities.

#### Cipher block chaining
a. intercept n<sub>i</sub>

b. {n<sub>i</sub>}<sub>K</sub> = E(n<sub>i</sub> XOR {n<sub>i-1</sub>}<sub>K</sub>, K)

c. {n<sub>0</sub>}<sub>K</sub> = E(n<sub>0</sub> XOR timestamp, K)

d. Alice : send {n<sub>0</sub>}<sub>K</sub>,timestamp

e. Bob : 
D({n<sub>i</sub>}<sub>K</sub> XOR {n<sub>i-1</sub>}<sub>K</sub>);
D({n<sub>0</sub>}<sub>K</sub> XOR timestamp)

if Mallory intercepts the msg:

1. timestamp ≈ the time when mallory intercepts {timestamp}
2. {timestamp}<sub>K</sub> = E(timestamp, K)
3. as soon as match, Mallory knows the key

(1) confusion(对原文以最小的粒度(按字节)进行混淆生成秘文。最简单的confusion可以是A+3=D)

(2) diffusion(原文所包含的位置信息进行打乱排布,比如将第一个字节与第四个字节的位置对换)

#### RSA
**too expensive, slow**
1. P = 13, Q = 17, N = 221
2. (P-1)*(Q-1) = 12 * 16 = 192 = z
3. "d" prime with 192 d = 5
4. e * d = 1 mod 192, e = 385/5 = 77
5. (e, N) = (77, 221); (d, N) = (5, 221)
6. K => 2<sup>K</sup> < 221(N) => K = 7 (each block has 7 bits)
7. E(n<sub>i</sub>, e) = n<sub>i</sub><sup>e</sup> mod N = {n<sub>i</sub>}<sub>e</sub>;
D({n<sub>i</sub>}<sub>e</sub>, d) = {n<sub>i</sub>}<sub>e</sub><sup>d</sup> mod N = n<sub>i</sub>

#### low-cost signatures with a shared secret key
**only used in close group**

### Time and Global States
(1) External synchronization (with Time Server)

(2) Internal synchronization (TV wall)

Ts ->m(t)-> Clients Tc = t + (max_delay+min_delay)/2

Client ->Request-> Ts 收到请求之后经一定时间 ->Reply(t)->Client

[从Client发送Request到接收到Reply(t)的时间为T<sub>round</sub>]

若 Client 发送 Request 的时间最小，Tc = t + T<sub>round</sub> - min

若 Time Server发送Reply时间最小， Tc = t + min

所以最后 Tc = t + Tround/2

(3) Berkclay's
a. master computer polls the slaves
b. master uses cristian's to estimate te current time of the slaves
c. t = (t<sub>0</sub> +t<sub>1</sub>+...+t<sub>n-1</sub>)/n,[fault tolerant range]
d. master informs the slaves of the new time "t"
△<sub>0</sub> = t<sub>0</sub> - t
△<sub>1</sub> = t<sub>1</sub> - t
△<sub>n-1</sub> = t<sub>n-1</sub> - t
e. master sent the △<sub>s</sub> to the slaves

(4) Network Time Protocol (NTP)
a. rpc mode (cristian)
b. symmetric : clock sync between two time servers

1. m(T<sub>i -3</sub>)
2. T<sub>i-2</sub>
3. m'(T<sub>i-1</sub>, T<sub>i-2</sub>, T<sub>i-3</sub>)
4. T<sub>i</sub>, T<sub>i-1</sub>, T<sub>i-2</sub>, T<sub>i-4</sub>

① t is delay from m, t' is delay from m'
② difference between A & B is "O"

T<sub>i-2</sub> = T<sub>i-3</sub> + t - O

T<sub>i</sub> = T<sub>i-1</sub> + t' + O
t +t' = T<sub>i</sub> - T<sub>i-1</sub> + T<sub>i-2</sub> - T<sub>i-3</sub>
O = (T<sub>i-2</sub> - T<sub>i-3</sub> + T<sub>i-1</sub> - T<sub>i</sub>)/2 + (t'-t)/t
O<sub>i</sub>= d<sub>i</sub>/2 <= O <= O<sub>i</sub>+d<sub>i</sub>/2

"hppen - before" relationship
 1. assuming e1 and e2 (both by instruction execution  within the same program) their order of execution will be decided by PC. e1 -> e2 if the pc(e1)< pc(e2)
 2. send(m)->receive(m)
 3. if e1 -> e2 && e2 -> e3, e1 -> e3

 a. Lamport's logical time

 D = {P<sub>1</sub>,P<sub>2</sub>,P<sub>3</sub>}
 for P<sub>i</sub>, there is L<sub>i</sub>. initial L<sub>i</sub> = 0 = P<sub>i</sub>

 L<sub>c1</sub>: when an event occurs at P<sub>i</sub>, L<sub>i</sub> = L<sub>i</sub> + 1

 L<sub>c2</sub>: when P<sub>i</sub> send(m) => update L<sub>i</sub> according to L<sub>c1</sub>; send(m, L<sub>i</sub>)

 L<sub>c3</sub>: when P<sub>j</sub> receive(m) => L<sub>j</sub> = max(L<sub>j</sub>, L<sub>i</sub>); update L<sub>j</sub> according to L<sub>c1</sub>

 if a -> f, then L(a)< L(f)
 if e -> f, then L(e)< L(f)
 if e -> e', then L(e)< L(e')

 **if L(e) < L(e'), e -> e' or e || e' 无法确定只是e -> e'，只能排除e' -> e**

 b. Mattern logical time

 D = {P<sub>1</sub>,P<sub>2</sub>,P<sub>3</sub>}
 for P<sub>i</sub>, there is L<sub>i</sub>. initial P<sub>i</sub> : V<sub>i</sub>(0, 0, 0)

 V<sub>1</sub> = (0, 0, 0)

 V<sub>1</sub> = (4, 5, 3)
 
 V<sub>1</sub>[1] (4) : (V<sub>i</sub>[i] : there have been 4 events occured at P<sub>1</sub>(P<sub>i</sub>)
 
 V<sub>1</sub>[2] (5) : As of now, P<sub>1</sub> has observed 5 events from P<sub>2</sub>

 V<sub>1</sub>[3] (3) ：As of now, P<sub>1</sub> has observed 3 events from P<sub>3</sub>

 M<sub>c1</sub> : when e occurs at P<sub>i</sub>, P<sub>i</sub>: V<sub>i</sub>[i]++;
 
 M<sub>c2</sub> : when P<sub>i</sub> sends m, P<sub>i</sub> => update V<sub>i</sub>[i] according  Mc1; send(m, V<sub>i</sub>)

 M<sub>c3</sub> : when P<sub>j</sub> receives m, P<sub>j</sub> => component wise merge V<sub>i</sub> and V<sub>j</sub> in to V<sub>j</sub>; V<sub>j</sub>[j] according to Mc1

 if e -> e' => V(e) < V(e')

 if V(e) < V(e') => e -> e'

 if V(e) ≮ V(d) && V(e) ≯ V(d) && V(e) ≠ V(d), then e ||(concurrent) d

(1) Distributed mutual exclusion

(2)Multicast synchronization

D = {P<sub>1</sub>,P<sub>2</sub>,P<sub>3</sub>}
for P<sub>i</sub>, there is L<sub>i</sub>. initial P<sub>i</sub> : L<sub>i</sub> = 0 

L<sub>1</sub> = 40, L<sub>2</sub> = 33, L<sub>3</sub> = 23 record the multicast messages

1. L<sub>1</sub> = 41, P<sub>1</sub> multicast(g, m, 41)
2. L<sub>2</sub> = 34, P<sub>2</sub> multicast(g, m', 34)
3. P<sub>1</sub> realizes P<sub>1</sub> needs the token(41);
P<sub>2</sub> needs the token(34)
4. P<sub>2</sub> gets all the votes and P<sub>1</sub> will have to wait
5. P3 multicast(g, m'', 24)

P<sub>2</sub> puts P<sub>1</sub>'s request in the queue without reply

L<sub>1</sub> = 33, L<sub>2</sub> = 33, L<sub>3</sub> = 23 record the multicast messages

1. L<sub>1</sub> = 34, P<sub>1</sub> multicast(g, m, 34)
2. L<sub>2</sub> = 34, P<sub>2</sub> multicast(g, m', 34)
3. P<sub>1</sub> realizes P<sub>1</sub> needs the token(34);
P<sub>2</sub> needs the token(34);
using (L<sub>i</sub>, ProcessID + IP)
