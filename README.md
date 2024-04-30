# Advanced Programming - Tutorial


------------
## Overview

This repository contains the code and reflections for Tutorial 9 in Advanced Programming course by Vinka Alrezky As (NPM: 2206820200)❄️

------------
# Tutorial 9

### _Reflection_


> What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

   Berikut merupakan perbedaan utama antara _unary RPC, server streaming  RPC,_ dan _bi-directional streaming RPC_:
- **Unary RPC**: Komunikasi satu arah, dimana _client_ mengirim satu permintaan dan menerima satu respon, mirip dengan panggilan fungsi. Cocok untuk operasi sederhana dan transaksional yang tidak memerlukan aliran data.
- **Server Streaming RPC**: _Client_ mengirimkan satu permintaan dan menerima aliran data dari server. Ideal untuk skenario di mana _client_ perlu membaca serangkaian pesan. Data dikirim secara berurutan dari server ke _client_, misalnya mengirimkan log atau data _feed_.
- **Bidirectional Streaming RPC**: _Client_ dan server mengirim serangkaian pesan secara independen dan simultan. Ideal untuk kasus penggunaan yang memerlukan komunikasi dua arah _real-time_, seperti aplikasi obrolan atau game interaktif.


> What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Ketika mengimplementasikan layanan gRPC di Rust, ada beberapa pertimbangan keamanan yang perlu diperhatikan, terutama dalam hal autentikasi, otorisasi, dan enkripsi data:

- **Autentikasi**: Autentikasi dalam layanan gRPC di Rust memastikan hanya _client_ yang sah yang dapat mengakses layanan gRPC sebelum memberikan akses ke layanan. Ini dapat melibatkan penggunaan TLS untuk saluran komunikasi yang aman dan implementasi mekanisme autentikasi berbasis token seperti JWT (JSON Web Tokens) atau integrasi dengan sistem OAuth untuk otentikasi pengguna.
- **Otorisasi**: Otorisasi dalam konteks gRPC Rust mengelola kontrol akses _client_ berdasarkan peran dan izin yang ditetapkan. Hal ini memastikan bahwa hanya _client_ yang memiliki izin yang sesuai yang dapat mengakses fitur atau data tertentu dalam layanan.  Ini biasanya melibatkan kontrol akses berbasis peran (RBAC), di mana peran yang diberikan kepada pengguna menentukan izin yang dimilikinya. Middleware dapat diimplementasikan untuk memeriksa izin ini sebelum mengizinkan akses ke metode gRPC tertentu.
- **Enkripsi Data**: Enkripsi data penting dalam melindungi kerahasiaan informasi yang ditransmisikan antara _client_ dan server. Dalam implementasi gRPC Rust, data dienkripsi saat transit menggunakan protokol seperti TLS, sehingga mengamankan informasi sensitif dari ancaman penyadapan atau manipulasi.


> What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Beberapa tantangan dalam implementasi _bidirectional streaming_ di gRPC Rust, khususnya untuk aplikasi obrolan, meliputi:

- **Manajemen Aliran Pesan**: Menangani banyak pesan dan sesi pengguna secara bersamaan.
- **Sinkronisasi dan Penanganan Kesalahan**: Memastikan pesan diterima dalam urutan yang benar dan mengelola kesalahan transmisi.
- **Koneksi dan Timeout**: Mengatasi koneksi yang tidak stabil dan masalah timeout.
- **Penggunaan Sumber Daya**: Mengoptimalkan penggunaan CPU dan memori karena beban kerja yang tinggi.
- **Skalabilitas**: Menyesuaikan sistem untuk mendukung peningkatan jumlah pengguna dan pesan.
- **Keamanan**: Menjaga keamanan data dengan otentikasi dan enkripsi yang kuat.
- **Kompleksitas Implementasi**: Mengelola kompleksitas pengembangan dan pemeliharaan aplikasi streaming.

> What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

_**Advantages**_:
- **Kemudahan Konversi**: Memudahkan konversi `tokio::sync::mpsc::Receiver `menjadi Stream yang dapat digunakan dalam layanan gRPC.
- **Kemampuan Konkurensi**: Memungkinkan pemrosesan yang bersamaan, dengan banyak tugas yang menghasilkan respons dan mendorongnya ke `mpsc::Receiver`.
- **Integrasi dengan async/await**: Berfungsi dengan mulus dengan sintaksis `async/await` Rust, memudahkan penulisan kode asinkron.

_**Disadvantages**_:
- **Kompleksitas**: Menambah kompleksitas pada kode dan potensi _overhead_ dari abstraksi tambahan.
- **Ketergantungan pada Ekosistem**: Penggunaan `ReceiverStream` mengikat kode dengan ekosistem `Tokio`, yang mungkin tidak cocok dengan proyek yang menggunakan ekosistem lain. Ini dapat membatasi fleksibilitas dan portabilitas kode dalam lingkungan yang berbeda.

> In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Rust gRPC memberikan beberapa fitur yang memfasilitasi struktur kode yang mendukung penggunaan kembali dan modularitas, serta mempromosikan pemeliharaan dan perluasan kode dari waktu ke waktu:
- **Generated Code**: Rust gRPC secara otomatis menghasilkan kode untuk _client_ dan server berdasarkan definisi protokol. Ini memisahkan kode yang berkaitan dengan komunikasi jaringan dari implementasi bisnis inti. Dengan demikian, memungkinkan pengembang untuk fokus pada logika aplikasi mereka tanpa harus khawatir tentang detail implementasi jaringan.
- **Async/Await Support**: Rust memiliki dukungan bawaan untuk `async/await`, yang memungkinkan untuk menulis kode yang asinkron secara alami. Ini sangat berguna saat berurusan dengan operasi jaringan seperti yang dilakukan dalam gRPC. Dengan `async/await`, kode dapat ditulis dengan cara yang mudah dimengerti dan mudah dipelihara.
- **Trait-based Design**: Rust mendorong penggunaan trait untuk mendefinisikan _shared behavior_. Dalam konteks Rust gRPC, ini memungkinkan untuk mendefinisikan trait untuk operasi umum seperti koneksi ke server atau penanganan kesalahan. Dengan menggunakan trait, kode yang serupa dapat digunakan kembali di berbagai bagian aplikasi, meningkatkan modularitas dan keterbacaan.
- **Dependency Management**: Rust memiliki sistem manajemen dependensi yang kuat melalui Cargo. Ini memudahkan untuk menggunakan dan memperbarui dependensi, serta menjaga keselarasan versi yang konsisten. Dengan manajemen dependensi yang baik, memasukkan fungsionalitas tambahan atau memperbaiki bug dapat dilakukan dengan mudah dan aman.

> In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Dalam implementasi `MyPaymentService`, untuk menangani logika pemrosesan pembayaran yang lebih kompleks, langkah-langkah tambahan yang mungkin diperlukan adalah:
- **Validasi Input**: Memastikan setiap `PaymentRequest` yang diterima memiliki informasi yang valid dan lengkap. Ini termasuk memeriksa apakah user_id tidak kosong dan apakah jumlah pembayaran positif.
- **Autentikasi dan Otorisasi**: Verifikasi bahwa _client_ yang mengirimkan permintaan pembayaran memiliki izin yang sesuai untuk melakukan tindakan tersebut. Ini bisa melibatkan pengecekan token autentikasi dalam metadata permintaan dan verifikasi izin akses _client_.
- **Logika Bisnis**: Mengimplementasikan logika sebenarnya untuk pemrosesan pembayaran. Ini melibatkan pengurangan jumlah pembayaran dari saldo pengguna, penambahan ke saldo penerima, dan pencatatan transaksi. 
- **Penanganan Kesalahan**: Menangani segala kemungkinan kesalahan yang dapat terjadi selama proses pembayaran. Ini mencakup penanganan kasus-kasus seperti pembayaran gagal, kegagalan koneksi jaringan, atau masalah internal lainnya. Dengan memberikan tanggapan yang informatif kepada _client_ dan melakukan pemulihan yang diperlukan.

> What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Adopsi gRPC sebagai protokol komunikasi memiliki dampak signifikan pada arsitektur dan desain sistem terdistribusi, terutama dalam hal interoperabilitas dengan teknologi dan platform lain:
- **Efisiensi dan Performa**: gRPC menggunakan HTTP/2 yang lebih efisien daripada HTTP/1.1, memungkinkan komunikasi yang lebih cepat dan lebih banyak permintaan secara simultan.
- **Bahasa Agnostik**: gRPC mendukung berbagai bahasa pemrograman, memudahkan integrasi antara sistem yang ditulis dalam bahasa yang berbeda.
- **Skalabilitas**: Dengan kemampuan untuk menangani permintaan secara bersamaan dan dukungan untuk _load balancing_, gRPC memfasilitasi skalabilitas horizontal sistem terdistribusi.
- **Desain Berbasis Kontrak**: Menggunakan _Protocol Buffers_ untuk definisi API memastikan bahwa kontrak antara layanan jelas dan konsisten.
- **Kemudahan Pengembangan**: gRPC menyediakan alat yang mempermudah pembuatan stub dan skeleton, yang mengurangi kompleksitas pengembangan lintas platform.
- **Tracing dan Monitoring**: Arsitektur _pluggable_ gRPC mendukung _tracing_ dan _health checking_, yang penting untuk pemantauan dan pemecahan masalah dalam sistem terdistribusi.

Secara keseluruhan, gRPC meningkatkan kemampuan sistem terdistribusi untuk berinteraksi dengan berbagai layanan dan platform, sambil mempertahankan performa tinggi dan desain yang dapat diandalkan

> What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

HTTP/2, sebagai protokol dasar untuk gRPC, memiliki beberapa keuntungan dan kerugian dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk REST APIs:

_**Advantages**_:

- **Multiplexing**: Dapat menangani banyak permintaan dan respons secara bersamaan melalui satu koneksi, mengurangi waktu tunggu dan memperbaiki penggunaan _bandwidth_.
- **Server Push**: Mendukung pengiriman proaktif sumber daya dari server ke klien, mengurangi latensi dengan menghindari permintaan tambahan.
- **Protokol Binari**: Menggunakan format biner yang lebih efisien untuk komunikasi data, mempercepat proses analisis dan pengiriman data.
- **Kompresi Header**: Mengurangi overhead dengan menyusutkan ukuran header, terutama bermanfaat untuk permintaan dan respons kecil.
- **Pengendalian Arus**: Memiliki kontrol yang lebih baik atas aliran data, mencegah pengiriman yang berlebihan ke penerima.

_**Disadvantages**_:
- **Kompleksitas**: Memerlukan pemahaman yang lebih mendalam dan waktu yang lebih lama untuk mengimplementasikan dan memecahkan masalah.
- **Enkripsi**: Sering memerlukan enkripsi TLS yang memperkenalkan overhead tambahan, terutama jika tidak diimplementasikan secara efisien.
- **Interoperabilitas**: Tidak semua platform dan aplikasi mendukung HTTP/2, membatasi kemampuan untuk berinteraksi dengan sistem yang lebih tua atau yang tidak kompatibel.
- **Streaming**: Meskipun mendukung _streaming_, mekanisme ini tidak sefleksibel dan langsung seperti WebSocket, mengharuskan adaptasi khusus untuk pola komunikasi yang berkelanjutan.
- **Protokol Berbasis Teks**: Karena menggunakan format biner, sulit untuk membaca atau memodifikasi pesan secara langsung untuk tujuan _debugging_ atau pengembangan manual.

> How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

- REST APIs: Dalam REST API, _client_ mengirimkan permintaan dan menunggu respons dari server. Model ini sederhana dan berfungsi baik untuk banyak jenis aplikasi. Namun, tidak ideal untuk komunikasi _real-time_ karena _client_ tidak dapat menerima data dari server kecuali mengirimkan permintaan. Untuk mendapatkan pembaruan _real-time_, _client_ harus memeriksa server secara berkala, yang dapat tidak efisien dan meningkatkan beban pada server.
- gRPC Bidirectional Streaming: Dalam layanan gRPC dengan _Bidirectional Streaming_, _client_ dan server dapat mengirimkan dan menerima data kapan saja. Ini ideal untuk komunikasi _real-time_ karena server dapat mendorong pembaruan ke _client_ segera setelah tersedia. Ini juga memungkinkan untuk komunikasi yang lebih interaktif karena _client_ dan server dapat mengirimkan dan menerima pesan secara independen satu sama lain.
> What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Terdapat perbedaan yang signifikan antara penggunaan gRPC dengan Protocol Buffers dan penggunaan JSON dalam payload API REST. gRPC dengan Protocol Buffers mewajibkan struktur data yang ketat, memastikan konsistensi data antara _client_ dan server. Ini meminimalkan kemungkinan kesalahan saat aplikasi berjalan dan membuat proses pengiriman data lebih efisien. Namun, penggunaan Protocol Buffers memerlukan langkah tambahan dalam pengembangan aplikasi.

Di sisi lain, penggunaan JSON dalam payload API REST memberikan fleksibilitas yang lebih besar. Tanpa perlu struktur data yang terikat, JSON memungkinkan representasi data yang dinamis dan mudah diubah. Ini berguna saat aplikasi membutuhkan adaptasi cepat terhadap perubahan data. Selain itu, JSON mudah dibaca dan didukung oleh berbagai bahasa pemrograman dan _web browser_. Namun, penggunaan JSON dapat menghasilkan payload data yang lebih besar dan memakan waktu lebih lama dalam proses pengiriman.