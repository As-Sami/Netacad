# Module-7

### Socket

**A** **socket** **a kind of endpoint**. **An endpoint is a point where the data is available from and where the data may be sent to.**

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

