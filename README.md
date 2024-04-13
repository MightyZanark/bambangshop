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
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

1. Menurut saya, untuk kasus BambangShop, sebuah struktur Model `Subscriber` sudah cukup untuk pattern Observer karena dalam kasus ini, BambangShop hanya memiliki suatu model `Product` yang memiliki atribut tipe yang akan di *subscribe* oleh `Subscriber`. Namun, jika BambangShop nantinya akan menambahkan model lain yang juga bisa di *subscribe* oleh `Subscriber`, akan lebih baik apabila terdapat sebuah interface atau trait untuk `Subscriber`.

> id in Product and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

2. Dalam kasus ini, penggunaan `DashMap` tetap diperlukan meskipun `id` dalam `Product` dan `url` dalam `Subscriber` unik. Hal ini dikarenakan kita ingin menyimpan `Subscriber` berdasarkan `product_type` dari suatu `Product`. Jika digunakan `Vec`, maka kita harus memetakan juga index dari `Vec` ke `product_type`, sehingga pada akhirnya akan menambah struktur data yang diperlukan juga. Dalam penyimpanan `url`, akan lebih mudah dengan menggunakan `DashMap` juga karena akan mempermudah penghapusan `Subscriber`. Jika `url` disimpan dalam `Vec`, maka ketika ada `Subscriber` yang ingin berhenti *subscribe* terhadap sebuah `product_type`, kita harus melakukan *linear search* untuk mencari objek `Subscriber` yang relevan baru bisa menghapusnya dari `Vec`. Dengan menggunakan `DashMap`, hal ini diharapkan dapat dilakukan dalam waktu konstan dibanding linear. 

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

3. Menurut saya, kita bisa-bisa saja mengimplementasikan pattern Singleton, namun tenaga yang harus dikeluarkan untuk mengimplementasikannya jugalah besar karena Rust memiliki banyak restriksi terkait static variable dan mutability. Sehingga, dalam kasus ini dimana kita hanya ingin menyimpan `Subscriber` berdasarkan `product_type` yang di *subscribe*, menurut saya tenaga yang harus dikeluarkan terlalu besar dibanding apa yang ingin dicapai, sehingga akan lebih mudah menggunakan DashMap untuk mencapai apa yang kita inginkan.

#### Reflection Publisher-2

#### Reflection Publisher-3
