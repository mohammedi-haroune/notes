# Scala

## How to slow down an akka stream ?

```scala
//send messages over tcp one message per second in order to make sure Tcp connection is closed after sending
//a message. Note that is a requirement from collectorApp
Source(messages)
  .throttle(1, 1 seconds, 1, ThrottleMode.Shaping)
  .runWith(tcpSink)
```

## How akka Tcp Work ?

All of the Akka I/O APIs are accessed through manager objects. When using an I/O API, the first step is to acquire a reference to the appropriate manager. The code below shows how to acquire a reference to the`Tcp`manager.

```Scala
import akka.io.{ IO, Tcp }
import context.system // implicitly used by IO(Tcp)

val manager = IO(Tcp)
```

The manager is an actor that handles the underlying low level I/O resources \(selectors, channels\) and instantiates workers for specific tasks, such as listening to incoming connections.





