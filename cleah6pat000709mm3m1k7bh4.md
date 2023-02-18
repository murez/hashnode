# Paper Review of PerfScope: Practical Online Server Performance Bug Inference in
  Production Cloud Computing Infrastructures


## Paper Summary

This work introduces PerfScope, an online performance bug inference tool
for production cloud computing systems that does not need source code
and consumes very little system resources. This effort may reduce the
search scope for bug-related functions on prominent open source systems
to a tiny proportion.

## Paper Details

### Algorithm

PerfScope is only capable of diagnosing software issues, such as a
program hang or a system slowdown. To keep track of recent system calls,
PerfScope uses a 5-minute timeframe to collect data.

PerfScope performs its function as described below.

1.  A FSM(Finite State Machine) is used, so that, numerous call episodes
    are constructed for each function call and performed offline and
    system libraries are excluded to facilitate monitoring of
    user-defined functions.

2.  A change in the thread or CPU ID, indicative of a context shift, is
    detected by the online bug inference technique, which extracts the
    execution units.

3.  A hierarchical clustering method is used to locate anomalous units,
    and the appearance vector of the system is recovered using Euclidean
    distances. These aberrant units are then mapped to bug-related
    functions and ranked depending on abnormality level.

4.  PerfScope can deduce the system call route, which may be utilized to
    learn more about the bug's origin and course of spread.

### Experiments

Tests on various open source server systems including Hadoop, HDFS,
Casandra, Tomcat, Apache, Lighttpd, and MySQL are performanced well
which showed PerfScope has appropriately discovered and solved actual
performance problems. When a performance anomaly is identified,
perfscope makes a 90-second system call trace to identify and rank a
subset of likely issues. Moreover the paper gives examples on Cassandra,
HDFS and Lighttpd-1 to show how PerfScope infer bug. And PerfScope
achieves a very low overhead of 1.8 percent runtime.

## Strong Points

1.  PerfScope is an online bug inference tool so there is a wide range
    of applications and it owns a high productivity.

2.  The unavailability of source code makes PerfScope simple to
    implement and does not infringe on user privacy.

3.  Overhead is small enough to be used in a production environment.

## Weak Points

1.  The situation in which numerous functions cause performance concerns
    at the same time is not addressed.

2.  The paper's PerfScope evaluation is confined to a single node
    server, hence its behavior in a multi-node cluster is unknown.

## Brain Storm

Taking the source code into consideration may result in more accurate
detection of performance issues. For example, fuzzing methods may be
used to uncover potential performance issues during the compilation
step.
