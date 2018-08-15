# JMS
Java Messaging Service

In this tutorial,you can find complete information about JMS in quick time. This is designed in very simple manner to understand and by end of this, you will gain knowledge about JMS and way to work with it. Yehhh, Let's Begin..!

Java message service, or simply JMS, is a medium which sends messages to two or more clients.

This message-oriented middleware has two models i.e JMS Messgaing service has two models.

- **Point-to-point or queue model**

     This model is point point model. As the name says message will be delivered between one to one. Its simple.

- **Publish or subscribe or topic model**

  The producer submits messages to the queue, and recipients can browse the queue and decide which messages they wish to receive

| Queue Model | Topic Model |
| ------------- | ------------- |
| One sender and one receiver. | One sender but multiple subscribers. |
| This model is asynchronous since it's one to one model. | This model is synchronous. |
| Receiver gets the sent message. | Subscribers will receive their  messages.Consumers can subscribe to a particular topic, or channel, and receive all messages within the chosen topic.
| Messages have to be delivered in the order sent. | There is no guarantee messages have to be delivered in the order sent.| 
| A JMS queue only guarantees that each message is processed only once.| There is no guarantees that each message is processed only once.
| The Queue knows who the consumer or the JMS client is. The destination is known. | The Topic, have multiple subscribers and there is a chance that the topic does not know all the subscribers. The destination is unknown. | 
| The JMS client (the consumer) does not have to be  active or connected to the queue all the time to receive or read the message. | The subscriber / JMS client needs to the active when the messages are produced by the producer, unless the subscription was a durable subscription. |
| Every message successfully processed is acknowledged by the consumer. | Every message successfully processed is not acknowledged by the consumer/subscriber.|

### Coding both models

Below, you can find the code for creating Queue and topic model 

#### Queue Model
 
Publisher:

    //1) create JNDI Lookup
    InitialContext ctx=new InitialContext();  
    QueueConnectionFactory f=(QueueConnectionFactory)ctx.lookup("myQueueConnectionFactory");  
    QueueConnection con=f.createQueueConnection();  
    con.start();  
    //2) create queue session  
    QueueSession ses=con.createQueueSession(false, Session.AUTO_ACKNOWLEDGE);  
    //3) get the Queue object  
    Queue t=(Queue)ctx.lookup("myQueue");  
    //4)create QueueSender object         
    QueueSender sender=ses.createSender(t);  
    //5) create TextMessage object  
    TextMessage msg=ses.createTextMessage();     
    //6) Setting message 
    msg.setText(s);  
    //7) Sending message
    sender.send(msg); 
    //8) closing connection
    con.close();


Subscriber:

    //1) create JNDI Lookup
    InitialContext ctx=new InitialContext();  
    QueueConnectionFactory f=(QueueConnectionFactory)ctx.lookup("myQueueConnectionFactory");  
    QueueConnection con=f.createQueueConnection();  
    con.start();  
    //2) create queue session  
    QueueSession ses=con.createQueueSession(false, Session.AUTO_ACKNOWLEDGE);  
    //3) get the Queue object  
    Queue t=(Queue)ctx.lookup("myQueue");  
    //4)create QueueReceiver object         
    QueueReceiver receiver =ses.createReceiver(t);  
     //5) create listener object  
    MyListener listener=new MyListener();     
    //6)  register the listener object with receiver  
    receiver.setMessageListener(listener);


MyListener :

    import javax.jms.*;  
    public class MyListener implements MessageListener {  
          public void onMessage(Message m) {  
            try{  
            TextMessage msg=(TextMessage)m;  
            System.out.println("following message is received:"+msg.getText());  
            }catch(JMSException e){System.out.println(e);}  
        }  
    }  
 

#### Topic Model

Publisher :

    TopicConnectionFactory f=(TopicConnectionFactory)ctx.lookup("myTopicConnectionFactory");  
    TopicConnection con=f.createTopicConnection();
    TopicSession ses=con.createTopicSession(false, Session.AUTO_ACKNOWLEDGE);  
    Topic t=(Topic)ctx.lookup("myTopic");  
    TopicPublisher publisher=ses.createPublisher(t);  
    TextMessage msg=ses.createTextMessage();  
    msg.setText(s); 
    publisher.publish(msg);  
    con.close();
 
 
Subscriber :

    TopicConnectionFactory f=(TopicConnectionFactory)ctx.lookup("myTopicConnectionFactory");  
    TopicConnection con=f.createTopicConnection();
    TopicSession ses=con.createTopicSession(false, Session.AUTO_ACKNOWLEDGE);  
    Topic t=(Topic)ctx.lookup("myTopic");  
    TopicSubscriber publisher=ses.createSubscriber(t);  
    MyListener listener=new MyListener(); 
          //  register the listener object with subscriber
    receiver.setMessageListener(listener);
    publisher.publish(msg);  
    con.close();
 
MyListener

    import javax.jms.*;  
    public class MyListener implements MessageListener {  
        public void onMessage(Message m) {  
            try{  
                TextMessage msg=(TextMessage)m;  
                System.out.println("following message is received:"+msg.getText());  
             }catch(JMSException e)
             {
               System.out.println(e);
             }  
        }  
    }  
 

### References :

http://integrationspot.blogspot.in/2011/03/jms-queue-difference-between-queue-and.html

https://examples.javacodegeeks.com/enterprise-java/jms/jms-queue-example/

https://www.javatpoint.com/jms-tutorial

