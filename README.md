[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Yilin Bai
### Student Id: 21072927
### Email: ybaiar@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > **Measurement Tool**: The performance tests were conducted using the **Phoronix Test Suite (PTS)**, an open-source tool for benchmarking CPU, memory, and other system components.

    > **Configuration**: I used Phoronix Test Suite to measure **CPU** and **memory** performance on AWS EC2 instances: **t2.micro**, **t2.medium**, and **c5d.large**. For **CPU** testing, the default settings were used, measuring performance in **MIPS** (Million Instructions Per Second) during compression and decompression tasks. For **memory** testing, the configuration included tests like `copy`, `scale`, `add`, and `triad`, measuring memory throughput in **MB/s** for both integer and floating-point operations.

    > **Explanation**: The **MIPS** values reflect CPU efficiency in processing instructions, with higher values indicating better performance. The **MB/s** values measure memory throughput, with higher values indicating faster memory handling.



2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |     Compression Rating: 4021 MIPS; Decompression Rating: 3127 MIPS   |   Integer: 10833.34 MB/s; Floating Point: 10882.45 MB/s    |
    | `t2.medium`  |   Compression Rating: 10685 MIPS; Decompression Rating: 6152 MIPS  |   Integer: 20291.06 MB/s; Floating Point: 20075.68 MB/s    |
    | `c5d.large` |    Compression Rating: 7482 MIPS; Decompression Rating: 4805 MIPS   |   Integer: 13283.58 MB/s; Floating Point: 13250.19 MB/s    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

    > **Analysis**:  
    > - **CPU Performance**: The `t2.micro` has the lowest performance, while `t2.medium` shows a significant improvement. Surprisingly, `c5d.large` has lower CPU performance than `t2.medium`, likely due to architectural differences.  
    > - **Memory Performance**: `t2.medium` has a notable increase in memory throughput compared to `t2.micro`. `c5d.large` shows higher throughput than `t2.micro`, but does not proportionally scale with the increase in resources.  

    > **Conclusion**:  
    > - **CPU performance** does not scale linearly with the increase in vCPUs. While `t2.medium` outperforms `t2.micro`, `c5d.large` doesn't show a proportional improvement in CPU performance.  
    > - **Memory performance** generally improves with larger instances, but `c5d.large` does not show a proportional increase compared to `t2.medium`, likely due to architectural optimizations.  
    > - Overall, scaling up resources does not always lead to linear performance gains across these EC2 instance types.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |     3950       |  0.276   |
    | `m5.large` - `m5.large`   |     9210       |  0.104   |
    | `c5n.large` - `c5n.large` |     4960       |  0.166   |
    | `t3.medium` - `c5n.large` |     2270       |  0.764   |
    | `m5.large` - `c5n.large`  |     8470       |  0.097   |
    | `m5.large` - `t3.medium`  |     2480       |  0.557   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    > **Analysis**:  
    > - **Within the same instance type**, both **TCP bandwidth** and **RTT** perform best for `m5.large`, followed by `c5n.large`, and then `t3.medium`.
    > - **Between different instance types**, TCP bandwidth tends to be lower and RTT increases, especially between `t3.medium` and `c5n.large`.  

    > **Conclusion**:  
    > - **TCP bandwidth** is typically higher within the same instance type, and **RTT** is lower for same-type connections. Performance drops when instances of different types communicate, with an increase in latency.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |     42.0       |  48.324  |
    | N. Virginia - N. Virginia |     4720       |  0.178   |
    | Oregon - Oregon           |     4790       |  0.177   |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    > **Analysis**:  
    > - **Network performance between the same region** (e.g., N. Virginia to N. Virginia, Oregon to Oregon) shows high TCP bandwidth and low RTT, with **low latency** for connections within each region.
    > - **Network performance between different regions** (N. Virginia to Oregon) has **lower TCP bandwidth** and significantly **higher RTT** compared to within-region connections. This indicates the natural performance drop when connecting instances across different geographic locations.

    > **Conclusion**:  
    > - Network performance degrades when connecting instances between **different regions**, with **higher RTT** and **lower TCP bandwidth**, reflecting the increased distance and cross-region routing.