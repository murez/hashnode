# Paper Review of Distributed snapshots: determining global states of distributed systems

## Paper Summary

This paper presents an algorithm to determinate a global state of a distributed system during the computation where the processors in the system do not share a common clock or memory by taking distributed snapshots. This global state can help to detect service stability by identifying errors like deadlock and computation termination.

## Paper Details

This paper assumes that a distributed system is consisted of a series of processes and directed channels which can construct a DAG where processes are vertices and channels are edges. And each process can compute, send/receive messages and record state by itself. So the state on the process can be presented as a tuple $e = &lt;p, s, s', M ,c&gt;$ where $p$ is the process, $s$ is the state of $p$ before $e$ and $s'$ is the state of $p$ after $e$, $M$ is the delivered message and $c$ is the channel to communicate. And each channel is error-free, has a infinity sending and receiving buffer and messages is ordered.

And the minimized system is shown below.

So when a message is delivered, we have four status:

1. Message in \\(p\\)
    
2. Message in $c$
    
3. Message in $c'$
    
4. Message in $q$
    

The errors can happens every where but we cannot determinate where it happens, so the paper introduce an algorithm to snapshot the system.

### Distributed Snapshots Algorithm

This paper introduce the following algorithm to take a snapshot of the whole system:

#### for Sender $P$

$P$ keeps track of its state (computational result or system attributes) and sends a marker to each channel.

#### for Receiver $Q$

1. If $Q$'s state hasn't been recorded, record it and set channel $c$'s state to null.
    
2. If $Q$'s state has been recorded, then from the first message $Q$ received to the marker, $Q$ set channel $c$'s state messages.
    

## Strong Points

1. This paper gives a appropriate definition of the distributed system model in terms of process, channels, events, and global state, as well as the interaction between them.
    
2. This paper uses basic sending and receiving principles under a no sharing clock and memory environment to solve the problem.
    
3. This paper's algorithm does not interrupt the normal execution of process.
    

## Weak Points

1. This paper gives a limited networking environment where channels never fail and messages are ordered.
    
2. The algorithm's output can be regarded as a decent prediction rather than the "real" state.
    

## Improvement

We may introduce a global clock and take more tight snapshots at a same globally time, which should result in a fine grain global state that is closer to the reality.