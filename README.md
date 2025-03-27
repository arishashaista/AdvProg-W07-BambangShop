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
1. Pada kasus BambangShop, saat ini tidak perlu mendefinisikan *interface* atau *trait* untuk Subscriber. Karena Subscriber hanya memiliki dua atribut (URL dan nama) yang perlu disimpan, sehingga cukup menggunakan *struct* sebagai model data. Trait lebih cocok digunakan ketika ada beberapa perilaku atau metode yang perlu diimplementasikan oleh berbagai tipe data, namun dalam hal ini Subscriber hanya berfungsi sebagai representasi data, sehingga cukup dengan *struct* saja.

2. Dalam kasus ini, menggunakan DashMap lebih tepat dibandingkan Vec. Meskipun Vec dapat digunakan untuk menyimpan daftar Subscriber, tetapi dengan DashMap dapat mengakses dan memanipulasi data Subscriber lebih efisien dengan kunci yang unik, seperti url dan id. DashMap menyediakan pencarian yang lebih cepat dan aman dalam hal konkuren (thread-safe), yang sangat berguna jika ingin mengelola banyak Subscriber dengan cara yang lebih efisien dan aman.

3. Menggunakan DashMap dalam kasus ini sudah tepat karena diperlukan thread-safety untuk menangani akses bersamaan ke daftar Subscriber. DashMap memungkinkan untuk menambahkan, menghapus, dan mencari Subscriber secara aman pada multi-threading. Pola Singleton dapat digunakan jika hanya membutuhkan satu instance dari daftar Subscriber, tetapi DashMap sudah menyediakan fungsionalitas yang dibutuhkan tanpa harus menambah kompleksitas lebih lanjut. Jadi, meskipun Singleton bisa digunakan, DashMap lebih sesuai karena menyediakan fungsionalitas yang lebih fleksibel dan aman untuk kasus ini.

#### Reflection Publisher-2
1. Dalam pola MVC, Model biasanya menangani penyimpanan data dan logika bisnis. Namun, memisahkan tanggung jawab ke dalam komponen yang berbeda, yaitu Service dan Repository, dapat meningkatkan pemeliharaan dan skalabilitas kode. Repository bertanggung jawab untuk berinteraksi dengan penyimpanan data, yang memberikan lapisan abstraksi untuk operasi data. Service menangani logika bisnis, seperti memproses atau memanipulasi data sebelum dikirim ke view atau setelah diterima dari repository. Pemisahan ini mengikuti prinsip Single Responsibility Principle (SRP), yang memastikan bahwa setiap komponen memiliki tanggung jawab yang jelas dan terpisah. Selain itu, pemisahan ini memudahkan pengujian, karena Service dan Repository dapat diuji secara terpisah.

2. Jika hanya menggunakan Model tanpa memisahkan ke dalam Service dan Repository, kompleksitas kode akan meningkat. Model akan menjadi tanggung jawab untuk mengelola data dan logika bisnis, yang membuat lebih sulit untuk dipelihara dan di-*test*. Sebagai contoh, dalam model Program, Subscriber, dan Notification, akan ditempatkan logika untuk menyimpan data, memproses aturan bisnis, dan mengelola hubungan antara model dalam satu file. Hal ini akan membuat kode menjadi lebih kompleks dan sulit untuk dimodifikasi, karena perubahan pada satu bagian model dapat memengaruhi bagian lainnya. Selain itu, hal ini juga akan sulit untuk diskalakan seiring bertambahnya proyek, dan berbagai bagian kode menjadi sangat bergantung satu sama lain. 

3. Dengan menggunakan Postman, saya dapat mengirim permintaan HTTP (GET, POST, PUT, DELETE) ke aplikasi dan menganalisis responsnya. Postman membantu untuk memverifikasi apakah endpoint-endpoint dalam aplikasi BambangShop berfungsi sesuai yang diharapkan. Dengan kemampuan Postman untuk mempermudah proses pengujian menjadikannya alat yang sangat penting untuk proyek grup dan pekerjaan rekayasa perangkat lunak yang akan saya lakukan karena memastikan bahwa API terverifikasi dengan baik pada setiap tahap pengembangan.

#### Reflection Publisher-3
1. Pada kode ini, digunakan Push model. Publisher yang akan mengirimkan data ke Subscriber ketika ada perubahan atau event baru, seperti ketika sebuah produk dibuat atau dihapus. Ketika event ini terjadi, Publisher mengirimkan informasi tersebut melalui HTTP POST ke URL yang diberikan oleh Subscriber, yang berisi data terkait produk tersebut.

2. Keuntungan menggunakan Pull model adalah Subscriber memiliki kontrol penuh atas kapan mereka pull data dari Publisher. Hal ini berguna jika Subscriber ingin mengontrol frekuensi atau waktu pemanggilan data, menghindari beban dari pengiriman data yang tidak diperlukan Selain itu, Publisher tidak perlu terus menerus mengirimkan data ke banyak Subscriber. Sebaliknya, Subscriber yang akan mengambil data ketika mereka membutuhkannya.

Namun, Pull model memiliki kerugian seperti Subscriber harus mengatur dan mengelola polling atau permintaan berkala untuk menarik data dari Publisher, yang bisa meningkatkan kompleksitas dan menyebabkan keterlambatan. Subscriber mungkin tidak mendapatkan data terbaru sampai mereka melakukan permintaan baru, yang dapat mengakibatkan data yang tertinggal jika tidak ada mekanisme pembaruan yang baik.

3. Jika tidak menggunakan multi-threading dalam proses notification, maka proses pengiriman pemberitahuan ke semua Subscriber akan berjalan secara sinkron. Artinya, aplikasi akan mengirim pemberitahuan ke setiap Subscriber satu per satu, dan aplikasi akan menunggu hingga pemberitahuan ke Subscriber pertama selesai sebelum melanjutkan ke Subscriber berikutnya. Dengan ini, jika terdapat banyak Subscriber, aplikasi akan menjadi lebih lambat karena harus menunggu setiap permintaan HTTP selesai secara berurutan. Hal ini dapat mempengaruhi kinerja dan waktu respons aplikasi secara keseluruhan. Selain itu, aplikasi menjadi tidak efisien ketika jumlah Subscriber meningkat, karena hanya satu thread yang akan menangani pemberitahuan ke seluruh Subscriber, yang dapat menyebabkan bottleneck. Dengan menggunakan multi-threading,  dapat dikirim pemberitahuan ke banyak Subscriber secara paralel, yang mempercepat proses dan membuat aplikasi lebih responsif.


