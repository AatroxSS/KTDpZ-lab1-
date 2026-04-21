```mermaid
componentDiagram
    title Part 1 - Message Delivery System Architecture
    
    actor UserA as "User A (Sender)"
    actor UserB as "User B (Receiver)"

    package "Client Layer" {
        component WebApp as "Web/Mobile Client"
    }

    package "Backend Layer" {
        component API as "API Gateway"
        component MessageService as "Message Service"
        component AuthService as "Auth Service"
        component DeliveryService as "Delivery Service"
    }

    database "Database" {
        component DB as "Messages DB"
    }

    queue "Message Broker" {
        component Queue as "Delivery Queue"
    }

    UserA --> WebApp : "Sends message"
    WebApp --> API : "POST /v1/messages"
    API --> AuthService : "Authorize"
    API --> MessageService : "Create Message"
    
    MessageService --> DB : "Store message (Pending)"
    MessageService --> Queue : "Push to queue"
    
    Queue --> DeliveryService : "Consume message"
    DeliveryService --> UserB : "Attempt Delivery (WebSocket/Push)"
    
    DeliveryService -.-> DB : "Update status (Delivered/Failed)"
    UserB -.-> DeliveryService : "ACK (Acknowledgement)"
```