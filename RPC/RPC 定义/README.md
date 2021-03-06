# RPC 定义

远程过程调用范例可以在较高级别上定义为一组两个通过网络连接的通信端点，其中一个端点发送请求，另一个端点根据该请求生成响应。简单来说，这是一个请求-响应范例，其中两个端点/主机具有不同的地址空间。请求远程过程的端点可以称为调用方，对此作出响应的端点可以称为被调用方。RPC 中的端点可以是客户端和服务器，对等网络中的两个节点，网格计算系统中的两个主机，甚至两个微服务。RPC 通信不仅限于两个主机，还可以涉及多个主机或端点（Bergstrom 和 Pandey，2007）。

![Remote Procedure Call.](https://s1.ax1x.com/2020/03/29/GZ78nP.png)

最简单的 RPC 实现如上图所示。在这种情况下，客户端（或调用者）和服务器（或被调用者）由物理网络分开。系统的主要组件是客户端例程/程序，客户端存根，服务器例程/程序，服务器存根和网络例程。存根是一个小程序，通常用作较大程序的替代品（或接口）（Rouse，n.d。）。客户端存根向客户端例程公开服务器例程提供的功能，而服务器存根向服务器例程提供类似客户端的程序（Taing，n.d。）。客户端存根从客户端程序获取输入参数并返回结果，而服务器存根向服务器程序提供输入参数并获取结果。客户端程序只能与提供远程服务器到客户端接口的客户端存根进行交互。此存根还会序列化由客户端例程发送到存根的输入参数。同样，服务器存根提供到服务器例程的客户端接口，以及处理发送到客户端的数据的序列化。

客户端例程执行远程过程时，它将调用客户端存根，该存根将输入参数序列化。使用 OS 网络例程（TCP / IP）（Taing，n.d.），将此序列化的数据发送到服务器。然后，数据由服务器存根反序列化，并使用给定的参数提供给服务器例程。服务器例程的返回值将再次进行序列化，并通过网络发送回客户端，在此由客户端存根反序列化，并提供给客户端例程。通常，此远程过程对于客户端例程是隐藏的，对于客户端而言它是本地过程。RPC 服务还需要发现服务/主机解析机制来引导客户端和服务器之间的通信。RPC 的一个重要功能是所有端点的地址空间不同（Birrell＆Nelson，1984），但是，将位置传递到全局存储（Amazon S3，Microsoft Azure，Google Cloud Store）并不是不可能的。在 RPC 中，所有主机都有单独的地址空间。他们无法共享指向一台主机中某个内存位置的指针或引用。这种地址空间隔离意味着所有信息都在主机之间的消息中传递，这些主机以值（对象或变量）的形式进行通信，而不是通过引用进行通信。由于 RPC 是远程过程调用，因此发送到远程主机的值不能是指向本地内存的指针或引用。但是，将链接传递到全局共享内存位置并不是不可能的，而是取决于系统的类型。

最初，RPC 是作为一种同步请求-响应机制开发的，它与特定的编程语言实现绑定在一起，并具有用于将计算外包的自定义网络协议（Birrell＆Nelson，1984）。它有一个注册系统来注册所有服务器。最早的基于 RPC 的系统之一（Birrell＆Nelson，1984）是在 1980 年代初期以 Cedar 编程语言实现的。该系统的目标是为本地过程调用提供类似的编程语义。它是为具有低效率网络协议和用于使用所述网络协议传输信息的串行化方案的 LAN 网络而开发的，该系统旨在在远程地址空间中执行过程（也称为方法或功能）。单线程同步客户端和服务器随附一个注册表系统，服务器使用该注册表系统来绑定（或注册）其过程。客户端使用此注册表系统来查找要在其上执行其远程过程的特定服务器。这个 RPC 的实现（Birrell＆Nelson，1984）有一个非常特定的用例。它是专门为在“施乐研究互联网络”之间进行外包计算而构建的，“施乐研究互联网络”是一个小型的封闭的以太网网络，具有 16 位地址（Birrell＆Nelson，1984）。

现代的 RPC 系统是与语言无关的，异步的，负载均衡的系统。已根据需要添加了对这些系统的身份验证和授权以及其他安全功能。这些系统中的大多数都将故障处理作为模块内置在其中，并且这些系统通常遍布整个 Internet。RPC 程序跨网络（或通信通道）运行，因此它们需要处理远程错误并能够成功传递信息。错误处理通常会有所不同，可分为远程主机或网络故障处理。根据系统的类型和错误，调用者（或被调用者）将返回错误，并且可以相应地处理这些错误。对于异步 RPC 调用，可以指定事件以确保进度。

RPC 实现在基础通信协议（传统上是基于 IP 的 TCP）之上使用序列化（也称为编组或酸洗）方案。这些序列化方案使呼叫者和被叫者都变得与语言无关，从而可以并行开发这两个系统而没有任何语言限制。序列化方案的一些示例是 JSON，XML 或协议缓冲区。现代 RPC 系统允许彼此独立开发大型系统的不同组件。语言不可知的本质与系统某些部分的解耦相结合，使这两个组件（主叫方和被叫方）可以分别扩展并添加新功能。系统的这种独立扩展可能会导致相互促进的相互连接的 RPC 服务的网格。

## 无法定义的 RPC

实际上，我们现在很难去定义或者区分 RPC 与非 RPC 的边界。在分布式系统世界中，系统的每个单独组件（无论是硬盘，多核处理器还是微服务）都是 RPC 的扩展，因此很难对 RPC 范式进行具体定义 。因此，任何与请求-响应机制松散关联的东西都可以视为 RPC。RPC 范例在请求-响应机制中表现最为突出。Futures & Promises 也出现在 RPC 的新品种中。这就引发了一个疑问，即每个请求-响应系统是否都是 RPC 范例的修改实现，还是实际上给表带来了新的东西？这些现代的通信协议，例如 HTTP 和 REST，可能只是 RPC 的另一种形式。在 HTTP 中，客户端请求网页（或其他内容），然后服务器以所需的内容进行响应。这种通信的动态可能与传统的 RPC 稍有不同，但是，HTTP 无状态服务器遵循 RPC 范例背后的大多数概念。同样，请考虑向您喜欢的 Google API 发送请求。假设您要使用其反向地理编码 API 将纬度/经度转换为地址，或者想使用其 Places API 在附近找到一家不错的餐厅，您将向其服务器发送请求以执行以下操作：将使用一些输入参数（例如坐标）并返回结果。尽管这些 API 遵循 RESTful 设计，但它似乎是 RPC 范例的扩展。

RPC 范式随着时间的流逝而发展。目前，将 RPC 与非 RPC 进行区分变得非常困难。随着时间的流逝，RPC 的局限性和局限性不断发展。当前的 RPC 实现甚至还支持服务器从客户端请求信息以响应这些请求，反之亦然（双向性）。RPC 的这种双向特性已经使 RPC 从简单的客户端-服务器模型过渡到了一组相互通信的端点。

在过去的四十年中，研究人员和行业领导者一直在努力提出他们对 RPC 的定义。RPC 范式的支持者将每次请求-响应通信都视为 RPC 范式的一种实现，而反对 RPC 的人则试图明确列举 RPC 的局限性。但是，随着时间的推移，随着新的 RPC 模型的引入，这些限制似乎逐渐消失。RPC 支持者将其视为分布式系统的圣杯。他们将其视为现代分布式通信的基础。从 Apache Thrift 和 ONC 到 HTTP 和 REST，他们都将其倡导为 RPC，而 REST 开发人员强烈反对 RPC。

而且，利用现代的全局存储机制，对 RPC 系统具有单独的地址空间的需求似乎正在慢慢消失并逐渐消失。因此，问题仍然是什么是 RPC，什么不是 RPC？这是一个开放式的问题。关于 RPC 的外观并没有达成一致的共识，只是它在两个端点之间具有通信。

# RPC 案例

在现代系统中，RPC 已变得非常重要，Google 甚至每秒执行 10^10 个 RPC 调用的命令，每秒有数十万亿次 RPC 调用。在最简单的 RPC 系统中，客户端通过网络连接连接到服务器并执行过程。该过程可以很简单，就像用您喜欢的编程语言返回“ Hello World”一样。但是，此远程过程的复杂性没有上限。

```py
from xmlrpc.server import SimpleXMLRPCServer

# a simple RPC function that returns "Hello World!"
def remote_procedure():
    return "Hello World!"

server = SimpleXMLRPCServer(("localhost", 8080))
print("RPC Server listening on port 8080...")
server.register_function(remote_procedure, "remote_procedure")
server.serve_forever()
```

下面是用 Python 3 编写的上述服务器的简单 RPC 客户端的代码。

```py
import xmlrpc.client

with xmlrpc.client.ServerProxy("http://localhost:8080/") as proxy:
    print(proxy.remote_procedure())
```

在上面的示例中，我们创建了一个名为 remote_procedure 的简单函数，并将其绑定到 localhost 上的端口 8080。然后，RPC 客户端连接到服务器，并在没有输入参数的情况下请求 remote_procedure。然后，服务器以执行 remote_procedure 返回的返回值作为响应。甚至可以将三向握手视为 RPC 范例的一个示例。三向握手最常用于建立 TCP 连接。在这里，服务器端应用程序绑定到服务器上的端口，并向 DNS 服务器添加了主机名解析条目（可以看作是 RPC 中的注册表）。现在，当客户端必须连接到服务器时，它请求 DNS 服务器将主机名解析为 IP 地址，然后客户端发送 SYN 数据包。该 SYN 数据包可以看作是对另一个地址空间的请求。服务器收到此消息后，将返回 SYN-ACK 数据包。来自服务器的此 SYN-ACK 数据包可被视为来自服务器的响应以及建立连接的请求。然后，客户端以 ACK 数据包响应。
