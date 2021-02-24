# Module-7

## Socket

A socket a kind of endpoint. An endpoint is a point where the data is available from and where the data may be sent to.

### BSD Socket 

BSD\( _Berkeley Software Distribution_\) socket is the first universal API suitable for implementation in nearly all operating systems. 

 After some amendments, the standard was adopted by POSIX as **POSIX sockets**.

### Socket Domain

* **UNIX Domain** : a part of the BSD sockets’ API used to communicate with processes working within one operating system
* **INET Domain** : a part of the BSD sockets’ API used to communicate with processes working within different computer systems, connected together using a TCP/IP network

{% hint style="info" %}
UNIX domain work within the same computer, and INET domain doesn't.
{% endhint %}

### Socket Address

INET domain sockets are **identified** \(**addressed**\) by a pair of values:

* the **IP address** of the computer system inside which the socked is located;
* the **port number** \(more often referred to as the **service number**\)

{% tabs %}
{% tab title="IP Address" %}
### IP Address

 The **IP address** \(more precisely: **IP4 address**\) is a **32-bit-long value** used to identify computers connected to any TCP/IP network. The value is usually presented as four numbers within the range 0..255, coupled together with dots \(e.g. 87.98.239.87\).

There is also a newer IP standard, named **IP6**, which uses 128 bits for the same purpose.
{% endtab %}

{% tab title="Port Number" %}
### Port/Service Number

A **socket/service number** is a **16-bit-long integer number** identifying a socket within a particular system.
{% endtab %}
{% endtabs %}

### Network Order

The **network order** is a way in which all multiple-byte data is **ordered** by network services.

Due to historical reasons, the **big-endian** order is used in TCP/IP networks. For the same reason, the term _byte_ is not used in standardization documents, and it is replaced by the word _**octet**_.

### Protocol

 A **protocol** is a standardized **set of rules** allowing processes to communicate with each other.

### Protocol Stack

 A **protocol stack** is a multilayer **set of cooperating protocols** providing a unified repertoire of services.

{% hint style="warning" %}
Apatoto jani na
{% endhint %}

{% tabs %}
{% tab title="IP" %}
### IP \(Internet Protocol\)

The **IP** \(_Internetwork Protocol_\) is one of the lowest part of the TCP/IP protocol stack. Its functionality is very simple – it sends a packet of data \(a **datagram**\) between two network nodes.

IP is a very unreliable protocol. It doesn’t guarantee that:

* any of the sent datagrams **will reach their targets** \(moreover, if any of the datagrams is lost, it may remain undetected\)
* the datagram will reach the target **intact**;
* a pair of sent datagrams will reach the target **in the same order** as they were sent.

However, the upper layers are able to compensate for all of these IP’s infirmities.
{% endtab %}

{% tab title="TCP" %}
### TCP \(Transmission Control Protocol\)

The **TCP** \(_Transmission Control Protocol_\) is the **highest** part of the TCP/IP protocol stack. It uses datagrams \(provided by the lower layers\) and handshakes them \(an automated process of synchronizing the flow of data\) to construct a reliable communication channel, able to transmit and receive single characters.

Its functionality is very complex, as it guarantees that:

* a stream of data **reaches the target**, or the sender is informed that communication has failed;
* the data **reaches the target intact**
{% endtab %}

{% tab title="UDP" %}
### UDP \(User Datagram Protocol\)

The **UDP** \(_User Datagram Protocol_\) lies at the **higher** part of the TCP/IP protocol stack, but lower than TCP. It doesn’t use handshakes, which have two serious consequences:

* it’s **faster** than TCP \(due to lower overheads\)
* it’s **less reliable** than TCP.

This means that:

* TCP is the first-choice protocol for applications where **data safety is more important that efficiency** \(e.g. WWW, mail transfer, etc.\)
* UDP is more adequate for applications where **response time is crucial** \(DNS, DHCP, etc.\)
{% endtab %}
{% endtabs %}

### Connections

{% tabs %}
{% tab title="Connection Oriented Communication" %}
### Connection Oriented Communication

 A form of communication which demands **some preliminary steps** to establish the connection and other steps to finish it is called **connection-oriented communication**. Both sides of the communication are aware that the other part is connected. Connection-oriented communication is usually built on top of **TCP**.

_Example_ : A phone call. The rules are strictly defined. The caller has to dial callee's number and wait until the callee receive the call. The actual communication doesn't start until the previous steps are completed successfully. The communication ends when any of parties hang up.

TCP/IP networks use the following names for both sides of the communication:

* the side that initiates the connection \(caller\) is named the **client**;
* the side that answers the client \(callee\) is named the **server**.
{% endtab %}

{% tab title="Connectionless Communia" %}
### Connectionless Communication

Communication which can be established _ad-hoc_ \(when necessary\) is called **connectionless communication**. Both parties usually have equal rights, but neither is aware of the other side’s state.

_Example_ : Walkie-talkies.  Either of the parties may **initiate communication at any time.** It only involves pushing the _talk_ button. Talking to the mic doesn’t guarantee that anybody hears it \(you have to wait for an incoming answer to be sure\)
{% endtab %}
{% endtabs %}

## Main coding part: Socket in C

Our program has to perform the following steps →

1. **create a new socket** able to handle **connection-oriented transmission based on TCP**;
2. **connect the socket** to the HTTP server of a given address;
3. **send a request** to the server \(the server wants to know what we want from it\)
4. **receive the server’s response** \(it will contain the requested _root document_ of the site\)
5. **close the socket** \(end the connection\)

```c
socket = make_a_new_socket();
connect_to_server(socket,server);
write(socket,request);
read(socket,response);
close(socket); 
```

{% hint style="info" %}
To create a socket, we need to know the **number of protocols** it will use to communicate.
{% endhint %}

### Protocol File

Every operating system has a text file named protocols, containing a list of known protocols along with their numbers. The file is located:

* in the /etc/ directory \(Unix/Linux\)
* in the C:\Windows\System32\drivers\etc directory \(MS Windows\)

The file consists of columns containing, in order:

1. the official protocol **name**,
2. the protocol **number**,
3. the protocol name’s **alias** or **aliases**,
4. a **comment** \(optional, starting with \#\)



### The function getprotobyname\(\)

The `getprotobyname()` function → gets only one argument – the **name of the protocol**.

It **returns a pointer** to the structure filled with the protocol’s data, or NULL if there’s no such protocol in the protocols file.

{% hint style="danger" %}
Note: the function stores the structure inside its own **static** data. This means that the function is **not reentrant** and consequently is **not thread-safe** – any subsequent invocation will destroy the previous result.
{% endhint %}

```c
#include <netdb.h>    /* UNIX/LINUX */
#include <Winsock2.h> /* MS Windows  */

struct protoent *getprotobyname(char *name);
```

```c
struct protoent{
    char  *p_name;    /* official protocol name */
    char **p_aliases; /* alias list             */
    int    p_proto;   /* protocol number        */
};
```

### Example: Here’s a simple \(and not very safe\) example of using the getprotobynumber\(\) function →

```c
#include <stdio.h>
#ifdef _MSC_VER
	#include <Winsock2.h>
	#pragma comment(lib, "ws2_32.lib")
#else
	#include <netdb.h>
#endif

int main(void) {

#ifdef _MSC_VER
	WSADATA wsa;
	WSAStartup(0x0202, &wsa);
#endif
	printf("%d\n", getprotobyname("tcp")->p_proto);
#ifdef _MSC_VER
	WSACleanup();
#endif
	return 0;
}

```

{% hint style="danger" %}
Code kaj e kore ni 😑😑😑 
{% endhint %}

### Creating Socket

To **create a new socket**, we use the socket\(\) function →

```c
#include <netdb.h>    /* UNIX/LINUX */
#include <Winsock2.h> /* MS Windows  */

int socket(int domain, int type, int protocol);
```

The function takes three arguments:

* a **domain code** \(we may use the AF\_INET symbol here to specify the Internet socket domain\)
* a **socket type code** \(we may use the SOCK\_STREAM symbol here to specify the high-level socket able to act as a _character device_ \(a character device that can handle single characters, e.g., a terminal is a character device while a disk isn’t\)
* a **protocol number**.

The function returns a **newly created socket descriptor**, or -1 on error.

{% hint style="warning" %}
Note: the descriptor may be used like a _file descriptor_ in Unix/Linux \(e.g. it may be passed as an argument to functions like read\(\), write\(\) or close\(\)\), but such use is prohibited in MS Windows \(e.g. MS Windows has a dedicated function named closesocket\(\) to close the opened socket\).
{% endhint %}

### The function gethostbyname\(\)

The `gethostbyname()` function → gets only one argument – the name of the host.

It returns a **pointer to the structure** filled with the host data, or NULL if there is no host of that name.

{% hint style="danger" %}
Note: the function is **not thread-safe**.
{% endhint %}

```c
#include <netdb.h>    /* UNIX/LINUX */
#include <Winsock2.h> /* MS Windows  */

struct hostent *gethostbyname(const char *name);
```

```c
struct hostent{
    char  *h_name;       /* official name of host */
    char **h_aliases;    /* alias list            */
    int    h_addrtype;   /* host address type     */
    int    h_length;     /* lenghth of address    */
    char **h_addr_list;  /* list of addresses     */
};
```

### The Function inet\_ntoa\(\)

The `inet_ntoa()` function → gets only one argument – an **internal form of an IP address** \(four octets\).

The name of the function comes from _INET numeric to ASCII_.

It returns a pointer to a string containing a **human-readable \(dotted\) form of the specified IP address**.

{% hint style="danger" %}
Note: the function is **not thread-safe**.
{% endhint %}

```c
#include <arpa/inet.h> /* UNIX/LINUX */
#include <Winsock2.h>  /* MS Windows  */

char *inet_ntoa(struct in_addr in);
```

```c
struct in_addr{
    unsigned long s_addr;
};
```

