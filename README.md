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
  

## JavaScript Intermdiate - Asynchronous - Async Await

# Git & Github Lanjutan

# Responsive Web Design


#Bootstrap 5
