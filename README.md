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
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

>1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust) in this BambangShop case, or a single Model `struct` is enough?

Dalam konteks BambangShop, di mana hanya terdapat satu kelas Observer, yakni `Subscriber`, dan tidak ada variasi behavior yang berbeda di antara `Subscriber`, penggunaan sebuah Model `struct` tunggal sudah mencukupi tanpa perlu memperkenalkan antarmuka tambahan. Interface diperlukan ketika terdapat beberapa tipe dengan perilaku yang berbeda pada `Subscriber`, namun hal ini tidak berlaku dalam kasus `BambangShop`. Sehingga, dalam implementasi ini, penggunaan single model `struct` sudah cukup untuk menangani observasi perubahan dengan efektif.

>2. `id` in `Program` and `url` in `Subscriber` is intended to be unique. Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary) like we currently use is necessary for this case?

Kita memutuskan untuk menggunakan `DashMap` dalam kasus ini karena efisiensi pemetaan antara `Product` dan `Subscriber`. Dengan menggunakan `DashMap`, kita dapat langsung memetakan `Product` ke `Subscriber` tanpa perlu membuat dua `Vec` terpisah untuk setiap produk yang nantinya dapat mempersulit pengelolaan data. `DashMap` memudahkan akses dan pemeliharaan data dengan pemetaan langsung antara `Product` dan `Subscriber` berdasarkan kunci, seperti `id` atau `url` produk, sehingga pencarian data atau value menjadi lebih mudah dan efisien.

>3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (`SUBSCRIBERS`) static variable, we used the `DashMap` external library for **thread safe `HashMap`**. Explain based on your understanding of design patterns, do we still need `DashMap` or we can implement Singleton pattern instead?

Dalam konteks pemrograman Rust, implementasi **Singleton** dan penggunaan `DashMap` dalam kasus variabel statis Daftar Pelanggan (`SUBSCRIBERS`) saling melengkapi untuk memastikan keamanan dan konsistensi data. **Singleton** memastikan bahwa hanya ada satu instance yang ada di satu interface, sementara `DashMap` digunakan untuk memastikan keamanan penggunaan `HashMap` oleh thread. Dengan kombinasi kedua pendekatan ini, program dapat memastikan **thread-safety** dan integritas data yang diperlukan dalam aplikasi.

#### Reflection Publisher-2

>1. In the Model-View Controller (MVC) compound pattern, there is no `Service` and `Repository`. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Dalam pola MVC, pemisahan antara `Service` dan `Repository` dari Model adalah penting untuk mematuhi prinsip tanggung jawab tunggal. Dengan "Service" menangani logika aplikasi dan "Repository" mengelola akses data, modularitas kode meningkat dan memudahkan pengembangan serta pemeliharaan. Memisahkan fungsionalitas ini ke dalam file-file terpisah memastikan bahwa setiap bagian kode dapat dibaca dan dimodifikasi dengan lebih mudah, menghindari kompleksitas yang terlalu tinggi dalam satu file.

>2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (`Program`, `Subscriber`, `Notification`) affect the code complexity for each model?

Jika hanya menggunakan Model tanpa lapisan lain seperti `Service` dan `Repository`, program akan memiliki keterikatan yang tinggi, meningkatkan kompleksitas kode karena adanya ketergantungan erat antara mereka. Hal ini berarti perubahan dalam satu bagian dapat berdampak luas pada yang lain, memerlukan penyesuaian yang rumit dalam kode. Selain itu, jika semua fungsionalitas digabung dalam satu file, seperti dalam kasus memiliki 3 model (`Program`, `Subscriber`, dan `Notification`), file tersebut akan sulit dibaca dan memahami. Ini mengakibatkan kesulitan dalam memodifikasi atau mengubah kode saat diperlukan perubahan, karena kompleksitas yang tinggi dan kesulitan untuk menavigasi kode yang panjang.

>3. Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Postman merupakan tools yang sangat membantu dalam menguji aplikasi yang dibuat. Dengan Postman, saya dapat memastikan respons aplikasi sesuai harapan berdasarkan permintaan yang saya buat, serta menyesuaikan metode seperti CRUD untuk memverifikasi keakuratan data. Fitur-fitur yang saya temukan berguna termasuk pengaturan koleksi permintaan HTTP, otomatisasi pengujian dengan skrip, serta menyediakan lingkungan terisolasi untuk menguji integrasi dengan API eksternal. Postman juga menyediakan fitur pengujian otomatis yang membantu menjalankan serangkaian tes secara berkala untuk memastikan konsistensi dan kualitas aplikasi.

#### Reflection Publisher-3
