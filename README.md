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
-   **STAGE 2: Implement services and controllers**
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
>In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Dalam case tutorial ini, kita lebih memilih menggunakan `RwLock<>` untuk menyinkronkan akses ke vektor Notifications adalah karena mekanisme ini memungkinkan beberapa thread untuk membaca data secara bersamaan. Sebaliknya, jika kita menggunakan `Mutex<>,` hanya satu thread yang bisa mengakses vektor tersebut dalam satu waktu, baik untuk membaca maupun menulis. Hal ini dapat menyebabkan bottleneck dalam aplikasi, terutama ketika banyak bagian dari sistem perlu membaca notifikasi tetapi harus menunggu giliran karena ada thread lain yang sedang mengakses data sehingga agak kurang efisien. 

>In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Seperti yang kita katahu, Rust tidak mengizinkan mutasi langsung pada variabel statis seperti di Java karena alasan thread safety dan keamanan memori. Dalam multi-threading, akses global ke variabel statis berisiko menyebabkan race condition. Rust secara default membuat variabel statis immutable untuk mencegah akses tak terkendali. Jika perlu diubah, kita harus menggunakan mekanisme seperti `RwLock<>` atau `DashMap<>`, yang memastikan data tetap aman saat diakses oleh banyak thread. nah disinilah `lazy_static!` digunakan untuk lazy initialization, sehingga variabel hanya diinisialisasi sekali saat pertama kali digunakan, membuatnya lebih efisien dan aman dibandingkan static mut. Dengan pendekatan ini, Rust menjaga keamanan memori lebih ketat dibanding Java.

#### Reflection Subscriber-2

>Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Ya. Di beberapa part code lain, saya sempat mengeksplorasi beberapa bagian code di luar langkah-langkah tutorial, terutama pada `src/lib.rs` dan `main.rs`. Dari `lib.rs`, saya belajar bagaimana aplikasi mengelola konfigurasinya menggunakan struct AppConfig, yang memanfaatkan `lazy_static` untuk menciptakan pola mirip singleton dalam pengaturan aplikasi. Saya menemukan bahwa penggunaan sistem konfigurasi Figment dari Rocket sangat menarik, karena memungkinkan penggabungan nilai default dengan variabel lingkungan yang dimuat melalui .env. nah hal ini membuat aplikasi lebih fleksibel tanpa perlu mengubah code secara langsung.

>Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Setelah menyelesaikan tutorial dan menguji sistem notifikasi dengan beberapa Receiver, menurut saya Observer pattern benar-benar mempermudah penambahan subscriber baru (one-to-many). Saya hanya perlu menjalankan Receiver lain di port yang berbeda (sesuai dengan petunjuk tutorial) dan mendaftarkannya ke jenis produk yang diminati. Aplikasi utama (Main app) tidak perlu mengalami perubahan kode, karena secara otomatis mencatat subscriber baru saat mereka berlangganan dan mengirimkan notifikasi ketika terjadi perubahan yang relevan. Hal ini menunjukkan bagaimana Observer pattern memungkinkan sistem tetap fleksibel dengan hubungan yang tidak terlalu terikat (loosely coupled). 

Kemudian bagaimana dengan spawning lebih dari 1 instance Main app? jawabannya menurut saya menjalankan beberapa instance Main app cukup rumit karena tiap instance memiliki daftar subscriber sendiri di DashMap. Subscriber harus mendaftar ke setiap instance atau perlu mekanisme sinkronisasi antar Main app. Selain itu, jika sebuah produk dibuat di satu instance, instance lain tidak otomatis mengetahuinya, sehingga notifikasi bisa terlewat. Untuk skenario many-to-many, dibutuhkan solusi seperti event sourcing atau message broker bersama.

>Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Saya belum pernah mencoba membuat test sendiri di Postman, tetapi dari test yang sudah ada di Postman collection, sepertinya fitur ini bekerja layaknya test biasa. Menurut saya, penambahan test sangat bermanfaat untuk memastikan API berfungsi sesuai ekspektasi dan mendeteksi error lebih cepat. Selain itu, dokumentasi di Postman mempermudah tim dalam memahami penggunaan API, berkolaborasi, dan melakukan debugging.