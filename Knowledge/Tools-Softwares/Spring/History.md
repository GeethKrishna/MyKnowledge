- Java started off by having three editions
	1. Java Standard Edition (Java SE)
	2. Java Enterprise Edition (Java2EE)
	3. Java Micro Edition (J2ME)
- Earlier we used to use J2EE for building any multi layer applications (applications which involves more than one application like client server applications (2 tier) or with an data base (3 tier)). 
- Java2EE comes with core java package which has a very powerful set of functions and API's in it to use. And also it comes with something called EJB (Enterprise Java Bean) which is an server-side component that helps build applications with security, transactions and concurrency all handled automatically.
- This runs inside an EJB container which manages the life cycle and provides services like remote access, load balancing and database interactions.
- But, we've shifted away from J2EE towards Spring cause:
	- EJB way very heavy and complex and the performance of entity beans was also poor.
	- So, from there the next idea came up was to build applications without EJB but create the Java beans not from EJB but from POJOS (Plain Old Java Objects)
	- So, Spring framework is launched with this feature which made it simple and light in weight which made it more performant, people liked it so much and it became popular very fast.
- Other features Spring was providing was:
	- It will provide all configurations by default (pom.xml)
	- Dependency jar file and plugins will be handled automatically.
	- Embedded Tomcat server in application out of the box.
	- Inversion of Control (IoC)
	- Dependency Injection (DI)
	- Spring MVC
	- Spring data JPA
	- Hibernate
	- REST
	- AOP 
	- Spring Security, etc...