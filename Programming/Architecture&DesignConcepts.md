# System Design Interview Questions – Concepts You Should Know
Based on an article by Zubin Pratap: https://www.freecodecamp.org/news/systems-design-for-interviews/

1. [Networks & Protocols](#networks--protocols)
2. [Storage, Latency & Throughput](#storage-latency--throughput)
3. [System Availability](#system-availability)

## Networks & Protocols
**Protocols** - an official way of doing something, rules to follow e.g. in the network communiction
**Network** - way of connecting machines and code to allow communication following rules and agreed-upon structure and procedures. An example is world wide web.
### IP - Internet Protocol
Fundamental layer of protocols for internet. Messages are send in sets of samll packets and each packet has structure made of *header* and *data*. *Header* containd the meta information about the source and destination *IP address* (a numeric label assigned to each device connected to internet).  
The are two versions: *IPv6* and *IPv4* - v4 is running out of addesses, so v6 is being adopted.
### TCP - Trasmission Control Protocol
Build on top of IP to ensure that correct trasmission of the sets of packets (their number and order). It appends a header with the info about the ordering, number of packets etc.  
A connection is established using a *handshake* - the source requesting to open the connection and the destination saying OK. Similiar exchange is done to close the connection.
### HTTP - Hyper Text Transfer Protocol
An abstraction build on top of TCP/IP introducing a *request-response* pattern for *client-server* interactions. *Client* is the machine requesting information (e.g. a web browser) and *server* is the machine responding with the information (e.g. a web-server). A server can be a client if it is the one sending a request.  
HTTP requests and responses have their headers as well and can be found as a key-value pairs.

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
