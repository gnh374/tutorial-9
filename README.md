## Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
- Unary RPC : Unary RPC melibatkan permintaan sederhana dari klien ke server, yang kemudian mengirimkan satu respons tunggal. Ini mirip dengan permintaan/respons HTTP tradisional. Cocok digunakan saat kita perlu mengambil sumber daya atau melakukan operasi sederhana

- Server Streaming RPC: Melibatkan satu permintaan dari klien yang memicu aliran respons dari server. Server mengirimkan kembali serangkaian pesan, tetapi klien hanya mengirim satu pesan. Cocok digunakan ketika server perlu mengirimkan sejumlah besar data ke klien, atau ketika server perlu mengirimkan pembaruan ke klien dari waktu ke waktu. Misalnya, streaming video atau pengiriman data log

- Bi-directional Streaming RPC: Melibatkan klien dan server yang saling mengirimkan aliran pesan satu sama lain. Ini memungkinkan saluran komunikasi dua arah di mana kedua pihak dapat mengirimkan dan menerima beberapa pesan. Cocok digunakan ketika klien dan server perlu berkomunikasi secara interaktif, seperti dalam aplikasi obrolan real-time, game multipemain, atau alat pengeditan kolaboratif

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
- Untuk autorisasi dan autentikasi, pada gRPC diperlukan validasi untuk tiap potongan data yang dikirimkan untuk menjamin keamanannya. Untuk enkripsi data, tiap potongan data akan dienkripsi secara terpisah untuk menjamin privasi.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
- Concurrency : mengatur concurrent stream dan memastikan bahwa thread safety cukup sulit. Karena konsep Rust ownership and borrowing system mungkin membutuhkan handling yang cukup hati-hati untuk menghindari data race.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Keuntungan :

- Integrasi dengan Tokio: ReceiverStream terintegrasi dengan baik dengan Tokio, yang merupakan runtime asinkronus populer untuk Rust. Hal ini memudahkan penggunaan dalam aplikasi Rust asinkronus, termasuk layanan gRPC.
- Kemudahan Penggunaan: ReceiverStream menyediakan API yang sederhana dan langsung untuk streaming respons. Ini dapat dengan mudah dibuat dari tokio::sync::mpsc::Receiver, yang umum digunakan untuk komunikasi antar-tugas dalam aplikasi Tokio.
- Fleksibilitas: ReceiverStream memungkinkan Anda untuk mendefinisikan logika sendiri untuk menghasilkan nilai yang akan di-stream. Ini memberi Anda fleksibilitas dalam bagaimana Anda menghasilkan dan menangani respons yang di-stream dalam layanan gRPC Anda

Kerugian:

- Terbatas pada Tokio: ReceiverStream khusus untuk runtime Tokio, yang berarti bahwa jika menggunakan runtime asinkronus yang berbeda (seperti async-std), kita perlu menggunakan solusi streaming yang berbeda.
- Kompleksitas dalam Penanganan Kesalahan: Menangani kesalahan saat menggunakan ReceiverStream bisa lebih kompleks dibandingkan dengan solusi streaming yang lebih sederhana. 
- Potensi Overhead Kinerja: Meskipun ReceiverStream menyediakan API yang nyaman, ada kemungkinan overhead kinerja dibandingkan dengan solusi streaming yang lebih rendah, terutama untuk aplikasi dengan persyaratan kinerja yang ketat

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
- Dengan menggunakan proto, kita bisa membuat layanan gRPC dan pesan dalam file yang terpisah. Ini dilakukan untuk meningkatkan modularitas.
- Gunakan kompiler protoc untuk menghasilkan kode daari file .proto. Hal ini dilakukan untuk memastikan konsistensi dan mengurangi risiko kesalahan dalam kode gRPC

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
- Bisa menggunakan server streaming dan tidak menggunakan unary untuk mengurangi overhead dalam komunikasi antara server dan client


7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
- gRPC menghasilkan kode klien dan server dalam berbagai bahasa berdasarkan definisi layanan dalam file `.proto`. Hal ini mendukung konsistensi di berbagai sistem  dan menghilangkan kebutuhan akan kode serialisasi dan deserialisasi manual.
- gRPC dikenal karena efisiensi dan performanya berkat penggunaan HTTP/2 untuk transport dan Protocol Buffers untuk serialisasi. Hal ini dapat menyebabkan komunikasi yang lebih cepat dan konsumsi sumber daya yang lebih rendah dibandingkan dengan protokol lain seperti REST

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

- Keuntungan HTTP/2: HTTP/2 memungkinkan beberapa request dan response dikirim dan diterima dalam 1 single connection. Hal ini meningkatkan efisiensi dan menurunkan latency jika dibandingkan HTTP/1.1
- Kerugian HTTP/2 : Bisa meningkatkan resource usage di server, terutama jika menangani concurrent connections dalam jumlah yang berat.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
- Dalam kontkes real-time communication gRPC lebih efficient dari pada REST APIs. Hal ini karena pada REST APIs, real-time communication dilakukan dengan client yang mengirimkan request berkali-kali sedangkan pada gRPC client hanya memerlukan 1 koneksi untuk mengirimkan banyak request.


10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
- Protokol Buffer berbasis skema yang ditentukan dalam `.proto` yang menggambarkan struktur data yang dikirim dan diterima. Sedangkan dalam JSON tidak ada skema yang ditentukan sehingga struktur data lebih fleksibel dan dinamis
- Ukuran file Protocol Buffer lebih kecil dibandingkan JSON karena menggunakan serialisasi biner dan formatnya sama semua

