# Module-7

### Socket

A socket a kind of endpoint. An endpoint is a point where the data is available from and where the data may be sent to.

### BSD Socket 

BSD\( _Berkeley Software Distribution_\) socket is the first universal API suitable for implementation in nearly all operating systems. 

 After some amendments, the standard was adopted by POSIX as **POSIX sockets**.

### Socket Domain

* **UNIX Domain** : a part of the BSD sockets’ API used to communicate with processes working within one operating system
* **INET Domain** : a part of the BSD sockets’ API used to communicate with processes working within different computer systems, connected together using a TCP/IP network

> UNIX domain work within the same computer, and INET domain doesn't.

### Socket Address

INET domain sockets are **identified** \(**addressed**\) by a pair of values:

* the **IP address** of the computer system inside which the socked is located;
* the **port number** \(more often referred to as the **service number**\)

### IP Address

 The **IP address** \(more precisely: **IP4 address**\) is a **32-bit-long value** used to identify computers connected to any TCP/IP network. The value is usually presented as four numbers within the range 0..255, coupled together with dots \(e.g. 87.98.239.87\).

There is also a newer IP standard, named **IP6**, which uses 128 bits for the same purpose.

### Port/Service Number

A **socket/service number** is a **16-bit-long integer number** identifying a socket within a particular system.

### Network Order

The **network order** is a way in which all multiple-byte data is **ordered** by network services.

Due to historical reasons, the **big-endian** order is used in TCP/IP networks. For the same reason, the term _byte_ is not used in standardization documents, and it is replaced by the word _**octet**_.

### Protocol

 A **protocol** is a standardized **set of rules** allowing processes to communicate with each other.

### Protocol Stack

 A **protocol stack** is a multilayer **set of cooperating protocols** providing a unified repertoire of services.

> Apatoto jani na

### IP \(Internet Protocol\)

The **IP** \(_Internetwork Protocol_\) is one of the lowest part of the TCP/IP protocol stack. Its functionality is very simple – it sends a packet of data \(a **datagram**\) between two network nodes.

IP is a very unreliable protocol. It doesn’t guarantee that:

* any of the sent datagrams **will reach their targets** \(moreover, if any of the datagrams is lost, it may remain undetected\)
* the datagram will reach the target **intact**;
* a pair of sent datagrams will reach the target **in the same order** as they were sent.

However, the upper layers are able to compensate for all of these IP’s infirmities.

### TCP \(Transmission Control Protocol\)



The **TCP** \(_Transmission Control Protocol_\) is the **highest** part of the TCP/IP protocol stack. It uses datagrams \(provided by the lower layers\) and handshakes them \(an automated process of synchronizing the flow of data\) to construct a reliable communication channel, able to transmit and receive single characters.

Its functionality is very complex, as it guarantees that:

* a stream of data **reaches the target**, or the sender is informed that communication has failed;
* the data **reaches the target intact**

### UDP \(User Datagram Protocol\)

The **UDP** \(_User Datagram Protocol_\) lies at the **higher** part of the TCP/IP protocol stack, but lower than TCP. It doesn’t use handshakes, which have two serious consequences:

* it’s **faster** than TCP \(due to lower overheads\)
* it’s **less reliable** than TCP.

This means that:

* TCP is the first-choice protocol for applications where **data safety is more important that efficiency** \(e.g. WWW, mail transfer, etc.\)
* UDP is more adequate for applications where **response time is crucial** \(DNS, DHCP, etc.\)

### Connection Oriented Communication

 A form of communication which demands **some preliminary steps** to establish the connection and other steps to finish it is called **connection-oriented communication**. Both sides of the communication are aware that the other part is connected. Connection-oriented communication is usually built on top of **TCP**.

_Example_ : A phone call. The rules are strictly defined. The caller has to dial callee's number and wait until the callee receive the call. The actual communication doesn't start until the previous steps are completed successfully. The communication ends when any of parties hang up.

TCP/IP networks use the following names for both sides of the communication:

* the side that initiates the connection \(caller\) is named the **client**;
* the side that answers the client \(callee\) is named the **server**.

### Connectionless Communication

Communication which can be established _ad-hoc_ \(when necessary\) is called **connectionless communication**. Both parties usually have equal rights, but neither is aware of the other side’s state.

_Example_ : Walkie-talkies.  Either of the parties may **initiate communication at any time.** It only involves pushing the _talk_ button. Talking to the mic doesn’t guarantee that anybody hears it \(you have to wait for an incoming answer to be sure\)



