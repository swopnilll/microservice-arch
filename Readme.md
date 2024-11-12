# Microservices Communication: Synchronous vs Asynchronous

## Introduction

In this video, we dive deeper into the challenges that arise when introducing a new service in a microservices architecture, particularly when it needs to access data from other services. We will explore two strategies for communication between services: **Synchronous (Sync)** and **Asynchronous (Async)** communication.

### Background

We are building upon the simple e-commerce application with services handling users, products, and orders. The challenge emerges when introducing a new service (Service DX) that needs data from multiple existing services. Since each service has its own database, **Service DX** cannot directly access the databases of other services, and thus, we need to figure out how these services can communicate.

## Synchronous Communication

### Definition

In a **synchronous communication** model, one service communicates directly with another service to fetch information. This could involve HTTP requests, JSON exchanges, or other types of direct requests.

### Example: Applying Synchronous Communication

Let's apply synchronous communication to the e-commerce example:

1. **Service DX** receives a request to show all products ordered by the user with ID 1.
2. **Service DX** sends a direct request to **Service A** (User Service) to verify if the user exists.
3. If the user exists, **Service DX** sends a request to **Service C** (Order Service) to retrieve all orders placed by the user.
4. Then, **Service DX** sends another request to **Service B** (Product Service) to get details about the products ordered.
5. Finally, **Service DX** compiles all the data and responds to the initial request.

In this model, Service DX does not directly access the databases of any service, maintaining database isolation between services.

### Upsides of Synchronous Communication

1. **Conceptually Simple**: It is easy to understand. A service makes requests to other services to fetch data, which is a straightforward model.
2. **No Database for Service DX**: Service DX doesn't need its own database since it relies on data from other services, thus saving resources.

### Downsides of Synchronous Communication

1. **Service Dependency**: Service DX depends on Services A, B, and C. If any of these services fail, **Service DX** will fail as well, causing potential disruptions in the application.
2. **Cascading Failures**: If any request fails (e.g., failure in fetching user data from Service A), the entire request will fail. This introduces the risk of a poor user experience due to unreliable service interactions.
3. **Latency and Speed**: The overall response time is determined by the slowest request. If one of the requests takes longer (e.g., a 20-second delay in fetching product details), the entire process will be delayed.
4. **Complex Dependencies**: Synchronous communication can lead to a tangled web of dependencies. For example, Service A might internally rely on other services (Service Q, Service Z), and if any of those fail or are slow, it can impact the entire system, making troubleshooting more complex.

### Real-World Challenges

- **Web of Dependencies**: As services become more complex, each service might make additional requests to other services, causing a chain reaction of dependencies. For example, Service A may need to query Service Q, which queries Service Z, and so on, making debugging and maintaining the system challenging.
- **Failure Points**: The more dependencies there are, the more failure points you introduce. If any part of the communication chain fails, it impacts the entire flow, which is a significant concern in production environments.

### Conclusion on Synchronous Communication

While synchronous communication is simple and intuitive, it has significant operational downsides, especially when it comes to reliability and performance. It can create tight service dependencies, and the system becomes as slow as the slowest component.

---

## Next Steps: Exploring Asynchronous Communication

Now that we have a good understanding of synchronous communication, we will dive into **asynchronous communication**. This model can help address some of the issues faced with synchronous communication, such as service dependency and latency.

Stay tuned as we explore how asynchronous communication works and how it can provide more flexibility and reliability in microservices architectures.
