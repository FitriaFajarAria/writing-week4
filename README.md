# writing-week4

# JavaScript Intermediate - Asynchronous - Fetch & JavaScript Intermdiate - Asynchronous - Async Await

## JavaScript Intermediate - Asynchronous - Fetch

Fetch API merupakan penyedia antarmuka JavaScript untuk mengakses dan memanipulasi bagian protokol, seperti permintaan dan tanggapan, metode fetch() global yang menyediakan cara mudah dan logis untuk mengambil sumber daya secara asinkron di seluruh jaringan. Fungsionalitas semacam ini sebelumnya dicapai menggunakan XMLHttpRequest. Fetch memberikan alternatif yang lebih baik yang dapat dengan mudah digunakan oleh teknologi lain seperti Service Worker, fetch juga menyediakan satu tempat untuk mendefinisikan konsep terkait HTTP lainnya seperti CORS dan ekstensi ke HTTP. Menggunakan permintaan melalui fetch():

        
             fetch('http://example.com/movies.json')
            .then((response) => response.json())
            .then((data) => console.log(data));

Di sini kita akan mengambil file JSON di seluruh jaringan dan mencetaknya ke consol, penggunaan paling sederhana dari fetch() membutuhkan satu argumen (jalur ke sumber daya yang ingin Anda ambil) dan tidak secara langsung mengembalikan badan respons JSON tetapi sebaliknya mengembalikan promise yang diselesaikan dengan objek Respons. Objek Respon, tidak secara langsung berisi badan respons JSON yang sebenarnya, tetapi merupakan representasi dari seluruh respons HTTP. Jadi, untuk mengekstrak isi JSON dari objek Response, kita menggunakan metode json() yang mengembalikan promise kedua yang diselesaikan dengan hasil parsing teks isi tanggapan sebagai JSON, Fetch secara lengkap:


              // Example POST method implementation:
            async function postData(url = '', data = {}) {
             // Default options are marked with *
            const response = await fetch(url, {
            method: 'POST', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json'
             // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer
            body: JSON.stringify(data) // body data type must match "Content-Type" header
            });
            return response.json(); // parses JSON response into native JavaScript objects
            }

            postData('https://example.com/answer', { answer: 42 })
            .then((data) => {
            console.log(data); // JSON data parsed by `data.json()` call
            });


- Uploading JSON data dengan fetch()


            const data = { username: 'example' };

            fetch('https://example.com/profile', {
            method: 'POST', // or 'PUT'
            headers: {
            'Content-Type': 'application/json',
            },
            body: JSON.stringify(data),
            })
            .then((response) => response.json())
            .then((data) => {
            console.log('Success:', data);
            })
            .catch((error) => {
            console.error('Error:', error);
            });

- Mengupload sebuah file

File dapat diunggah menggunakan elemen input <input type="file" /> HTML, FormData() dan fetch().


            const formData = new FormData();
            const fileField = document.querySelector('input[type="file"]');

            formData.append('username', 'abc123');
            formData.append('avatar', fileField.files[0]);

            fetch('https://example.com/profile/avatar', {
            method: 'PUT',
            body: formData
            })
            .then((response) => response.json())
            .then((result) => {
            console.log('Success:', result);
            })
            .catch((error) => {
            console.error('Error:', error);
            });

- Mengupload banyak file 

File dapat diunggah menggunakan elemen input <input type="file" multiple /> HTML, FormData() dan fetch().


            const formData = new FormData();
            const photos = document.querySelector('input[type="file"][multiple]');

            formData.append('title', 'My Vegas Vacation');
            let i = 0;
            for (const photo of photos.files) {
            formData.append(`photos_${i}`, photo);
            i++;
            }

            fetch('https://example.com/posts', {
            method: 'POST',
            body: formData,
            })
            .then((response) => response.json())
            .then((result) => {
            console.log('Success:', result);
            })
            .catch((error) => {
            console.error('Error:', error);
            });

- Menyediakan request objek 

kita bisa membuat objek permintaan menggunakan konstruktor Request(), Request() menerima parameter yang sama persis dengan metode fetch() dan meneruskannya sebagai argumen metode fetch():

          const myHeaders = new Headers();

          const myRequest = new Request('flowers.jpg', {
          method: 'GET',
          headers: myHeaders,
          mode: 'cors',
          cache: 'default',
          });

          fetch(myRequest)
          .then((response) => response.blob())
          .then((myBlob) => {
          myImage.src = URL.createObjectURL(myBlob);
          });

- Objek respons

Seperti yang kita lihat di atas, Instance respons dikembalikan saat promise fetch() diselesaikan. Properti respons paling umum yang akan digunakan adalah:
  - Response.status
  - Response.statusText
  - Response.ok 
  
### Fetching data from the server

Hal lain yang sangat umum di situs web dan aplikasi modern adalah mengambil item data individual dari server untuk memperbarui bagian halaman web tanpa harus memuat seluruh halaman baru. Detail yang tampaknya kecil ini berdampak besar pada kinerja dan perilaku situs. Halaman web terdiri dari halaman HTML dan (biasanya) berbagai file lain, seperti stylesheet, skrip, dan gambar. Model dasar pemuatan halaman di Web adalah browser membuat satu atau lebih permintaan HTTP ke server untuk file yang diperlukan untuk menampilkan halaman, dan server merespons dengan file yang diminta. Jika kita mengunjungi halaman lain, browser meminta file baru, dan server meresponsnya. Banyak situs web menggunakan API JavaScript untuk meminta data dari server dan memperbarui konten halaman tanpa memuat halaman. Jadi ketika pengguna mencari produk baru, browser hanya meminta data yang diperlukan untuk memperbarui halaman.

API utama di sini adalah Fetch API. Ini memungkinkan JavaScript yang berjalan di halaman untuk membuat permintaan HTTP ke server untuk mengambil sumber daya tertentu. Saat server menyediakannya, JavaScript dapat menggunakan data untuk memperbarui halaman, biasanya dengan menggunakan API manipulasi DOM. Data yang diminta seringkali berupa JSON, yang merupakan format yang baik untuk mentransfer data terstruktur, tetapi bisa juga berupa HTML atau hanya teks.

                  const verseChoose = document.querySelector('select');
                  const poemDisplay = document.querySelector('pre');

                  verseChoose.addEventListener('change', () => {
                  const verse = verseChoose.value;
                  updateDisplay(verse);
                  });


Ini menyimpan referensi ke elemen <select> dan <pre> dan menambahkan listener ke elemen <select>, sehingga ketika kita memilih nilai baru, nilai baru diteruskan ke fungsi bernama updateDisplay() sebagai parameter. Fetch API adalah fungsi global yang disebut fetch(), yang menggunakan URL sebagai parameter selanjutnya, fetch() adalah API asinkron yang mengembalikan Promise. Jadi karena fetch() mengembalikan promise, =kita meneruskan fungsi ke metode then() dari promise yang dikembalikan, metode ini akan dipanggil ketika permintaan HTTP telah menerima respons dari server. Di handler, memeriksa apakah permintaan berhasil dan membuat kesalahan jika tidak. Jika tidak, maka memanggil response.text(), untuk mendapatkan isi respons sebagai teks. Ternyata response.text() juga tidak sinkron, jadi mengembalikan promise yang dikembalikannya, dan meneruskan fungsi ke metode then() dari promise baru ini. Fungsi ini akan dipanggil ketika teks respons sudah siap, dan di dalamnya kita akan memperbarui blok kita dengan teks. Contoh blok pertama yang menggunakan Fetch dapat ditemukan di awal JavaScript:



                   fetch('products.json')
                   .then((response) => {
                   if (!response.ok) {
                   throw new Error(`HTTP error: ${response.status}`);
                   }
                   return response.json();
                   })
                   .then((json) => initialize(json))
                   .catch((err) => console.error(`Fetch problem: ${err.message}`));



Terakhir, kita merangkai handler catch() di bagian akhir, untuk menangkap error yang terjadi di salah satu fungsi asinkron yang kita panggil atau handlernya. Fungsi fetch() mengembalikan promise jika ini berhasil diselesaikan, fungsi di dalam blok .then() pertama berisi respons yang dikembalikan dari jaringan.


## JavaScript Intermdiate - Asynchronous - Async Await

# Git & Github Lanjutan (Kolaborasi)


# Responsive Web Design


#Bootstrap 5
