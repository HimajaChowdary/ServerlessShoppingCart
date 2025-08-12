# Serverless Shopping Cart Application

A fully serverless e-commerce shopping cart built on AWS cloud services with secure authentication, real-time updates, and scalable event-driven architecture. Features global content delivery, managed user authentication, and serverless backend processing.

## What It Does

**Shopping Experience Journey:**
1. **User Authentication** → Secure login/signup using AWS Cognito with JWT tokens
2. **Product Browsing** → Fast global content delivery via CloudFront CDN
3. **Cart Management** → Real-time add/remove/update operations with instant feedback
4. **Event Processing** → Asynchronous cart operations using Lambda and SQS
5. **Data Synchronization** → Real-time updates across devices using DynamoDB Streams

## System Architecture

```mermaid
graph TB
    %% Users
    USER[👤 Users<br/>Global Access]
    
    %% Frontend & CDN
    S3[🪣 AWS S3<br/>Static Website<br/>React SPA]
    CLOUDFRONT[🌐 CloudFront<br/>Global CDN<br/>Edge Locations]
    
    %% Authentication
    COGNITO[🔐 AWS Cognito<br/>User Authentication<br/>JWT Tokens]
    
    %% API Gateway
    APIGW[🌐 API Gateway<br/>REST Endpoints<br/>CORS & Throttling]
    
    %% Serverless Backend
    subgraph "Lambda Functions"
        CART_LAMBDA[⚡ Cart Lambda<br/>CRUD Operations<br/>Business Logic]
        AUTH_LAMBDA[🔑 Auth Lambda<br/>Token Validation<br/>User Management]
        STREAM_LAMBDA[📡 Stream Lambda<br/>Real-time Updates<br/>Event Processing]
    end
    
    %% Message Queue
    SQS[📨 AWS SQS<br/>Message Queue<br/>Async Processing]
    
    %% Database
    DYNAMODB[(🗃️ DynamoDB<br/>Cart Data<br/>User Sessions)]
    STREAMS[📊 DynamoDB Streams<br/>Change Capture<br/>Real-time Events]
    
    %% User Flow
    USER --> CLOUDFRONT
    CLOUDFRONT --> S3
    S3 --> COGNITO
    
    %% API Calls
    S3 --> APIGW
    APIGW --> AUTH_LAMBDA
    APIGW --> CART_LAMBDA
    
    %% Authentication Flow
    AUTH_LAMBDA --> COGNITO
    COGNITO --> AUTH_LAMBDA
    
    %% Cart Operations
    CART_LAMBDA --> DYNAMODB
    CART_LAMBDA --> SQS
    
    %% Async Processing
    SQS --> STREAM_LAMBDA
    STREAM_LAMBDA --> DYNAMODB
    
    %% Real-time Updates
    DYNAMODB --> STREAMS
    STREAMS --> STREAM_LAMBDA
    STREAM_LAMBDA --> S3
    
    %% Styling
    classDef user fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#0d47a1
    classDef frontend fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#4a148c
    classDef auth fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,color:#e65100
    classDef api fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px,color:#1b5e20
    classDef serverless fill:#fce4ec,stroke:#c2185b,stroke-width:2px,color:#880e4f
    classDef messaging fill:#f1f8e9,stroke:#558b2f,stroke-width:2px,color:#33691e
    classDef database fill:#fff8e1,stroke:#f9a825,stroke-width:2px,color:#f57f17
    
    class USER user
    class S3,CLOUDFRONT frontend
    class COGNITO auth
    class APIGW api
    class CART_LAMBDA,AUTH_LAMBDA,STREAM_LAMBDA serverless
    class SQS messaging
    class DYNAMODB,STREAMS database
```

## Tech Stack

**Frontend:** React.js, AWS S3 Static Hosting, CloudFront CDN  
**Authentication:** AWS Cognito, JWT Tokens, Federated Identity  
**Backend:** AWS Lambda (Node.js), API Gateway, Serverless Framework  
**Database:** DynamoDB, DynamoDB Streams for real-time data  
**Messaging:** AWS SQS for asynchronous processing  
**Infrastructure:** Fully serverless, auto-scaling, pay-per-use

## Key Features

- 🌐 **Global Performance** - CloudFront CDN for fast worldwide content delivery
- 🔐 **Secure Authentication** - AWS Cognito with JWT tokens and federated login
- ⚡ **Serverless Backend** - Lambda functions for cart operations with automatic scaling
- 📨 **Asynchronous Processing** - SQS message queues for reliable cart operations
- 📊 **Real-time Updates** - DynamoDB Streams for instant cart synchronization
- 💰 **Cost Efficient** - Pay-per-use serverless architecture with no idle costs
- 🚀 **Auto Scaling** - Handles traffic spikes automatically without configuration
- 🛡️ **High Availability** - Multi-AZ deployment with built-in fault tolerance

## Architecture Benefits

### 🌍 **Global Scale**
- **CloudFront CDN** provides low-latency access worldwide
- **S3 static hosting** with high durability
- **Edge locations** cache content closer to users

### 🔒 **Enterprise Security**
- **AWS Cognito** manages user pools and federated identities
- **JWT tokens** for stateless authentication
- **API Gateway** with built-in DDoS protection and throttling

### ⚡ **Serverless Performance**
- **Lambda functions** scale for concurrent executions
- **DynamoDB** provides single-digit millisecond latency
- **SQS** ensures reliable message delivery with dead letter queues

### 💸 **Cost Optimization**
- **No server management** or idle resource costs
- **Pay-per-request** pricing model
- **Automatic scaling** prevents over-provisioning

## API Endpoints

```

## Event-Driven Flow

```
Cart Action → API Gateway → Lambda Function → DynamoDB
                                ↓
                            SQS Message → Stream Lambda → Real-time Update
                                ↓
                        DynamoDB Streams → Frontend Notification
```

---
**Fully serverless shopping cart with AWS cloud services, real-time updates, and global scale**
