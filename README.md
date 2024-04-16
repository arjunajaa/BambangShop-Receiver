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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Ketika kita mempertimbangkan penggunaan antara Mutex<> dan RwLock<>, kita perlu memperhatikan sifat akses ke data yang dibutuhkan oleh thread-thread dalam aplikasi kita. Mutex<> memungkinkan akses eksklusif ke data, artinya hanya satu thread yang dapat mengakses data pada satu waktu. Di sisi lain, RwLock<> memungkinkan pembacaan data oleh beberapa thread secara bersamaan, namun hanya memungkinkan penulisan oleh satu thread pada satu waktu.
<br><br> Jika kita memiliki skenario di mana pembacaan notifikasi sering dilakukan oleh beberapa thread secara bersamaan, penggunaan RwLock<> mungkin lebih cocok. Ini karena RwLock<> memungkinkan banyak thread untuk membaca data notifikasi tanpa mengalami blocking satu sama lain. Namun, perlu diingat bahwa jika terdapat operasi penulisan yang jarang terjadi tetapi kritis, kita harus memperhatikan bagaimana RwLock<> akan mempengaruhi performa aplikasi, karena penulisan akan memerlukan akses eksklusif dan mungkin menghentikan semua pembacaan.
<br> <br> Jadi, dalam situasi di mana pembacaan notifikasi sering diakses oleh banyak thread secara bersamaan, RwLock<> menjadi pilihan yang lebih cocok karena memungkinkan pembacaan bersamaan tanpa mengorbankan keselamatan data.

2. Aturan kepemilikan dan peminjaman di Rust bertujuan untuk mencegah mutasi variabel status di luar konteks awal mereka. Ini dilakukan untuk menghindari potensi kesalahan seperti perlombaan data di mana dua atau lebih thread berlomba untuk mengakses atau memodifikasi data yang sama secara bersamaan. Dengan menerapkan aturan ini, Rust memastikan keamanan thread dengan memastikan bahwa hanya satu thread yang memiliki akses untuk memodifikasi data pada suatu waktu, sehingga mengurangi risiko kecacatan terkait thread.

#### Reflection Subscriber-2

1. Setelah saya melakukan eksplorasi pada file lib.rs, saya menyimpulkan bahwa secara keseluruhan, file ini bertanggung jawab untuk mengatur dan menentukan kerangka utama dari proyek Rust ini. Ini termasuk menginisialisasi variabel global, mendefinisikan struktur data yang diperlukan, serta mengimplementasikan fungsi-fungsi yang krusial untuk aplikasi ini.
<br> <br> Dari apa yang saya lihat, lib.rs ini sepertinya menjadi titik awal dari seluruh aplikasi, di mana segala sesuatu dimulai dan dikendalikan. Ini adalah tempat di mana kita menetapkan fondasi yang dibutuhkan untuk menjalankan aplikasi dengan lancar, dan tempat di mana kita mendefinisikan struktur data yang akan digunakan di seluruh aplikasi. Jadi, dapat dikatakan bahwa lib.rs adalah jantung dari proyek Rust ini.

2. Dengan menggunakan Observer pattern, kita bisa mengurangi ketergantungan antara Main App dan Receiver, sehingga perubahan atau penambahan receiver tidak akan berdampak pada bagian inti aplikasi. Hal ini karena setiap receiver berfungsi sebagai observer yang independen, dan mereka hanya akan menerima notifikasi ketika ada perubahan yang relevan dengan mereka. Ini memungkinkan fleksibilitas yang lebih besar dalam mengelola dan memperluas aplikasi tanpa harus khawatir tentang efek samping pada bagian utama aplikasi.
<br> <br> Selain itu, penambahan instance Main App juga menjadi lebih mudah dengan penggunaan Observer pattern. Setiap instance dari Main App dapat dengan mudah mengelola daftar observernya sendiri, sehingga kita tidak perlu khawatir tentang interaksi antara instance-instance tersebut. Namun, jika kita menginginkan semua observer menerima notifikasi dari setiap instance Main App, kita perlu menambahkan logika tambahan untuk menyatukan komunikasi antar instance. Hal ini mungkin memerlukan implementasi tambahan untuk menyinkronkan informasi antara instance-instance tersebut, tetapi dapat memberikan manfaat yang besar dalam memperluas fungsionalitas aplikasi secara keseluruhan.

3. Meskipun saya belum memiliki pengalaman dalam membuat test sendiri atau mengembangkan dokumentasi pada Postman Collection, saya melihat nilai besar dalam praktek tersebut. Saya percaya bahwa melakukan pengujian dan dokumentasi dengan cara ini dapat sangat membantu memastikan kelancaran aplikasi.
<br> <br> Pengujian memungkinkan kita untuk secara sistematis memeriksa respons dari setiap permintaan atau tindakan dalam aplikasi, memastikan bahwa mereka sesuai dengan harapan. Ini membantu mengidentifikasi dan memperbaiki masalah dengan cepat, serta memastikan bahwa setiap kasus dalam aplikasi sudah ter-cover.
<br> <br> Sementara itu, pengembangan dokumentasi pada Postman Collection juga dapat memberikan nilai tambah yang signifikan. Hal ini memungkinkan kita untuk secara jelas mendokumentasikan setiap endpoint API, parameter yang diperlukan, dan respons yang diharapkan. Ini membantu pengguna atau pengembang lain memahami dan menggunakan API dengan lebih efektif.
<br> <br> Secara keseluruhan, meskipun saya belum mencobanya sendiri, saya melihat bahwa praktik ini merupakan bagian penting dari pengembangan aplikasi yang profesional dan dapat memberikan manfaat yang besar dalam memastikan kualitas dan kelancaran aplikasi.
