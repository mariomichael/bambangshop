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
1. Dalam kasus BambangShop, menurut saya penggunaan single model `struct` sudah cukup. Hal ini dikarenakan ukuran projek ini masih terbilang kecil dan tidak banyak dependencies dari fitur ini. Selain itu, semua subscriber dapat dikatakan berperilaku identik tanpa adanya hal khusus yang dapat dilakukan salah satu subscriber. Oleh karena itu satu struktur Subscriber cukup untuk menangani kasus ini. Penggunaan interface lebih baik dilakukan untuk projek yang lebih besar dan adanya kebutuhan khusus untuk mengangani setiap objek secara spesifik.
2. Menurut saya, penggunaan Vec tidak akan cukup. Hal ini dikarenakan kode kita akan menjadi lebih kompleks sehingga susah di-maintain. Penggunaanya memungkinkan terjadinya duplikasi id sehingga id akan kehilangan sifat keunikannya. Oleh karena itu pada kasus ini lebih baik digunakan DashMap. Dengan menggunakan DashMap, kita bisa menyimpan setiap objek dengan keynya secara unik.
3. Menurut pemahaman saya, dalam kasus ini kita tetap memerlukan DashMap. Hal ini dikarenakan penggunaan HashMap pada Rust tidak bersifat thread-safe. Beberapa thread berbeda akan mengakses SUBSCRIBER secara bersamaan sehingga perlu dipastikan keamanannya dengan DashMap. Dengan menggunakan DashMap ini kita juga bisa memastikan bahwa hanya satu thread yang memodifikasi satu Subscriber dalam satu waktu. Hal ini menjamin keamanan subscriber yang dimodifikasi.

#### Reflection Publisher-2
1. Menurut pemahaman saya, pemisahan "Service" dan "Repository" dari sebuah model perlu dilakukan untuk memenuhi salah satu prinsip desain yaitu Single Responsibility Principle. Dengan melakukan pemisahan ini juga dapat meningkatkan loose coupling sehingga dependency antarsuatu bagian dengan bagian lain tidak terlalu tinggi.
2. Jika kita hanya menggunakan model, maka kode yang dihasilkan akan sangat kompleks dan berantakan. Hal ini karena setiap model akan mengelola penyimpanan data sekaligus business logic. Interaksi antarketiga model akan sangat kompleks sehingga harus dilakukan aggregation. Akibatnya, kompleksitas kode akan meningkat.
3. Postman membantu saya dalam mengetes projek yang sedang saya jalankan. Dengan menggunakan Postman, saya dapat mencocokan antara input dan output yang diharapkan serta bagaimana data tersebut diproses di web yang sedang dibuat.

#### Reflection Publisher-3
1. Dalam kasus tutorial kali ini, kita menggunakan variasi Push. Hal ini dapat dilihat dari NotificationService yang selalu mem-push data ke subscriber setiap adanya update.
2. Keuntungan jika kita menggunakan variasi lain, yang dalam hal ini Pull adalah subscriber hanya perlu melakukan request update saat dibutuhkan sehingga beban yang dilakukan berkurang. Kekurangan dari Pull adalah bisa terjadi kesalahan jika subscriber tidak mengambil data perubahan dan ada perubahan lain.
3. Jika kita memutuskan untuk tidak menggunakan multi-threading dalam notifikasi proses, pengiriman notifikasi akan dilakukan secara satu per satu. Hal ini akan memperlambat waktu pengiriman notifikasi karena notifikasi tidak bisa dikirimkan sekaligus. Hal ini akan merugikan subscriber di urutan paling terakir.
