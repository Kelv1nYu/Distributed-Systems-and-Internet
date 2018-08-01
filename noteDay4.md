(3)we use the start time of transaction as its timestamp
a. for each object to be accessed by transcation
1. tentative versions
2. write set
3. read set

b. if T's write request is approved
1. create a tentative version
2. add T to the write set

c. if T's read request is approved
1. add T to the read set
2. identify the tentative version with the largest timestamp(the largest timestamp <= T's timestamp)

d. if T is allowed to commit
1. make all the tentative versions it has modified the commited versions
2. remove T from the objects read & write sets

### Replication
(1) performance
(2) reliability/availability

**Front Ends(FE), Replica managers(RM)**

a. a client communicates with one FE

b. Interaction between FE & RMs
1. forward: FE forwards requests to one or more RMs
2. Coordination: how RM(s) coordinate the request and process them in order
3. execution : RM(s) execute the business logic of the transcation for request
4. Agreement : make sure the states of RMs to be consistent
5. Notification : one or more RMs reply to FE with a response

#### passive model

① FE -> Primary
② primary FCFS(priority queue)
③ only primary
④ Agreement - primary sends updates to backups
⑤ primary -> FE

#### Active replication

a. FE TR-multicasts(total & reliable - order) request to all RMs

b. coordination between RMs enforce reliable total order

c. Each RM

d. No need

e. Each RM sends notification back to FE

****
(1) linearizability

a. The interleaved sequence of operations meets the specification of a (single) correct copy of objects.

b. The order of operations in the interleaving is consistent with the real times at which the operations occurred in the actual execution. 

c. The order of operations in the interleaving is consistent with the program order in which each client executed them