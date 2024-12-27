# AuroraPress - Coming soon!

AuroraPress is a high-throughput webhook-as-a-service platform built with Ruby on Rails, hosted on Heroku. It supports real-time event delivery to client-provided HTTPS endpoints with secure HMAC-signed payloads and handles 5,000+ webhook events per second using scalable, event-driven architecture.

---

## Features
- Real-time event delivery to client webhooks.
- Secure HMAC-signed payloads for authentication.
- Scalable architecture with AWS SNS, SQS, Sidekiq, and Redis.
- Exponential backoff for retries to ensure reliable delivery.
- Hosted on Heroku for easy deployment and scalability.

---

## **System Architecture**

The architecture is designed to efficiently process, queue, and deliver webhook events while maintaining scalability and security.

1. **Event Generation**
   - Ruby on Rails generates events when specific triggers occur (e.g., user actions, email interactions).

2. **Event Distribution**
   - Events are published to AWS SNS (Simple Notification Service) for distribution.
   - AWS SNS sends events to Amazon SQS (Simple Queue Service) and optional AWS Lambda functions.

3. **Job Queueing**
   - Amazon SQS stores events for processing.
   - Redis acts as a local queue for high-priority tasks.

4. **Background Processing**
   - Sidekiq workers process jobs from Redis and Amazon SQS.
   - Lightweight tasks are handled by AWS Lambda.

5. **Webhook Delivery**
   - Sidekiq sends events to client-provided HTTPS endpoints with secure HMAC-signed payloads.

6. **Retry Mechanism**
   - Failed deliveries are retried using exponential backoff until a configurable maximum retry count is reached.
  
---

## **End-to-End Flow**

1. **Event Trigger**
   - An event (e.g., `email.opened`) is generated in the Rails application.

2. **Publish to SNS**
   - The event is published to AWS SNS (Simple Notification Service) for distribution.

3. **Event Fan-Out**
   - AWS SNS distributes the event to:
     - **Amazon SQS**: For queuing and eventual processing by Sidekiq.
     - **AWS Lambda**: For lightweight processing tasks.

4. **Job Processing**
   - **Sidekiq** workers fetch jobs from Redis or Amazon SQS and process them.

5. **Webhook Delivery**
   - The processed job is sent to the client’s HTTPS webhook endpoint with a secure HMAC signature for verification.

6. **Retry on Failure**
   - If delivery fails, the system retries with exponential backoff until the maximum retries are reached.

---

## **Technologies Used**

- **Ruby on Rails**: Core framework for application logic and API development.
- **Heroku**: Hosting platform for simplified deployment and scalability.
- **AWS SNS**: Event distribution for real-time notifications.
- **AWS SQS**: Scalable queue for decoupling job processing.
- **Redis**: Local queue for high-priority job caching.
- **Sidekiq**: Background job processing with retry logic.
- **AWS Lambda**: Lightweight task execution.
- **HMAC**: Secure webhook payload authentication.

---

## **Contact**
 - Contact me on shuklavishwam111@gmail.com for more information
