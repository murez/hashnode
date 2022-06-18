## Paper Review of Time, clocks and the ordering of events in a distributed system

## Paper Summary

Clock synchronization in distributed systems is a very difficult problem due to the time delay errors in message communication and the differences in distinct physical clocks. 

This paper defines a concept of **happens-before** to describe the **partial ordering** in a system, presents **logical clocks** with a distributed algorithm to order the events, and extends the partial ordering event system into a system with an **total ordering**.  Nevertheless, the system may cause anomalous behavior, the paper extended the system by **Strong Clock Condition** to detach the anomalies. Finally, the paper adds physical clocks into the system and give the resistance of the clocks.

## Paper Details
### Happens-Before

The paper defines **happens-before** so called \\(\to\\) between events \\(e_1\\) and \\(e_2\\) by the following rules:


1. \\(e \to e\\) is not allowed.
2. If \\(e_1\\) occurs before \\(e_2\\) in the same process, then \\(e_1 \to e_2\\).
3. If \\(e_1\\) sends a message and \\(e_2\\) is required to receive it, the \\(e_1 \to e_2\\).
4. If \\(e_1 \to e_2 \\) and \\(e_2 \to e_3\\), then \\(e_1 \to e_3\\).

This is a very intuitive definition.
####  Partial Ordering and Logical Clocks


For every process \\(P_i\\), the paper introduces the logical clock which is a individual clock \\(C_i\\) assigning a number \\(C_i(e)\\) to event \\(e\\). And the paper gives some rules to correct the clock of the system:

1. If \\(e_1, e_2\\) are events in \\(P_i\\) and \\(e_1\\) comes before \\(e_2\\), then \\(C_i(e_1)<C_i(e_2)\\).
2. If \\(e_1\\) is sending a message from \\(P_i\\) to \\(e_2\\) in $P_j\\), then \\(C_i(e_1) < C_j(e_2)\\).


So we can create a system of clocks that satisfies the rules easily by the following steps:


1. A process \\(P_i\\) advances its clock \\(C_i\\) between successive events.
2. Every message must contain a timestamp \\(T_m\\) of what it was sent and when a process \\(P_j\\) receives the message, it advances its clock to be greater than \\(T_m\\).


 So the system can somehow sort all the events by a partial ordering but not a unique one.

### Total Ordering and Anomalous Behavior

The paper then describes a total ordering of events as events sorted by their timestamps so as to handle the problem of resource mutual exclusion problem by a distributed algorithm:


1. To request the resource, process \\(P_i\\) sends the message \\(T_m:P_i\\) requests resource to every other process, and puts that message on its request queue, where \\(T_m\\) is the timestamp of the message.
2. When process \\(P_j\\) receives the message \\(T_m:P_i\\) requests resource, it places it on its request queue and sends a (timestamped) acknowledgment message to \\(P_i\\).
3. To release the resource, process \\(P_i\\) removes any \\(T_m:P_i\\) requests resource message from its request queue and sends a (timestamped) \\(P_i\\) releases resource message to every other process.
4. When process \\(P_j\\) receives a \\(P_i\\) releases resource message, it removes any \\(T_m:P_i\\) requests resource message from its request queue.
5. Process \\(P_i\\) is granted the resource when the following two conditions are satisfied: 
    
5.1. There is a \\(T_m:P_i\\) requests resource message in its request queue.
5.2. \\(P_i\\) has received a message from every other process timestamped later than \\(T_m\\).



The system of clocks ensures the order of two events in the system and if two events can not infect each other then they are considered concurrent and we assign them the timestamp order randomly. 


But this may cause anomalous behavior, like two events are influenced by external events. The paper introduces the **Strong Clock Condition** to use the external information to definite the timestamps of two concurrent events.


## Strong Points

1. This paper is one of the most fundamental concepts in distributed systems and adopted by most of distributed systems.
2. This paper shows that a distributed system is a certain sequential state machine that is implemented with a network of processors. This means the ability to completely organize input requests instantly leads to a method for implementing an arbitrary state machine by a network of processors, and hence any distributed system.
3. This paper proposes a very novel idea to build up a time ordering model from special relativity successfully.

## Weak Points


1. This paper assumes that processes never fail, as we can not tell the difference between a hanging process or a failed process.
2. This paper is hard to decide the order of two concurrent events.


## Improvement
We may deal with the process failure in the system with more acknowledgment between processes.