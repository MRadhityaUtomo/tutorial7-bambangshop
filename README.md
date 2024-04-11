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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Based on the defined need of trait implementation, it is not needed to implement an interface because the behaviour of all Subscribers will be the same, thus eliminating the primary need of traits that is to add and differentiate multiple behaviours for Subscribers . If in the future it is intended to implement Subscribers with unique behaviours, then based on the good coding practices learned from before, it is recommended to implement an interface.  

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

The main characteristics of Vec is it's suitable for smaller datasets with a fixed order. Similar to a list it doesn't do much to a dataset with unique identifiers such as keys, so it has a linear look up time due to it needing to iterate through the list.
On the other hand, DashMaps are similar to dictionary or hashmaps/maps where it's operations are based on the key + value combination of datasets. It has a constant look up time by searching for the unique identifiers of a datum in a dataset. 
Because id and url is intended to be unique, then it can implement keys for their identifiers, thus making it better or more suitable to use DashMaps for better operation time than a single list/Vec. 

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread-safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

The advantages of using a Singleton pattern are being able to control access to the shared instance of your data structure, ensuring that only one instance exists throughout the program's lifetime, It also eliminates the need for an external dependency, simplifying your project's dependency graph.
However, I think using an external library like DashMap is better as it provides a robust and efficient solution for managing data shared data structures in a multithreaded environment. Furthermore, Singleton is needed to ensure that there is only one instance of the SUBSCRIBER instance from DashMap accessed by multiple threads.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Based on Seperation of Concerns principles, it's more beneficial to further divide layers of a program to increase maintainability, testability, flexibility, and scalability and also covering design principles such as SRP, modularity, and encapsulation. For example, by seperating the "Service" to only operate the business logic and application specific operations, while "Repository" can focus on accessing the database and data manipulation functions, both of these can be tested and implement new features on if needed seperately without having to tweak the other.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If having MVC with model doing the business model is already pretty complex, then having only model doing everything will make the code extremely hard to maintain. Having each module relying eachother in a single layer will drastically increase code complexity and high risk of making it tightly coupled (highly dependent of eachother's components).

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Postman is an extremely helpful program that helps API data managing in projects. It has simple functions for HTTP requests such as GET, POST, and DELETE to retrive accurate data and test a program's CRUD via URL endpoints. It is also very helpful for importing or exporting collections with an easy to navigate GUI. Not only this project, but it has also helped me on previous projects as well such as Web Development with Django, and it surely help me on future projects too. 

#### Reflection Publisher-3