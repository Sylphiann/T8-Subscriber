# Subsciber App
Favian Naufal - **2006597802**


## Reflection
**Q** : What is **amqp**?

**A** : It stands for the Advanced Message Business Protocol, an open standard for passing business messages between applications or organizations, which we are currently using for this module. There are several things this protocol provides us with; it connects systems, feeds business processes with the information they need, and reliably transmits onward the instructions that achieve their goals. <br>
As it was stated within its official website on: https://www.amqp.org/about/what

Within this tutorial, and in the case for `main.rs` file, 

---
**Q** : What does `guest:guest@localhost:5672` means? What is the first **guest**? What is the second **guest**? And what is **localhost:5672** for?

**A** : Let's dissect the whole line first:
```
let listener = CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
```
It was shown that it's an initialization of an immutable variable `listener` with a value of a QueueListener object instance which is provided within the method `new_queue_listener` from the `CrosstownBus` implementation ([source](https://docs.rs/crosstown_bus/0.5.3/crosstown_bus/struct.CrosstownBus.html#method.new_queue_listener)). 

The method `new_queue_listener` then would return a listener instance that would receive messages and send them to a queue. This can be done by providing an url as a parameter to said method:
```
new_queue_listener(<url>: String)
```
Just like what we are doing in this module, with the string:
```
"amqp://guest:guest@localhost:5672"
```
With the following explanation on each part of the URL string:
- the URL provided above shows that it uses an AMQP Protocol for the connection, with `amqp://`.
- A part of the credentials provided for the connection is shown with the first `guest`, that is the password that will be passed to the url.
- The second `guest` is the other credential that is being passed which is the username.
- And `localhost:5672` is what we would specify the host where the AMQP server is running, followed with the port number where the AMQP server listens. In this case, using localhost would mean that the server would be in the same machine with the client, while using the port `5672` which is the default standard port for non-encrypted AMQP connections.

Then the literal string provided would be converted into a `String` object with the method `.to_owned()`, and the object returned by the `new_queue_listener` method then would later be followed with the method `.unwrap()` which assumed to handle possible failures upon calling previous method. 

---
### Simulation of slow subscriber
![Simulation of slow subscriber](image.png)

The number of queues shown above was 6, as it meant there are 6 messages left to be processed by the subscriber as it were waiting in the queue listener.

---
### Running at least three subscribers
![alt text](image-1.png)

![alt text](image-2.png)

In the RabbitMQ dashboard shown above, it shows that there were 3 channels and connections connected to the the system which means that there really were 3 subscribers listening for the barrages of messages sent by the publisher. The queue of messages were cleared faster than running a single subscriber, as all the messages were equally shared and processed between those three running subscribers application. A possible improvement would be to run multiple queue listener that runs asynchronously within the running subscriber to process the incoming messages faster. 