# Reflection for Subscriber

- what is amqp?

AMQP stands for Advanced Message Queuing Protocol. It's a messaging protocol that enables communication between different components of a distributed application, often used for building messaging systems. It is also an open standard for passing business messages between applications or organizations. It connects systems, feeds business processes with the information they need and reliably transmits onward the instructions that achieve their goals. 

- what it means? guest:guest@localhost:5672 , what is the first quest, and what is the second guest, and what is localhost:5672 is for? 

The string "guest:guest@localhost:5672" is essentially a URI-like syntax used to represent the connection details for the AMQP (Advanced Message Queuing Protocol) broker. "guest:guest" represents the username and password used for authentication when connecting to the message broker. In many cases, especially in local development environments, the default username and password for RabbitMQ are both "guest". So, in this case, "guest:guest" means the username and password are both "guest". Conversely, "localhost:5672" represents the address and port of the message broker. "localhost" refers to the local machine, and and "5672" is the default port number used by RabbitMQ for AMQP connections. So, "localhost:5672" specifies that the message broker is expected to be running on the same machine where this code is executed, and it should be accessible via port 5672.

-------------------------------------------------------------------------------------------------------------
# Screenshots

## Simulation slow subscriber
![Screenshot 2024-04-23 091133.png](assets%2FScreenshot%202024-04-23%20091133.png)
![Screenshot 2024-04-23 091219.png](assets%2FScreenshot%202024-04-23%20091219.png)

The total number of queue listed in RabbitMQ is approximately 21. This is because `cargo run` was executed 5 times and requires a total of 25 seconds of runtime (5 messages per run x 5 times running `cargo run`). However, since I executed `cargo run` in a sequential manner, there is a delay that occurs that causes RabbitMQ to only capture a maximum queue of 21 in a single time frame. The delay is caused by the line `thread::sleep(ten_millis);` that causes the subscriber program to "sleep" for 10 milliseconds. This essentially simulates a slow subscriber, and thus causes the delay to happen to initiate a maximum queue of 21 within a single time period.

## Reflection and Running at least three subscribers
![Screenshot 2024-04-23 093232.png](assets%2FScreenshot%202024-04-23%20093232.png)
![Screenshot 2024-04-23 093321.png](assets%2FScreenshot%202024-04-23%20093321.png)

As we can observe from the chart, the total number of queue became 13 as it was previously 21 when running 3 subscribers consecutively and running 5 `cargo run` sequentially in the publisher. This is becase the subscriber initiates multithreading since there are multiple messages being sent/received at the same time in handling the communication event that is being sent by the publisher. This further implies that the messages are being sent in parallel to one another across 3 different subscribers. Furhermore, multithreading here also improves the performance of the messages being communicated between the subscriber and publisher across a certain time period. This is multithreading makes use of multiple threads in a single process that is used to communicate between the subscriber and publisher.

Some improvements to the code that can be done is that we can conduct/add parallelism to the publisher's code. This helps sending many requests at a time, and allows it to simulate a higher traffic comprising of multiple messages in a more accurate manner.


