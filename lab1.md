### Component Diagram

'''Mermaid

graph TD


    UserA["User A (Sender)"]
    UserB["User B (Receiver)"]
    
    subgraph ClientLayer["Client Layer"]
        WebApp["Web/Mobile Client"]
    end
    
    subgraph BackendLayer["Backend Layer"]
        API["API Gateway"]
        MessageService["Message Service"]
        AuthService["Auth Service"]
        DeliveryService["Delivery Service"]
    end
    
    subgraph DataLayer["Data Layer"]
        DB[("Messages DB")]
    end
    
    subgraph MessageBroker["Message Broker"]
        Queue["Delivery Queue"]
    end
    
    UserA -->|Sends message| WebApp
    WebApp -->|POST /v1/messages| API
    API -->|Authorize| AuthService
    API -->|Create Message| MessageService
    MessageService -->|Store message Pending| DB
    MessageService -->|Push to queue| Queue
    Queue -->|Consume message| DeliveryService
    DeliveryService -->|Attempt Delivery WebSocket/Push| UserB
    DeliveryService -.->|Update status Delivered/Failed| DB
    UserB -.->|ACK Acknowledgement| DeliveryService
'''
