---
layout: post
title:  "Observability vs Monitoring : A DevOps Perspective"
author: israel
categories: [ 'Cloud Native' ]
tags: [containers, devops, cloud-native, kubernetes ]
image: https://user-images.githubusercontent.com/2548160/147579433-1c8ead27-8c3a-436c-adc0-3ac194fc7bfe.jpg
date:   2021-12-28 15:01:35 +0300
permalink: /blog/ob.html

---

When organisations adopt the DevOps culture, they typically start by decomposing monolithic applications into microservices architectures to improve scalability, deployment cadence, fault isolation, etc. Traditional Application Performance Monitoring (APM) tools are typically designed to address predictable failures in monoliths, and as a result, they are incapable of monitoring modern distributed applications.

As (cloud-native) microservices architectures become the norm for most modern applications, effective debugging requires that the application (and its underlying infrastructure) be observable. In other words, the system's internal state can be deduced by observing its output. Consequently, the need for observability is becoming increasingly important as businesses strive to scale their DevOps strategies to keep up with the ever-increasing complexity of the software delivery process.

Moreso, while observability is crucial in any DevOps-oriented organisation, it is often confused with monitoring. Although observability and monitoring are not the same, they are often used interchangeably by vendors and customers alike. 

Additionally, observability and monitoring are not mutually exclusive. The purpose of this post is to demystify the differences and relationships between monitoring and observability.

## What is Monitoring? 
A monitoring system must answer two fundamental questions - "what is broken, and why?" (source: <a href="https://sre.google/sre-book/" target="_blank"> Google SRE book </a>). A monitoring tool provides critical insights and information about an application's performance and usage trends - this includes information on memory issues, code bottlenecks, availability, server health, end-user experience, and much more. 
Refer to " <a href="https://sre.google/sre-book/monitoring-distributed-systems/" target="_blank"> Chapter 6 - Monitoring Distributed Systems </a> of the <b> Google SRE book </b> for details. 

Furthermore, monitoring provides insights into how an application, network, or infrastructure is performing. It is critical for building operations and business dashboards, creating health rules (using static thresholds or dynamic baselines), analysing usage and system performance trends, etc.

Monitoring, on its own, has a disadvantage in a complex microservices architecture since production failures are non-linear and difficult to predict owing to the distributed nature of microservices. Despite this disadvantage, monitoring remains essential for developing and operating modern distributed applications. If the monitored metrics and health rules are simple and focused on actionable data, they will provide the business with a solid picture of how healthy the system is--answering the "what is broken, why" question.

To summarise, monitoring helps organisations to: 
- Detect system issues: It alerts you to problems or displays them on dashboards. Improves Mean Time to Detection (MTTD)
- Problem Resolution: Improve Mean Time to Repair (MTTR and aid Root Cause analysis (RCA) of problems. 
- Continuous Improvement: Enhance capacity planning, financial planning, trending, performance engineering, reporting, software delivery process.

## What is Observability? 
While monitoring answers the "what is broken and why?" question, observability uncovers the unknown-unknows. Observability orgininated from a <b>control theory </b> (source <a href="https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-241j-dynamic-systems-and-control-spring-2011/readings/MIT6_241JS11_chap24.pdf" target="_blank">  MIT : Dynamic Systems and
Control </a> ). 


> The objective of the Control Theory is to develop a model or algorithm governing the application of system inputs to drive the system to a desired state, while minimising any delay, overshoot, or steady-state error and ensuring a level of control stability; often with the aim to achieve a degree of optimality.
<i> Source <a href="https://en.wikipedia.org/wiki/Control_theory" target="_blank"> Wikipedia: Control theory </a> </i>

Observability, therefore, is a measure of how well a system's internal state can be deduced from its external characteristics or outputs. In this context, the `internal state` refers to the unknown-unknows, or unpredictability/non-linearity of a failure in a distributed system, and the `outputs` refers to the Metrics, Events, Logs and Traces (aka MELT) data from the observable system. Thus, MELT data are the pillars of observability. 

<p class="aligncenter">
<img class="lazyimg" src="https://user-images.githubusercontent.com/2548160/147602582-abbee2bb-f030-4f3f-95cd-23b9b5329b1e.jpg"/> 
<br>
</p>
<i>Source <a href="https://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html"> metrics-tracing-and-logging</a> </i>
 
Unlike monitoring, observability is a measure of a system's ability to diagnose what is going on inside rather than a tool used to measure the system's performance. In other words, when a system is observable, it allows for the measurement and inference of its internal state; this provides an additional context to allow you to get to the underlying cause of problems quicker. This characteristic is the primary reason why observability is more suitable for gaining insights into the internals of a complex microservices architecture.

A software observability solution must be able to answer the following questions: 

- Which services did a request traverse, and where did performance bottlenecks occur?
- How did the request's execution deviate from the intended system behaviour?
- Why did the request fail?
- How was the request processed by the service?

## So, how are Observability and Monitoring different? 

This section summarises the differences and relationships between Observability and Monitoring. 

#### Mutual Exclusivity 
Observability and monitoring are NOT mutually exclusive. Observability precedes monitoring; that is, you set up your monitoring after a system has been observed. In other words, observability is a superset of monitoring. A system can be observed in many ways. However, the most popular method for observing applications is via instrumentation, which injects agents into the application's byte code. The agent can either be vendor-specific or an opensource agent - such as the <a href="https://opentelemetry.io/docs/collector/getting-started/" target="_blank"> OpenTelemetry </a> agents. 

#### Purpose  
Observability and monitoring are complementary, with each fulfilling a different purpose. 

Monitoring uses a sampling mechanism to collect data such as response time, requests, downtime, bottlenecks, and so on to track the overall health of a system. On the other hand, observability provides additional contextual information into a failure by tracing the requests through different microservices. 

#### Addressing the unknown-unknowns
Monitoring seeks to report known and predictable failures, while observability seeks to detect issues that users have not yet found. The underpinning principle of observability is to handle known failures and identify incoming issues before the users discover them.

## Conculusion  
Although observability and monitoring are not the same, they are often used interchangeably by vendors and customers alike, which creates lots of confusion.
Monitoring and observability have distinct objectives, and they are not the same thing. Observability does not replace monitoring. They are NOT mutually exclusive; rather, they are mutually reinforcing.

Monitoring tracks the overall health of a system, and it is best suited to measure known and limited KPIs and failure modes. Observability, on the other hand, aims to provide highly granular insights into the behaviour of a system—the rich context makes it more suitable for cloud-native microservices architecture.
