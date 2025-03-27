# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Menggunakan `RwLock<>` diperlukan untuk memungkinkan akses yang lebih efisien terhadap daftar notifikasi secara bersamaan oleh beberapa thread dibandingkan `Mutex<>`. Keduanya sama sama digunakan untuk mengelola akses data dalam multi-threaded server agar tidak thread ada yang mengelola data yang sama. Namun `Mutex<>` hanya memungkinkan satu thread untuk mengakses data baik baca maupun tulis, sedangkan `RwLock<>` memungkinkan beberapa thread untuk membaca data bersamaan selama tidak ada thread yang menulis. Hal ini dibutuhkan pada method add yang membutuhkan satu thread pada satu data, dan method list_all_as_string yang membutuhkan banyak thread membaca data bersamaan tanpa perlu menunggu.

2. Rust tidak mengizinkan kita untuk memodifikasi variabel statis secara langsung seperti yang bisa dilakukan di Java, karena pendekatan Rust yang sangat mengutamakan keamanan di lingkungan multi threaded. Di Java, variabel statis bersifat mutable secara default, dan kita bisa mengubah isinya melalui metode statis. Namun, ini membawa risiko terkait dengan masalah seperti kondisi balapan (race condition) dalam lingkungan multi threaded.

#### Reflection Subscriber-2
1. Saya mengexplore file `src/lib.rs`, dari pemahaman saya file ini berisi konfigurasi konfigurasi yang dibutuhkan aplikasi kita seperti pengaturan URL root instance dan publisher lalu juga file ini memberi tahu aplikasi kita bahwa konfigurasi aplikasi disimpan di file `.env`. Selain menyimpan konfigurasi, file ini juga menyediakan helper method seperti `compose_error_response` yang dipakai di notification service.

2. Dengan menggunakan Observer pattern, menambah subscriber baru jadi lebih mudah karena pattern ini memisahkan publisher dari subscriber. Ketika publisher mengirimkan update, semua subscriber yang terdaftar akan menerima notifikasi secara otomatis. Hal ini memungkinkan fleksibilitas untuk menambah atau mengurangi subscriber tanpa mempengaruhi kode publisher, karena keduanya terpisah secara jelas.

3. Saya sudah mencoba menggunakan postman untuk mencoba endpoint endpoint yang sudah kita buat, seperti mencoba untuk mencari cara agar mendapat http response status yang tidak error pada Subscribe to type bambangshop receiver dengan cara menambahkan produk dulu di bambangshop (langkah 30). Saya juga mencoba coba untuk endpoint lainnya seperti list all product, get product by id, dll. Saya merasa bahwa ini adalah fitur yang sangat berguna untuk testing API. Dengan adanya postman collection, kita tidak perlu lagi debug secara manual di program kita untuk melakukan testing atau sekedar verifikasi.
