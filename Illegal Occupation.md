# Illegal Occupation

**Description:** Two client with the same Container-Id are not allowed to connect to the broker. When we send **two OPEN packet with same the Container-Id**, the broker will return error and the client will close the TCP connection. The client with this Container-Id will **never be able to connect with the broker** unless the broker resets. This vulnerability can be exploited by the adversary to perform the aforementioned attacks on many Container-Id to make a huge set of clients unable to connect with the broker. As the ActiveMQ are widely adopted by the IoT vendors, this can be a vulnerability affected a wide range.



Following are the details.

1. download our test scripts in  https://github.com/wqqqy/MPInspector/tree/master/Adapter/AMQP10/hack-amqp10_SameContainerId 

2. using following command

   `node index.js`

3. We offered the GUI in http://127.0.0.0:9126

   - First load the configuration by visiting 

   http://127.0.0.0:9126/amqp/init?filename=platforms/example.yml

   - Then we send **two OPEN packets with the same Container-Id 1** by click these two button

     `from init to open`

     `/amqp/send/open send` 

   - Reset the script

   - Try to connect to the server again by click the button

     `from init to open`

     

We can learn from the log A that in the broker side the broker returned close packets and the client closed this TCP connection with the broker after we send **two OPEN packets with the same Container-Id 1**

Then we build a new client to connect with the broker using the same Container-Id 1, we can learn from the log B in the attached picture that the broker returned errors as the broker believe the client with Container-Id 1 already connected.

 ![1590234052205.png](https://issues.apache.org/jira/secure/attachment/13003820/1590234052205.png) 

**Suggestion for repair:** May be the state of the broker after received two OPEN packets could be checked and the connection state of the client could be updated when the TCP connection is closed.



