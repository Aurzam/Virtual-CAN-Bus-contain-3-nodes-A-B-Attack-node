# Virtual-CAN-Bus-contain-3-nodes-A-B-Attack-node
Make a virtual CAN bus 

contain 2 nodes

First node name A and send two messages :

one in non-protected and go on path (a1 every t sec)

Two is protected by AES-128 and goes on the path (a2 every t sec)



Node B :
one in non-protected and go on path (b1 every t sec)

two is protected by AES-128 and goes on path (b2 every t sec)

Node A receive b1 and b2 

Node B receive a1 and a2

Create new node on the path name 

Attack node :

just to sniff the path and see all the messages was send on it the protected and non-protected.

Messages : 
A1 : My name is node A and the data is (A1) and [COUNTER START WITH 0]

A2 : My name is node A and the data is (A2 and C)

B1 : My name is node B and the data is (B1) and [COUNTER START WITH 0]

B2 : My name is node B and the data is (B2 and C)
