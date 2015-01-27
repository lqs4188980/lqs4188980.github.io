---
layout: post
title: "Preparing For System Design"
categories: Notes
---
Preparing For System Design
========================
<hr />
The System Design Process
--------------------------------------------

###Step1: Constraints and use cases	
Clarify the system's constraints and to identify what use cases the system needs to satisfy.

###Step2: Abstract design	
The goal of this is to outline all the important components that your architecture will need.	

Sketch your main components and the connections between them.	

Don't dive deep into some particular aspect of the abstract design before sketching out the important components and the connections between them. 	

Separate the system into different layer, keep abstraction and provide interfaces to lower layer. We can also separate into different module, and describe how they are connected.	

###Step3: Understanding bottlenecks	

From high-level design, start thinking about what bottlenecks it has.	

> Remember, usually each solution is a trade-off of some kind. Changing something will worsen something else. However, the important thing is to be able to talk about these trade-offs, and to measure their impact on the system given the constraints and use cases defined.	

###Step4: Scaling your abstract design	

Until the abstract design is done.	

