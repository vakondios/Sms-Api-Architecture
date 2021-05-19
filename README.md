# Sms-Api-Architecture
SMS API Architecture for SMS Services

# Requirement

As a company we have to provide to our clients an sms service. In order to terminate the sms from our clients we are connected  in four different providers that are capable of sending sms.

Every provider has predefined throughput 4sms/sec. In other words we can sent maximum 4 sms per second per provider. Our overall throughput is 16sms/sec (4 * 4).

We need to build a service that is going to implement the sms service flow.

For our clients there is no limit (throughput) for the amount of sms per second and our service must be capable to serve multiple clients at the same time.

 

Our API must have 2 calls.

 

Request POST /sendSms 

http headers :

clientId : string // unique id for each client

payload :

[

{

"from": "string" // The sender id of the message

"to" : "number" // The destination (phone number) of the message

"message" : "string" // The actual message

}, 

...

...

{

"from": "string" // The sender id of the message

"to" : "number" // The destination (phone number) of the message

"message" : "string" // The actual message

}, 

]

An array of messages that the client wants to sent.





Response

Payload

[

{

"messageId" : string // Unique id that identifies the message between client and our service.

"status"    : int   // 0 when there is no errors in request and the handling of the request otherwise the error code. if this value is not 0 then the messageId is null.

},

 

]

 

an array with results for every sms.

 

Request POST /smsStatus

http headers :

clientId : string // unique id for each client

Payload 

[

{

"messageId" : string // the messageId of the sms. The server is going to return the status of the specified message.

}

] 

 

Response

Payload

[

{

"messageId" : string ,

"status" : int 

}

]

 

Our service must respect the sms throughput to our providers.

Our service must follow load balancing logic between our clients. 

Example for load balancing logic between our clients.

Client A sends "sendSms" request for 1.000.000 messages.

Client B sends 2 sms 1 second after client A.

Our system must not send first all the sms for client A and then the sms for client B.

Must try to serve at the same time and the B client. (the same for N clients)

 

Furthermore each client can be assigned a priority value (1-3) to his sms

Sms with priority 1 must have a higher priority of 2 and 3 but not totally block priority 2 or 3 sms

Design a  versalite scheme to ensure that.

 

An SMS can be sent via provider A but also in failure of provider A for any reason the same SMS can be sent via provider B (but with lower performance or higher price). 

According to destination an SMS should be able to route via a subset of the 4 providers. 

Each provider has 2 metrics for each given destination (destination can be considered a country or a mobile operator , ie Vodafone UK) : 1) quality 1-10 ,2) price 

 

The price that the client is paying is fixed for him but it is up to us to route the sms to the destination to minimize our cost and maximize quality.

 

Design the architecture for the service described above. Create the Container level diagram and the component level diagram.

# Solution
 *** see the attached pictures of the container and component diagram
 
According to C4Model, we need to produce :
1)The Context Diagram (a very high level architecture diagram),
2)The Container Diagram (more focus - for the external and internal tools)
3)The Component Diagram (more,more focus per requirements)
4)The Code (Class Diagram), for each requirement.

On the Container.png, we have the main players according to the requirement.
On the Component-01a.png, we have the main components for the initialization of the microservice
On the Component-01b.png, we have the main components for the api -> Request POST /sendSms
On the Component-02.png, we have the main components for the api -> Request POST /smsStatus

Regards

Antonis Vakondios
