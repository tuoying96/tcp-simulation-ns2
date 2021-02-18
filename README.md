# tcp-simulation-ns2

# Project 3 Performance Analysis of TCP Variants_Milestone

## 1 Introduction
Transmission Control Protocol (TCP) worked reliably to transfer data stream between networks, but it could not provide satisfactory performance in large and congested networks.   
Several TCP variants, like TCP Tahoe, Reno, NewReno, Vegas, BIC, CUBIC, SACK, and others, are proposed to manage and avoid congestion. This paper aims to analyze the performance of these different TCP variants. We have three experiments and will use the NS-2 network simulator to perform experiments on the TCP variants' behavior under various load conditions and queuing algorithms.  
In the first experiment, TCP Performance Under Congestion, we will analyze TCP variants' performance (Tahoe, Reno, NewReno, and Vegas) under the influence of various load conditions. Next,  in the second experiment, Fairness Between TCP Variants, we will conduct experiments to analyze the fairness between different TCP variants.  Because there are many different operating systems out on the Internet, they may use a different TCP variant. Finally, queuing disciplines like DropTail and Random Early Drop (RED) are algorithms that control how packets in a queue are treated. In the third experiment, Influence of Queuing, instead of varying the CBR flow rate, we will examine the impact of nodes' queuing discipline on the overall throughput of flows.


## 2 Questions:
1. what is the seed of the flow (as mentioned in the piazza post).
2. Is there any other parameter we can use for generalization. Is generalization different for the three experiments
3. how is any of the experiment related with recovery after the added flow is removed (as mentioned in the piazza post).
4. Experiment 3 says we do not need to vary the bandwidth of the CBR flow, but what value it should be?
5. Ask for some example how to do the generalization

Before any experiment, we should set up the networks to the topology, to make the delay time between N1(N5) to N4(N6) in the order of 10ms, to make sure the flow can be as large as 10Mbps, and try to add a TCP flow when the CBR flow rate is 10Mbps and make sure it works.

Generalization: If for the same input, the output is always the same (e.g. if the CBR flow rate is fixed, the packet loss rate is always the same after we add in the TCP), then the result is only a special case. We need to do something to make the result general. I think maybe we should randomly choose some other parameters from a certain range. These parameters can be the delay time, how much time TCP runs, or the seed, etc.

## 2 Design of Experiments and Methodology
### 2.1 Methodology
                         N1                      N4
                           \                    /
                            \                  /
                             N2--------------N3
                            /                  \
                           /                    \
                         N5                      N6
Networks topology for our three experiments is shown in the figure above, and set the bandwidth of each link to 10Mbps.  
All experiments are conducted in NS-2, an object-oriented, discrete event driven network simulator developed at UC Berkely written in C++ and OTcl.  
### 2.2 Parameters Setting
**Queuing Discipline**: The queuing discipline of each link in experiment 1 and experiment 2 is DropTail with default queue size. In experiment 3, we compare the performance of two queue algorithms - Drop Tail and Random Early Drop (RED).  
**Window Size**: The window size of TCP streams in all the experiments are set as 20, which is the default size in NS2.  


### 2.3 Experiment 1: TCP Performance Under Congestion
#### 2.3.1 Purpose:
The purpose in this experiment is to evaluate performance of Tahoe, Reno, New Reno and Vegas under congestion. We vary the CBR rate from 1 to 10Mbps.
#### 2.3.2 Experiment Configurations:
    We will add a Constant Bit Rate (CBR) from N2 to N3, then add a TCP stream from N1 to a sink at N4; we start both traffic sources at 0. Therefore, we can analyze the throughput, packet drop rate, and latency of the TCP stream as a function of the bandwidth used by the CBR flow. 
	
	Input should be the flow rate of CBR(Constant Bit Rate) flow.
	Output should be the mean and variance of the throughput, packet drop rate, and latency of the TCP stream as a function of the CBR flow.


	The basic idea to evaluate the performance under congestion of each TCP variants would be: 
	(a) set up cases with CBR flow's rate from 1Mbps to 10Mbps with 1Mbps spacing. 
	(b) in each case, after the CBR flow is stable (not sure if it has to converge), do: (i) add a single TCP stream from N1 to N4, record the time if it can reach stable state (ii) after TCP stream is stable, write down the parameters of interest.

	About the CBR flow rate: We may want to try some value larger than 10Mbps to see if there is any difference, hope there will be a value such that any CBR flow rate larger than it will lead to the same result. We may add new cases to make the spacing smaller if necessary, or may use larger spacing when presenting the results.

	
### 2.4 Experiment 2: Fairness Between TCP Variants
#### 2.4.1 Purpose:
We will conduct experiments to analyze the fairness between different TCP variants. There are many different operating systems out on the Internet, and they may use a different TCP variant, whihc resulting different performance of TCP Variants.
#### 2.4.2 Experiment Configurations:
    We will start three flows: one CBR, and two TCP. We will set a CBR from N2 to N3, then add two TCP streams from N1 to N4 and N5 to N6 respectively. Next, We conduct experiments with the fours TCP variants pairs 
    - Reno/Reno
    - NewReno/Reno
    - Vegas/Vegas
    - NewReno/Vegas
    With one flow from N1 to N4, and the other flow from N5 to N6. We will also plot the average throughput, packet loss rate and latency of each TCP flow as a function of the bandwidth used by the CBR rate.

	The input gets more complicated: the flow rate of CBR, the type of TCP variants, the difference of the start time of the two TCP streams(from 0 to somewhere large enough that the first one is stable).
	The output is the same: the average and variance of the throughput, packet drop rate, and latency of the TCP stream as a function of the CBR flow. Also the influence of the second TCP stream to the first should be noted down.

	The steps of each experiment:
	(a) set up the CBR flow with a certain flow rate
	(b) add the first TCP stream
	(c) at a specific timing, add the second TCP stream
	(d) record the parameters of the two TCP streams till sometime after both of them are stable

	About the CBR flow rate: We can use the value from the first experiment, but in case two TCP streams make difference, we should still try some value out of the range got in experiment 1.
	
	
### 2.5 Experiment 3: Influence of Queuing
#### 2.5.1 Purpose:
In this experiment, we will compare the performance of two different queue algorithms - Drop Tail and Random Early Drop (RED) - with two TCP variants Reno and SACK under congestion. We will set one TCP flow from N1 to N4 and one CBR flow from N5 to N6.

#### 2.5.2 Experiment Configurations:
    We will set one TCP flow from N1 to N4 and another CBR/UDP flow from N5 to N6. We can compare the performance of two different queue algorithms - Drop Tail and RED - with two TCP variants TCP Reno and SACK under congestion.

	Input: one of the two queuing disciplines, maybe the bandwidth of the CBR flow, how much time the TCP stream runs, maybe how often packet loss happens or how many packet losses happen.
	Output: the average throughput of the TCP stream is the most important one. I think the instance throughput of the TCP stream after packet loss also matters. Maybe the time needed to restart is also important.

	The steps:
	(a) run the TCP stream till it's stable
	(b) add in the CBR, record the data

## 3 Statistics Approaches
Three parameters, drop rate, latency, throughput are taken into consideration when evaluating performance here. Drop rate is the percentage of dropped packets in total packets that have been sent. Latency in our experiments is measured as one-way, which is the time from the source sending a packet to the destination receiving it. Throughput is measured in Mbits per second (Mbit/s or Mbps), i.e. total data packets size per second.  

The formulas for calculating them are as follows:  
  
$$\[latency = \frac{\sum_{i=1}^n trip time of reveived packet i}{number of reveived packets}\]$$  
$$\[throughput = \frac{total packets received * avg packet size}{time}\]$$  
$$\[throughput = \frac{dropped pacckets}{total packets sent} * 100%\]$$  

### Experiment 1
In Experiment 1, we should plot the average throughput, packet loss rate, and latency of each TCP flow as a function of the bandwidth used by the CBR flow to answer the following questions,
  1. Which TCP variant(s) are able to get higher average throughput? 
  2. Which has the lowest average latency? 
  3. Which has the fewest drops? Is there an overall "best" TCP variant in this experiment, or does the "best" variant vary depending on other circumstances?
### Experiment 2  
In Experiment 2, we should plot the average throughput, packet loss rate, and latency of each TCP flow as a function of the bandwidth used by the CBR flow, based on four pairs of TCP variants,  
 - Reno/Reno  
 - NewReno/Reno  
 - Vegas/Vegas  
 - NewReno/Vegas   
  
We should answer the questions,    
  1. Are the different combinations of variants fair to each other? 
  2. Are there combinations that are unfair, and if so, why is the combination unfair? 
  3. How the protocols are implemented and why the different choices in different TCP variants can impact fairness.
### Experiment 3
In Experiment 3, we should plot the performance of the TCP and CBR flow over time to answer the following questions,
  1. Does each queuing discipline provide fair bandwidth to each flow?
  2. How does the end-to-end latency for the flows differ between DropTail and RED?
  3. How does the TCP flow react to the creation of the CBR flow?
  4. Is RED a good idea while dealing with SACK?