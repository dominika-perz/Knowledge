# System Design Concepts
Based on:
* an article by Zubin Pratap: https://www.freecodecamp.org/news/systems-design-for-interviews/  
* CS75 Harvard Course: https://www.youtube.com/watch?v=8KuO4r5CHjM&list=PLmhRNZyYVpDmLpaVQm3mK5PY5KB_4hLjE&index=1  
* YouTube playlist by Gaurav Sen: https://www.youtube.com/playlist?list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX
* System design primer: https://github.com/donnemartin/system-design-primer#study-guide
* System design template: https://leetcode.com/discuss/career/229177/My-System-Design-Template

Basic terms to go throuth durign your System Design interview:
1) Processing and servers  
2) Storage  
3) Caching  
4) Concurrency and communication  
5) Security  
6) Load balancing and proxy  
7) CDN (Content Delivery Network)  
8) Monetization  

General approach:  
1) Who and how will use your services, inputs, outputs - understand the system you are building
2) Write/read ratio, peak-time, how many users, requests per second, realibility vs availability - understand system constraints and priorities
3) High level design (think first for smaller system, circa 100 users)
4) Scale the design - load balancer, number of servers/DB, redundancy and consistency - discuss solutions and trade-offs, identify bottlenecks
5) Security

**Context table**
0. [Operating Systems](#operating-systems)
1. [Networks & Protocols](#networks--protocols)
2. [Storage, Latency & Throughput](#storage-latency--throughput)
3. [System Availability](#system-availability)
4. [Caching](#caching)
5. [Proxy](#proxy)
6. [Load balancing](#load-balancing)
7. [Databases](#databases)
8. [Leader Election](#leader-election)
9. [Polling, Streaming & Socket](#polling-streaming--socket)
10. [Endpoint Protection / Rate limiting](#endpoint-protection--rate-limiting)
11. [Messaging & Pub-Sub pattern](#messaging--pub-sub-pattern)
12. [Logging, monitoring, alerting](#logging-monitoring-alerting)

## Operating systems
**Operating system** is a collection of software that manages hardware and provides services for programs. It is a translator between the user natural language (sually through GUI) and the 0's and 1's understandable for the computer. It hides hardware complexity and manages internal resources. The major components of OS are: **file system**, **scheduler** and **devide driver**.

The most common OS are Windows, macOD and Linux (desktop OS for computers) or Androis and iOS (embedded OS for mobile devices). Linux is a family of open-source OS, with the source code available to all. Thanks to that Linux has different distributions (e.g. Ubuntu, Fedora) and is free. Most servers run Linux as it is relaively easy to customize. 

3 key elements of OS are:  
- **Abstractions** (process, thread, file, socket, memory),  
- **Mechanisms** (create, schedule, open, write, allocate),  
- **Policies** (LRU - least recently used, EDF). 

Examples of basic OD services:
- scheduler
- file system
- memory manager
- block device driver

2 OS design principles:  
- **Seperation of mechanim and policy**  
- **Optimize for common case** - what are use cases for the device? what will th euser want to run on it?  

There are two levels: kernel (privileged mode, this is were OS is) and user (unprivileged mode) level. All hardware access is done form the kernel level. The switch between user and kernel levels is supported by the hardware and takes some time. The programs can interact with the OS via system calls.  
There are 3 types of OS:  
- **Monolithic OS** - (historic) entire OS is working in kernel space, OS is very big, but it already includes everything that is needed
- **Modular OS** - (e.g. Linux, popular) some part of the system core are in independent files called modules that cen be added at the runtime.  
- **Micro OS** - kernel is broken into seperate processes known as servers, same of the servers run in the kernel and some in the user-space.  

### Processes and process management  
**Process** is basically a program in execution, where all tasks indicated in the code are performed. Before program is executed (become a process), it is stored e.g. on disk and it is then called application. Before execution, the program is loaded into the memory, where it can be divided into four sections. All these sections together are **address space** of the process. The memory as seen by the process are virtual addresses, as they do not have to correspounds to the physical addresses (decoupling or abstraction of the memory). The mapping between the virtual addres and the physical addres is hold in **page table**
- **stack** - grows and shrinks, it like LIFO queue, temporary data such as methods parameters, local variables and return addresses, the stack pointer rem,embers where the end of the stack is,  
- **heap** - dynamically allocated memory for runtime,  
- **text** - Program Counter (points to the assembler line of code that is currently being executed) and contents of the CPU registers,  
- **data** - global and static variables. 
The data structure the holds all the information about the process that OS needs to know the current state of the execution is called **Process Control Block (PCB)**.
Thanks to PCB, every time we change which process is being executed (**context switching**), we can resume the newly executing process starting from the same moment that it has been previously stopped at. The context switching is quite expensive, because cache is holding data from the preiously executed process and we will have a lot of **cache misses** (so called **cold cache**). After some time most of the data in the cache will correspounds to the currently run proces (**hot cache**).

During execution, the process can be in different states (the names differ in OS):  
- **Start** - process is created, PCB is created and initialised. After the successful initialisation, the process goes into ready state.
- **Ready** - (idle) ready to be executed, but not currently runnign at the CPU. Such process can be schedule by a scheduler to go to the running state.
- **Running** - currently running on the CPU, it can be interrupted when the context is switched and go back to ready state.
- **Waiting** - CPU not needed, process is waiting for an input from the user or the timer to finish, is reading data from a disk or downloading something from the internet (I/O bound tasks).
- **Terminated/Exit** - when the process terminates (by error or by compliting all the operations) it goes into terminate state.

Each process can create a child processeither by **fork** (the child PCB will be a copy of parent PCB, the executions for both child and parent starts after the instruction fork) or by **exec** (performs fork and then replaces child PCB to reflect a new program, child will start execution from the first instruction). 

**Inter-Process Communication (IPC)** mechanisms allows for the process to talk to each other, still having isolated address states. It can be either accomplished by a common memory space or through a channel operating in the kernal (OS) layer.

**Swap** - moving data from memory to storage, e.g. disk

### CPU scheduler 
An OS component which process and for how long (so called **timeslice**) should be run at the moment. Switching context requires: **Preempt** - interupt and save current context; **Schedule** - run scheduler to choose the next process; **Dispatch** load and switch to the chosen context. All these operations takes time, so thier frequency should be minimised.

### Process vs Thread
Threads can share the same address space (and mapping), data and file, but have seperate stacks, stack pointer, program counter and can execute different instructions. **Multithreading**: if we run multiple threads on different CPU we can split the data to be executed by different threads in parallel (parallelisation) or have each thread execute different part of the program if these parts are independent on one another (specialization, hot cache), both which results in significant speed-up. When the threads execute **concurrently** we need synchronisation mechanisms to assure consistency in our data (since threads share memory space).  
**Mutex** - or **lock** is a way to prevent two thread from access the same data at the same time. Once the thread whats to modify the data, it has to first acquire the mutex. If the mutex is already taken the thread is blocked and has to waits until the mutex is released by the thread holding it. This can leads to **deadlock** where two threads are blocking one another.
**Wait**, **signal** and **broadcast** can be used to send signals between threads when certain condition is meet.

## Networks & Protocols
**Protocols** - an official way of doing something, rules to follow e.g. in the network communiction
**Network** - way of connecting machines and code to allow communication following rules and agreed-upon structure and procedures. An example is world wide web.
**ISP** - internet service provider
### IP - Internet Protocol (Internet layer)
Fundamental layer of protocols for internet. Messages are send in sets of samll packets and each packet has structure made of *header* and *data*. *Header* containd the meta information about the source and destination *IP address* (a numeric label assigned to each device connected to internet).  
The are two versions: *IPv6* and *IPv4* - v4 is running out of addesses, so v6 is being adopted.  
Any address starting with: 192.168.x.x or 172.16.y.y or 10.z.z.z (with some constraints) is a private IP address (used in a private netork, not existing in the public domain.
### TCP - Trasmission Control Protocol (Transport layer)
Build on top of IP to ensure that correct trasmission of the sets of packets (their number and order). It appends a header with the info about the ordering, number of packets etc. After each packet is send, an acknowledge is return to cofirm a successful tranfer of the data. It means that the TCP is reliable, but it is not very quick as it has a bit of overhead int he communication.  
A connection is established using a *handshake* - the source requesting to open the connection and the destination saying OK. Similiar exchange is done to close the connection.
### HTTP - Hyper Text Transfer Protocol (Application layer)
An abstraction build on top of TCP/IP introducing a *request-response* pattern for *client-server* interactions. *Client* is the machine requesting information (e.g. a web browser) and *server* is the machine responding with the information (e.g. a web-server). A server can be a client if it is the one sending a request.  
HTTP requests and responses have their headers as well and can be found as a key-value pairs.
### UDP - User Datagram Protocol (Transport layer)
Another internet protocol build on top of IP. Not prior communication is neede to set up a channel (no handshake), it provides checksum for data integrity and port numbers for addressing different functions a the source and at the destination. It is unreliabile as there is no guarantee of delivery, ordering pr duplicate protection, but in exchange it give little overhead in the communication, allowing for better speed of transmission. Used for video or audio transmission, in DHCP (dynamic host configuration protocol) and DNS (Domain Name System).
### DNS - Domain Name System (Application layer)
Hierarchical and decentralized naming system for computer and services connected to the Internat. It translateds the domain name (e.g. www.example.com) into a IP address (e.g. 93.184.216.34). If your DNC server does nt about the mapping, there is a higher hierarchy it can forward the request to. **Root server** - a few servers distributed across the world, that knows who should know about all the different mappings between the domain names and IP addresses.
### DHCP - Dynamic Host Configuration Protocol (Application layer)
Assignes unique IP addresses to out hosts, also provides subnet masks, default gateways and DNS address. It comes as a client and as a server.  
Once the computer is turned on, if it does not have an IP address i runs DHCP client and send a discover message looking for a DHCP server, the server returns an offer of a IP address, the computer than accepts the first offer it receives and send back a request for this address, the server responds with subnet mask, default gateway and DNS address (this is an acknowledge). The server manages all the IP address in the local net.
### Port numbers:
Different port numbers allows the server to knows which service it being requested via the TCP/IP transport layer e.g:
- 80: HTTP (for web site traffic)  
- 25: SMTP (email service)  

## Storage, Latency & Throughput
**Storage** is about holding abd retrieving data. For that we use *database* - a software layer that helps us store/retrieve (set/get, store/fetch, write/read) data.  
There are two types of storage: *disk* and *memory*. *Disk* is a persistant storage - if you turn it off or restart it, the data stored there will still be present, e.g. hard disk on laptop. *Memory* gets wiped with lost power, e.g. RAM. Memory is faster to access so we keep there often needed data relevent to the current seesion - after the session if finished the data can be lost.  
There many solutions to storage and the decision which one to choose is  based on:  
* the structure of your data  
* sort of availability needed  
* scalability  
* consistency e.g. if using distributed storage  

**Latency** is a measure of duration for an action to be completed/getting the result. Very common meaning is the *round-trip*, so the time to send a request and get a responds. Low latency mean a smooth and fast experience for the client. The latency will be impacted by the way the data is stored (array vs. a lookup table), the type of storage (memory vs. disk) and the distance between the client and the server.

**Throughput** is the maximum capacity of the system or machine, it is an amount of data that can pass through your system in a unit of time. E.g. a 512 Mbps internet connection has a throughput of 512 megabit per second.
If you recieves more requests than the amount you can serve (your throughput) than there is a *bottleneck*. *bottlenecks* put constraints on the system, which is only as fast as the slowest bottleneck, so if you want to increase your throughput, do it at the lowest bottleneck first.  
Increasing your throughput can be achieved by adding hardware (*horizontal scaling*) or increasing the capacity and performance of an existing hardware (*vertical scaling*). You can also improve your throughput by splitting the load.

## System Availability
**Resilince** - comming back from failure quickly
**Availability** - resilience of the system, ability to handle eerrors (being fault-tolerant). For the whole system to be availabile each its element has to be highly available. It is calculate as percentage of time that primary functionality is available (**uptime**) in a given time window (commercially it is annual uptime). Needed availability depends on the nature of the system: some may require almost perfect other can have lower availability during off-peak hours. As commercial systems have availability on the level of 99.99%, it is common to refer to the *number of nines* or *class of nines* e.g. %99.999 - 5 nines reliability or class 5 - this level is assumed ideal as it translates to little over 5 minuts of **downtime** per year.  
**Uptime** - time when primary functionality and operations are available.  
**Downtime** - time when the basic functionality is not available.  

**SLA (Service Level Agreements/Assurances)** - is a metric provided by service providers that guarantees certain level of service (e.g. in terms of availability). Failing to meet promised SLA entitles to compensation from the client side. 

**HA (High Availability)** - to assure HA you need to eliminate *single points of failure*, so parts in the system where one element failing is enough for the whole system to fail. Thie means introducing **redundancy**, so basically adding backups or more equipment to duplicate the weakest parts.

**RAID** - Redundant Array of Independent Disks
Data storage technique where several disks are used as a single logical unit allowing for redundancy and better performance as data is being stored over several discs. Different RAID levels means different levels of performance ans redundancy (and price). Even if one dics dies, all data is secure and you can just buy a new one and plug it in. Used often for servers.

## Caching
**Caching** is technique to speed-up the system as it reduces *latency* in the system, by keeping frequently used data in the way that allows for a quick retrival. E.g. keeping data in the memory rather than on the disk or remembering previous results in computationally intensive work (memoisation, dynamic programming). Another example would be to keep websites (especially once that are static) in **CDN - Content Delivery Network** (a network for delivering content (images, text, video, audio) in a big, distributed system) to reduce the load on the backend servers. The caching can be also done for writes but requires additional work to assure syncronisation between the cache and the storage unit. Two possible way of synchronisation in *write-through* (synchronously, updaes on write to cache) and *write-back* (write only to cache, write to storage e.g. before deleting the variable from cache).

## Proxy
**Proxy** is a server or bit of code that acts as a middle man between the clint and another server. In general proxy refers to *forward proxy*, where it substitute client in client-server communication, itf it substitute server it is called *reversed proxy*. Proxy can be use to mask the identity of the client from server (**VPNs**), distribute load on the group of backend servers or impose filtering/security. Examples of proxy is **gatekeeper**, **screener** or **load balancer**.

## Load balancing
**Load balancing** - when we have multiple servers (horizontal scaling) the load balancer is responsible for allocating (distributing) the workload in a balanced way across all the servers. To balance the workload we can do something called **round robin** - we iterate over the available servers starting with the forst one, than second, and so on, after the last one, we go back to the first and the order is reapeted. A variation would be to add weights to the server to represents that some may be more powerful than others. Another idea is to monitor the load on each of the servers and distribute work accordingly (**load-based selection**). Another way is to use hashed IP of the client to determined the server, this is useful for distinguishing servers best suited for different regions (**IP hashing based selection**). You can also have dedicated servers for different functionalities (**service nased selecetion**). Ofc it is also possible to mix different load balancing approaches in your design.   

The DHCP client will now know not the IP address of the server itself, but rather of the load balancer, which allows all the servers to have private address only known to the load balancer. So load balancer is an example of a *reverse proxy*. To keep the addresses of the servers private, the respounce for client may have only a big number that load balancer can map to the server IP - that way the client cannot access server directly, but the load balancer knows to which server this client is talking to. This is important especially if client requests are part of teh same session on the website.

**Hashing** - deterministic, returns a fixed size number - a *hash*. Small difference in input should largely influence the hash. If two different inputs gives tha same hash it is a *colision*. Each values for ahsh should be equally probable - so hash funstion should be *uniformily distributed*.  
If using hashing for load balancing we can have problems when one server dies or we add a new server. To help with that we use **consistent hashing** (a ring of hash values, the requests are match to the first clock-wise server: https://www.youtube.com/watch?v=zaRkONvyGr8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=4, each server is hashed with k different hash funcitons).

## Databases
### Relational databases
**Relational databases** - have a specific structure defined in *schema* that is impose on every *record*. Whole databse can be represented as tables (sigle table is an *entity*) with different relations between them. Every row in the table is a *record* and each column is a *attribute* or *field*. To get information from the Relational DB with can send *queries* using *SQL (Structure Query Language)*.  
- Pros: consistency, possible complecated queries, quicker read, longer write.  
- Examples: PostgreSQL, MySQL

**ACID (Atomic, Consistent, Isolation, Durable)**
ACID *transactions* (= interaction with the DB, e.g. read or write)  
**Atomic** - if transaction is more complex, if any part of the transaction will fail, the whole transation should fail - all or nothing, this keeps the data clean and not modufied by an incomplete trasaction.  
**Consistency** - each transaction moves DB from one valid statwe to another valid state, the transaction cannot corrupt the data and every read operation gets the most recent write result.  
**Isolation** - even if several transaction are happening at the same time, they will be performed sequentially  
**Durabiliyt** - stored data will remain (persist) in the database even after the loss of power or otehr failure.

### Non-relational databases
**Non-relational databases (NoSQL DB)** are more flexible tructure, not schema, the data is kept most often as key-value pairs. Fast, simple to use, but do not support complex queries. Used for caching, configuration files, sesion state or environmental variables. Can be implemented with memeory (e.g. *Memcache*) or in persistent storage (*DynomaDb*). Another popular NoSQL database is *MongoDb* that has JSON-like format. Non-relational db follows **BASE**:  
**Basically Available** - system querantees availability  
**Soft State** - state of the system may change over time even without the input.  
**Eventual Consistency** - the data will be synchronised every very small period of time, but not instantly. It is possible to get stale state of data.  

### Indexing, replication, sharding
**Indexing** - adding an artitraly table that holds records ids in order with respect to chosen attribute. Use extra spac and makes writes a bit longer, but significantly speeds up the read process. It makes sense to do it for attributes that you often search by.  

**Replication** - creating copies (replicating) the database over two or more servers. Helps with redundancy, but itroduces problem of data synchronisation: the database can ba updated synchroniously or asynchronously. Chosen tactic depends on your needs.  

**Asynchronous processing** - it doesn’t block the client i.e. browser is responsive and during the time it is waiting for the response from the server, the user can perform another operations. In **synchronous processing** the client would be blocked for the whole time it is waiting for the response.  

**Sharding** or **Particioning** - dividing the workload between servers based on some information from the client, e.g. the geographic location or first letter of the surname.  

## Leader Election
**Leader election** is used when you have several server, from which only one is responsible for somthing (a leader) and the rest of the servers should be on stand-by (followers). If the leader crashes, the followers should select a new leader among themselves and the system an fuction properly. The problem is to have all the followers agree upon the selection (their states stay in sync) - for this a **consensus algrithm** is used. These algorithms are complicated and are implemented in sotware like *Etcd* (non-relational db with high availability and strong consistency - this allows it to store a key-value pair for indicating who the leader is, that will always be the source of truth for the servers.  

## Polling, Streaming & Socket
**Polling** - client repeatably sends request to server to get an update on a certain value (it *pulls* data from a server) . We can do it every 10s / 1min or any oter iterval. This will not quarantee an instant update and therefore can be only used in some applications. If the interval is really small, polling can overload the network or server. Can be used e.g. to update temperature reading on your smart phone app.  

**Streaming** - for instantenious updates we should use streaming. To allow for this communication, a web-socket has to be opened between the client and the server. The client does not need to send any further requests, it is on stand-by or any time the data change, the server *pushes* that data into a socket and it can be received by the client. E.g. can be used for message app. Example: *Apache Kafka*  

**Socket** - a two-way dedicated channel (socket) between client and a server. It is open with a signle request and stays open until one of the machine close it or the netwrok drops.  

## Endpoint Protection / Rate limiting
We may decide to limit the rate at which client can request data from our server. The limit can be per user, IP addres or region. We can allow for 3 requests per second and at the same time 10 times per minute or impose any other limit logic. After the limit is reach, the server will imediately return an error following the next request. The rate-limiting can also protect from **DoS (Denial of Service)** attack where malicious client will ping the server to block service for other users or brake the server. A variation that is harder to detect is **DDoS (Distributed DoS)** where the malicious pings can come from different clients that are hard to link together. It is popular to use externel server e.g. *Redis service* that will hold information related to rate-limiting that very server can check after getting a request.  

## Messaging & Pub-Sub pattern
**Network partion** - fail in the network communication, some of the data may not have been delivered.
**Pub/Sub pattern** - a very popular model for communication between machines. There are four elements: *publishers*, *subscribers*, *messages* and *topics*. A machine generating data or *messages* is called a *publisher*, after a message is ready to be send, it is published to a *topic*. Different *topics* can have different category of messages published to them. Any machine interested is recieving the messages can subscribe to the topic and become a *subscriber*. When the message appears in the topic, the topic guarantees (by e.g. waiting for an acknowledment) that it will be delivered to each of the subscribers **at least once** (it can be more than once if some problem occurs). Topic also eeps the original order of messages. Because the message can be delivered more than once to the client, it should be **idempotent** - their result should be the same no matter how many times it will be performed e.g. setting a flag to True or False. Popular messaging softwares are *Apache Kafka*, *RabbitMQ*, *Google Cloud Pub/Sub*, *AWS SNS/SQS*.  

**Message Queue** - a very useful concept in system design, it is an asynchronous communications protocol. Basic architecture s as follows: there is a client called producer that creates messages and deliver them to the message queue. After ack that the message is in the queue, client can go on doing different tasks, it does not need to wait for the respose to th emessage (asynchronous). On the other side, there are servers, called consumer, that connect to the queue and gets messages to be processed. The messages stays in the queue untill consumer realises them. MQ allows for decoupling of the system and in terms for better scalability. The messages are persist (remembered even with the loss of power) until there are released from the queue.  

## Logging, monitoring, alerting
**Logging** - useful for debugging and analising performance and characteristic of the system. Log messages from around the system should be collected into log files.  
**Monitoring** - timeseries gather by your service used to analise and monitor the service, let you get insights into your service.  
**Alerting** - whenever something like latency or througput will cross an acceptable limit, an owner of the servicxe should be alerted (e.g. by sending a message on slack) that this situation occurs. This helps to prevent failures.  


**Monolith** vs **microservice** architecture:

