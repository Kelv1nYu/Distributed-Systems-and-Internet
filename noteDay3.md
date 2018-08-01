(2) Election
a. logically organized ring

b. only the process on the ring can start a new election

c. each process has different states

1. non-paticipant
2. paticipant
3. elected

d. The process

1. correctly functioning
2. with the largest ID

=> new leader

e. ∀ P<sub>4</sub> ∈ D(P<sub>17</sub>) starts a new election

1. election(17)
2. state -> paticipant
3. pass "election(17)" clockwise to the next

f. when P<sub>j</sub>(P<sub>24</sub>)receive "election(17)"

if election id < P<sub>j</sub> id

Pj(P24) then,
1. if P<sub>j</sub> state == "non-paticipant", 
then P<sub>j</sub> state = paticipant and election id = P<sub>j</sub> id, then pass "election(new id)" clockwise;
2. else, P<sub>j</sub> discards "election"


else then,
1. if P<sub>j</sub> state == "non-paticpant", then P<sub>j</sub> state = paticipant
2. pass "election(orignal id)" clockwise

if P<sub>j</sub> id == election id(“election” 传了一圈回来发现自己是最大的id), then
1. P<sub>j</sub> will make itself the leader
2. from paticipant to non-paticipant
3. creates "elected" pass it clockwise

When P<sub>i</sub> receive "elected", P<sub>i</sub>
1. state = non-paticipant
2. pass it along
only if(P<sub>i</sub> ≠ P<sub>j</sub>)

(3) reliable multicast

a. ∀ P<sub>i</sub> ∈ g && 任意 M, P<sub>i</sub> can deliver(接收[accept]) m at most once (deliver ≠ receive)

b. ∀ P<sub>i</sub> ∈ g delivers m, then all the process ∈ g must also deliver m

****
e.g.

P<sub>i</sub> ∈ g, V<sub>i</sub>(0, 0, 0)

P<sub>1</sub> : V<sub>1</sub>(5, 6, 4)
P<sub>2</sub> : V<sub>2</sub>(5, 6, 4)
P<sub>3</sub> : V<sub>3</sub>(5, 6, 4)

V<sub>1</sub>[1] = 5
V<sub>i</sub>[i] = as of now P<sub>i</sub> has multicast V<sub>i</sub>[i] of messages to group (or V<sub>i</sub>[i] is the sequence number the lastest message P<sub>i</sub> has multicast to group)

V<sub>1</sub>[2] = 6, V<sub>1</sub>[3] = 4
V<sub>i</sub>[j] (i ≠ j) is the number of messages that P<sub>i</sub> has delivered from P<sub>j</sub> as of now
****

P<sub>i</sub> is the multicast(sender)
P<sub>j</sub> is the multicast(receiver)

① when P<sub>i</sub> multicast(m) -> group
P<sub>i</sub> => V<sub>i</sub>[i]++; multicast(g, m, V<sub>i</sub>)

② when p<sub>j</sub> receives(g, m, V<sub>i</sub>)
P<sub>j</sub> 

if(V<sub>i</sub>[i] == V<sub>j</sub>[i] + 1)
then V<sub>j</sub>[i]++; deliver(m)

else if(V<sub>i</sub>[i] < V<sub>j</sub>[i] + 1)
(retransmission because of data loss or data delay[只表示receiver在发送acknowledge message的时候丢失或超过sender重传时间])
discards m (duplicate)

else if(V<sub>i</sub>[i] > V<sub>j</sub>[i] + 1)
put it in the queue pending for delivery

if(V<sub>i</sub>[k] > V<sub>j</sub>[k])
then, P<sub>j</sub> requests from P<sub>k</sub> all messages when V<sub>j</sub>[k] < V<sub>k</sub>[k]

multicast must follow order
1. FIFO
2. total ordered
3. causal ordered multicast

P<sub>i</sub> m->m'->m''
P<sub>j</sub> deliver(m)->deliver(m')->deliver(m'')

**total ordered**

q<sub>1</sub>, q<sub>2</sub>, q<sub>3</sub>, q<sub>4</sub> ∈ g
∀ q<sub>i</sub> ∈ g has
1. P<sub>g</sub><sup>i</sup> is the largest protocol(the largest delivery order proposal)by q<sub>i</sub>
2. A<sub>g</sub><sup>i</sup> is the largest agreed proposal that process q<sub>i</sub> has receied(observed) 

① q<sub>i</sub> ∈ g multicast(m), q<sub>i</sub> multicast(g, m)

② when q<sub>j</sub> receives m, q<sub>j</sub> => P = max(P<sub>g</sub><sup>j</sup>, A<sub>g</sub><sup>j</sup>) + 1; P<sub>g</sub><sup>j</sup> = P; send (q<sub>i</sub>, P, m)

③ when q<sub>i</sub> receives all proposal, q<sub>i</sub> =>
a = max(q<sub>2</sub>, q<sub>3</sub>, q<sub>4</sub>) from <i, m>; A<sub>g</sub><sup>i</sup> = max(a, A<sub>g</sub><sup>i</sup>); multicast(g, a, i)

④ when q<sub>j</sub> receives (a, i), q<sub>j</sub> => A<sub>g</sub><sup>i</sup> = max(a, A<sub>g</sub><sup>i</sup>);q<sub>j</sub> need to put <i, m, a> into the delivery queue properly; only deliver <i, m, a>if it's at the front of the queue

a. q<sub>2</sub><i, m, 3>
b. is it possible for m<sub>1</sub>, m<sub>0</sub> to have the same "a" ?

**causal ordered multicast**

a. m<sub>1</sub> -> m<sub>2</sub> require that deliver(m<sub>1</sub>)->deliver(m<sub>2</sub>)

1. when P<sub>i</sub> multicasts "m", P<sub>i</sub> => V<sub>i</sub>[i]++; multicast (g, m, V<sub>i</sub>)

2. when P<sub>j</sub> receives (m ,V<sub>i</sub>) from P<sub>i</sub>, P<sub>j</sub> => 
if(V<sub>i</sub>[i] == V<sub>j</sub>[i]+1) && (V<sub>i</sub>[k] <= V<sub>j</sub>[k]),then V<sub>j</sub>[i]++, deliver(m);
eles, buffer m

### Transactions and Concurrency Control
(1) concurrency control

a. exclusive lock with 2- phase locking

b. readwite lock with 2-phase locking
1. (read lock shared)
2. (write lock exclusive)

① exclusive lock

② readwrite lock

③ 2 - version locking
1. read lock
2. write lock
3. commit lock

c. 2-version locking

(2) Optimistic Concurrency Control

T :  working phase | validation | update

a. working phase
1. a set of tentative versions for the objects that T has updated
2. a write set  W = {O<sub>w</sub><sup>1</sup>, O<sub>w</sub><sup>2</sup>, O<sub>w</sub><sup>3</sup>}
3. a read set R = {O<sub>r</sub><sup>1</sup>, O<sub>r</sub><sup>2</sup>}

b. validation phase

T<sub>1</sub>'s write set intersects T<sub>2</sub>'s read set