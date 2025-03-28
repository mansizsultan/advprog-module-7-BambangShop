# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust) in this BambangShop case, or a single Model `struct` is enough?

In the traditional Observer pattern, a Subscriber is typically defined as an interface to allow for flexibility and ensure that all subscribers implement certain methods, such as `update()`. In the BambangShop case, we could opt for a simple model struct without an interface if we are only managing a single type of subscriber. However, defining a trait (interface) in Rust would still be beneficial if we expect different types of subscribers with varying behaviors in the future.

2. `id` in `Program` and `url` in `Subscriber` is intended to be unique. Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary) like we currently use is necessary for this case?

While a `Vec` could work for a small number of subscribers, using a `DashMap` is more efficient when handling large numbers of unique identifiers, such as `id` or `url`. The `DashMap` allows for fast lookups, insertions, and deletions, which ensures better performance, especially as the number of subscribers increases. It also provides thread safety, which is crucial in concurrent environments.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers **(SUBSCRIBERS)** static variable, we used the `DashMap` external library for **thread-safe ``HashMap``**. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

The Singleton pattern ensures a single instance of an object, but it doesn't inherently provide thread safety. In this case, using the `DashMap` external library is still necessary because it ensures thread-safe operations for shared data. While a Singleton could ensure only one instance of the subscriber list, it would not manage concurrent data access efficiently without a thread-safe collection like `DashMap`.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

In complex applications, separating the "Service" and "Repository" from the Model helps to maintain a clean architecture. The Repository pattern abstracts the data access layer, making it easier to manage data storage independently of business logic. The Service pattern encapsulates business logic, keeping it separate from data storage concerns. This separation reduces code complexity and improves maintainability.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model **(Program, Subscriber, Notification)** affect the code complexity for each model?

If we only use the Model, the business logic and data access would be tightly coupled in each model. This would result in a more complex, harder-to-maintain system, as the models would be responsible for both data storage and processing. As the system grows, these models would become large and difficult to manage, increasing the likelihood of bugs and making testing harder.

3. Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Postman is an excellent tool for testing APIs. It allows us to test different endpoints and ensures that the correct data is returned from the server. Features like collections for organizing requests, environments for managing different setups, and automated testing make Postman invaluable. It helps in quickly validating API functionality and is essential for our group project testing as well as future projects.

#### Reflection Publisher-3

1. Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and **Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

In this tutorial, we are using the Push model of the Observer pattern. The publisher pushes notifications to the subscribers whenever a product is created, updated, or deleted.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

If we had used the **Pull model**, subscribers would request data from the publisher at regular intervals. The advantage would be that subscribers control when they receive updates. However, the major disadvantages are that it introduces unnecessary load on the server due to frequent polling and could result in delays in receiving important updates. The Push model is more efficient because the publisher directly sends notifications to subscribers when events occur.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Without multi-threading, the notification process would be sequential, meaning each notification would be sent one by one. This would create significant delays, especially when there are many subscribers. It could also block the main application process, leading to poor performance. Multi-threading allows notifications to be sent concurrently, improving performance and responsiveness.
