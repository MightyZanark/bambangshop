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
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

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
> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

1. Kita harus memisahkan `Service` dan `Repository` dari `Model` agar setiap komponen dalam kode kita memenuhi prinsip *Single Responsibility*. Dengan memisahkan `Service` dan `Repository` dari `Model`, kita memisahkan pula tanggung jawab `Model` yang awalnya bertanggung jawab menghandle penyimpanan data, *business logic*, dan representasi data, menjadi hanya representasi data. Tanggung jawab penyimpanan data diberikan kepada `Repository` dan tanggung jawab *business logic* diberikan kepada `Service`.

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Product, Subscriber, Notification) affect the code complexity for each model?

2. Ketika kita hanya menggunakan `Model`, kode di setiap `Model` akan menjadi lebih kompleks karena banyak tanggung jawab lain yang harus di handle selain tanggung jawab representasi data. Sebuah `Subscriber` saja sudah berinteraksi banyak dengan `Notification`, seperti bisa `subscribe`, `unsubsribe`, diberikan notifikasi ketika ada perubahan pada `product_type` yang di *subscribe*, dan bisa saja masih banyak lagi. Jika semua kode ini digabungkan ke dalam satu `Model`, tentu akan susah untuk menentukan sebenarnya apa yang dilakukan `Model` tersebut.

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

3. Saya belum terlalu mengeksplor banyak mengenai `Postman`, namun dari apa yang sudah saya ketahui, alat tersebut cukup membantu dalam melakukan testing terhadap API yang saya buat. HTTP Request yang bisa dilakukan juga tidak sebatas `GET` dan `POST` sehingga saya bisa dengan mudah mengetes apakah *endpoint* tertentu benar hanya dapat menerima suatu tipe HTTP Request atau tidak. Terdapat juga fitur menambahkan `Headers` dan `Authorization` yang menurut saya akan dapat digunakan dalam Tugas Kelompok saya atau projek lainnya yang menggunakan *custom headers* dan *authorization*. Secara keseluruhan menurut saya aplikasi `Postman` sangatlah membantu melakukan API testing, dibanding harus membuat script sendiri atau mengetes langsung dengan browser.

#### Reflection Publisher-3
> Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

1. Pada tutorial ini kita menggunakan model `Push` karena setiap kali ada perubahan pada `Product` (publisher), `Subscriber` akan diberikan notifikasi melalui `Notification`. Ini bisa dilihat pada `src/service/product.rs` dimana ketika ada produk baru yang di buat, di hapus, atau di publish, `NotificationService` akan digunakan untuk menotifikasi `Subscriber` terkait.

> What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

2. Keuntungan jika kita menggunakan model `Pull` pada kasus tutorial ini adalah kita tidak harus mengirimkan notifikasi kepada setiap `Subscriber` tiap kali ada perubahan pada `Product`, melainkan `Subscriber` bebas untuk kapan saja meminta perubahan kepada `Product` terkait. Ini tentu membuat server dimana web app kita dijalankan tidak memakan banyak *resource* setiap kali ada perubahan pada `Product` karena harus membuat thread-thread baru untuk mengirimkan notifikasi kepada `Subscriber`. Di lain sisi, kekurangan model `Pull` pada kasus tutorial ini adalah `Subscriber` harus secara aktif meminta data kepada `Product` jika mereka ingin selalu mendapatkan perubahan terbaru yang ada pada `Product` terkait. `Subscriber` tidak dapat menebak kapan ada perubahan baru pada `Product` sehingga harus meminta data baru secara berkala agar `Subscriber` terus memiliki perubahan terbaru.

> Explain what will happen to the program if we decide to not use multi-threading in the notification process.

3. Setiap `product_type` dapat memiliki banyak `Subscriber`. Jika kita tidak menggunakan multi-threading untuk proses notifikasi, ini akan menjadikan proses notifikasi memakan waktu yang sangat lama dan memblokir proses lain untuk berjalan. Ini akan menyebabkan web app kita tidak dapat menghandle *request* lain selagi mengirimkan notifikasi ke `Subcriber` terkait. Tentu ini merupakan hal yang tidak diinginkan kita, oleh karena itu, multi-threading digunakan agar proses pengiriman notifikasi tidak memblokir proses lain untuk berjalan, sehingga web app kita masih dapat menghandle *request* lain.
