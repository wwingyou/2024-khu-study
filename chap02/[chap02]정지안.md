# Back-of-the-envelope Estimation

The act of conducting experiments to estimate values based on universally accepted performance metrics

To assess whether a design meets the requirements

## Latency Value

| Operation | Latency(ns) |
| --- | --- |
| L1 Cache reference | 0.5 |
| Branch Mispredict | 5 |
| L2 Cache reference | 7 |
| Mutex lock/unlock | 100 |
| Main memory reference | 100 |
| 1kB compression using Zippy | 10,000 |
| Transmitting 2KB over a 1Gbps network | 20,000 |
| Contiguosly read 1MB from memory | 250,000 |
| Round-trip message latency within the same data center | 500,000 |
| Disk seek time | 10,000,000 |
| Contiguosly read 1MB from network | 10,000,000 |
| Contiguosly read 1MB from disk | 30,000,000 |
| Round-trip packet latency from California to the Netherlands | 150,000,000 |

Conclusions

- Memory is fast, but disk is still slow
- Avoid disk seeking as possible
- Simple compress algorithm is fast
- Compress data before transmitting them via internet
- Transmitting data between data centers shows high latency

## Metrics related to High Availability

High Availability: The ability of a system to operate continuously without interruption for extended periods of time

100% → Never having experienced any downtime

Typically, services aim for availability values of 99% or higher

Service Level Agreement(SLA): An agreement between the service provider and the customer

The uptime of the service provided by the service provider is officially documented

## Tips

- Do rounding and approximation rather than performing complex calculations with precision
- Write down assumptions for later reference
- Always specify units to prevent ambiguity
- Typically, problems involving QPS (Queries Per Second), maximum QPS, storage requirements, cache requirements, and server count estimations arise frequently

# Strategies for System Design Interviews

System Design Interviews ⇒ 

Demonstrating design skills and the ability to defend decisions made during the process

1. Understanding the problem and defining the scope of the design
    
    It is risky to provide answers without fully understanding the requirements
    
    → red flag
    
    Gather necessary information for system construction through questioning or making assumptions (**Write down assumptions for later reference)**
    
2. Presenting a rough design proposal and seeking agreement
    
    Present the initial blueprint of the design to the interviewer and seek feedback
    
    Calculate rigorously whether the initial design meets the constraints related to the system's scale
    
3. Detailed design
    
    Identify the key areas of focus and prioritize the components based on their importance and interdependencies
    
4. Conclusion
    
    Never say that there’s no room for improvement
    
    It's also beneficial to summarize the design and explain potential issues that may arise if errors occur
    

Dos

- Confirm through questioning
- Understand the requirements
- Keep in mind that there is no single correct answer
- Communicate with the interviewer
- Start from the most important component
- Never give up

Don’ts

- Don't prepare for typical interview questions
- Present the design before understanding the requirements
- Get caught up in details from the start