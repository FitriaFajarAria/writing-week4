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

### Async

async function adalah fitur baru dari javascript yang ditambahkan pada tahun 2017. Sebelumnya kita telah belajar menulis kode asynchronous dengan menggunakan Promise, nah Async function adalah cara mudah untuk menjadikan fungsi apapun menjadi Promise dengan hanya menambah keyword async.

        / buat sebuah async function
        const myAsync = async function () {
        let x = 10
        if (x <= 10) {
        // Tidak perlu menggunakan resolve, cukup return
       // untuk memberi tanda bahwa async function selesai
        return "Nilai kurang dari atau sama dengan 10"
        }
        throw Error("Nilai lebih dari 10")
        }

        // Karena Async function menghasilkan Promise
       // maka kita bisa gunakan method then dan catch juga
        myAsync()
        .then((pesan) => {
         console.log(pesan)
        })
        .catch((err) => {
        console.log(err)
        })

### Await

Operator await digunakan untuk menunggu Promise selesai (resolved), nilai yang didapatkan oleh await bisa berupa Promise maupun data biasa dalam bentuk resolved Promised. await hanya bisa digunakan di dalam sebuah async function.

        const getData = async () => {
        let data = await window.fetch("...")
        let obj = await data.json() // dijalankan hanya jika variabel data terisi
        console.log(obj) // dijalankan hanya jika variabel obj terisi
        }

          getData().catch((err) => {
          console.log(err)
      })
      console.log("...")


# Git & Github Lanjutan (Kolaborasi)

Berkontribusi pada open source dapat menjadi cara yang bermanfaat untuk belajar, mengajar, dan membangun pengalaman dalam keterampilan apa pun yang dapat kita bayangkan.  CI, adalah strategi alur kerja yang memastikan semua modifikasi yang dibuat digabungkan dengan versi terbaru cabang master sangat sering untuk semua pengembang dalam tim, dalam pengembangan sumber terbuka. Contoh layanan yang menyediakan CI adalah GitHub, di mana setiap pengembang dapat membuat garpu (salinan independen) dari repositori untuk membuat Permintaan Tarik untuk meminta pekerjaan mereka (komit) untuk digabungkan, ditinjau dengan komentar dan saran, atau ditutup. Di dunia proyek software, tidak dapat dihindari bahwa kita akan menemukan diri kita bekerja dalam tim untuk menghasilkan proyek.

###  Organisasi & Kolaborator

#### Menambahkan Anggota Tim

Biasanya ada dua cara menyiapkan Github untuk kolaborasi tim:
1. Organizations - Pemilik organisasi dapat membuat banyak tim dengan tingkat izin yang berbeda untuk berbagai repositori.
2. Collaborators - Pemilik repositori dapat menambahkan kolaborator dengan akses Baca + Tulis untuk satu repositori.

- Organizations
Jika ingin mengawasi beberapa tim dan ingin menetapkan tingkat izin yang berbeda untuk setiap tim dengan berbagai anggota dan menambahkan setiap anggota ke repositori yang berbeda, maka Organization akan menjadi pilihan terbaik. Akun pengguna Github apa pun sudah dapat membuat Organizations gratis untuk repositori open source.
- Collaborators
Collaborators digunakan untuk memberikan akses Read + Write access ke satu repositori yang dimiliki oleh akun pribadi. Untuk menambahkan Collaborators, (akun pribadi Github lainnya), masing-masing Collaborator kemudian akan melihat perubahan dalam status akses pada halaman repositori. Setelah kita memiliki akses Write ke repositori, kita dapat melakukan git clone, bekerja pada perubahan, membuat git pull untuk mengambil dan menggabungkan setiap perubahan dalam repositori jarak jauh dan akhirnya git push, untuk memperbarui repositori jarak jauh dengan perubahan kita sendiri:

#### Pull Requests

Pull Requests adalah jika kita mau, kita dapat mengirim permintaan tarik ke pemilik repositori untuk menggabungkan perubahan kode kita. Pull request itu sendiri dapat memicu diskusi untuk kualitas kode, fitur atau bahkan strategi umum.
Ada dua model pull request di Github:
1. Fork & Pull Model - Digunakan di repositori publik yang tidak memiliki akses push
2. Share Repository Model - Digunakan dalam repositori pribadi yang kita miliki akses push.

####  Analytics

Github Graphs memberikan wawasan tentang kolaborator dan komitmen di balik setiap repositori kode, sementara Github Network menyediakan visualisasi pada setiap kontributor dan komitmennya di seluruh repositori bercabang. Analisis dan grafik ini menjadi sangat kuat, terutama ketika bekerja dalam tim.
- Graphs
Grafik menyediakan analisis rinci seperti:
1. Contributors: Siapa yang kontributor? Dan berapa banyak baris kode yang mereka tambahkan atau hapus?
2. Commit Activity: Minggu-minggu mana komit berlangsung dalam setahun terakhir?
3. Code Frequency: Berapa banyak baris kode yang dilakukan sepanjang siklus hidup proyek?
4. Punchcard: Selama waktu apa waktu melakukan biasanya dilakukan?

- Network
Github Network adalah alat canggih yang memungkinkan kita melihat setiap komitmen kontributor dan bagaimana mereka terkait satu sama lain. Ketika kita melihat visualizer secara keseluruhan, kita melihat setiap commit pada setiap cabang dari setiap repositori yang dimiliki jaringan.


# Responsive Web Design

Responsive web design (RWD) bertujuan untuk membuat design wesbite kita dapat diakses oleh divice apapun. Desain Web Responsif adalah pendekatan yang menyarankan bahwa desain dan pengembangan harus merespons perilaku dan lingkungan pengguna berdasarkan ukuran layar, platform, dan orientasi. Praktik ini terdiri dari campuran grid dan tata letak yang fleksibel, gambar, dan penggunaan kueri media CSS yang cerdas. Saat pengguna beralih dari laptop ke divice yang lain, situs web akan secara otomatis beralih untuk mengakomodasi resolusi, ukuran gambar, dan kemampuan skrip. Menambahkan viewport (Area pandang/area tampilan poligon dalam grafik komputer) kedalam HTML

                      <meta name="viewport" content="width=device-width, initial-scale=1.0">
                      
                      
### Media Query

Media query adalah fungsi dari css untuk menggunakan css tertentu jika syarat yang ditentukan dipenuhi. Media query merupakan komponen penting untuk membuat web responsive layout. Media query merupakan modul CSS3 yang berguna membuat layout kita responsive dengan menyesuaikan tampilan berdasarkan ukuran layar perangkat. 
Terkadang tampilan yang sudah kita desain dengan sedemikian rupa bisa kacau jika ditampilkan pada tampilan mobile. Dengan media query kita dapat menyelesaikan masalah ini dengan menentukan aturan ukuran dan tata letak elemen dengan kondisi-kondisi tertentu

Media query juga disebut dengan Breakpoint, karena cara kerja media query yakni dengan cara mengecheck ukuran viewport(layar/area dimana konten terlihat) apakah sesuai dengan kondisi yang kita deklarasikan, jika benar maka kode dalam kondisi tersebut yang akan dieksekusi. Dengan kata lain media query memberikan kemampuan menggunakan kode css yang sesuai dengan kondisi yang ditentukan.

                       <link rel="stylesheet" media="screen and (max-width: 300px)" href="s300.css">
                       
Perintah diatas menunjukan browser agar load file s300.css bila ukuran viewport <= 300px dengan media screen. Property media yang disupport adalah screen, print dan lain-lain Ada beberapa cara menggunakan media query

        - Melalu link tag seperti contoh diatas
        - Menggunakan @media
        - Menggunakan @import
                       
CSS media queries digunakan untuk membatasiÂ  ruang CSS, artinya CSS yang kita buat melalui media queries ini hanya berjalan di ukuran labar layar tertentu, misalnya:

                        @media screen and (max-width: 600px) {
                                 article{
                	background :red !important;
  	
                        }
                        }
                        
Dari contoh script di atas, dapat kita lihat bahwa property CSS background red pada article hanya berlaku pada ukuran maksimal 600px, ketika tampilan web lebih dari 600px maka background tersebut tidak akan berlaku lagi.   

#### Eksternal & internal media query
Kita dapat menggunakan media query dengan cara berikut

Cara 1:

Dengan menggunakan tag <link> di dalam elemen head

                <head>
                <link rel=”stylesheet” media=”screen and (min-width: 600px)” href=”laptop_styles.css”>
                <link rel=”stylesheet” media=”screen and (min-width: 320px) and (max-width: 360)” href=”mobile_styles.css”>
                </head>
                
Cara 2:

Kita definisikan dengan rule @media di dalam internal css atau file css terpisah   

                @media screen and (min-width: 240px) and (max-width: 480px) {
                p {
                font-size: 11px;
                }
                }
                
                
#### Media Features

Untuk menentukan kondisi kita bisa menyertakan media features di bawah ini dengan nilai batas nantinya sebuah rules akan dieksekusi. Media featurs harus berada dalam tanda kurung. 

1. width
2. height
3. device-width
4. device-height
5. aspect-ratio
6. device-aspect-ratio
7. color
8. color-index
9. monochrome
10. resolution
11. orientation
12. scan
13. grid
Contoh penggunaan

(orientation: landscape)
(orientation: potrait)
Beberapa memiliki min- dan max-. contoh

(min-width: 200px) 
(max-width: 760px)
(min-device-width: 200px)
(max-device-width: 800px)    


#Bootstrap 5

Bootstrap adalah framework HTML, CSS, dan JavaScript yang berfungsi untuk mendesain website responsive dengan cepat dan mudah. 
Framework open source ini diciptakan pada tahun 2011 oleh Mark Otto dan Jacob Thornton dari Twitter. Itulah kenapa dulunya Bootstrap dinamakan Twitter Blueprint. 
Bootstrap dengan cepat meraih popularitas digunakan oleh 27% website di seluruh dunia. Hal itu karena kesederhanaan dan konsistensi yang ditawarkan Bootstrap dibanding framework lainnya saat itu. Kemudahan yang ditawarkan oleh Bootstrap adalah Anda tak perlu coding komponen website dari nol. Framework ini tersusun dari kumpulan file CSS dan JavaScript berbentuk class yang tinggal pakai. Class yang disediakan Bootstrap juga cukup lengkap. Mulai dari class untuk layout halaman, class menu navigasi, class animasi, dan masih banyak lainnya. Menariknya lagi, Bootstrap bersifat responsive berkat grid system yang digunakan. Sistem grid pada bootstrap menggunakan rangkaian containers, baris, dan kolom untuk menyesuaikan bentuk layout dan konten website Anda. Dengan kata lain, Bootstrap menjamin tampilan website Anda akan tetap rapi dan konsisten di berbagai perangkat pengunjung. Baik melalui smartphone, tablet, atau laptop, kegunaan Bootsrap

- Menciptakan website Mobile Friendly —Berkat sistem grid, proses membuat website mobile friendly tak akan membutuhkan waktu lama.
- Memudahkan resize gambar — Cukup dengan menambahkan class .img-responsive ke gambar, maka gambar tersebut akan otomatis di-resize sesuai ukuran layar pengguna.
- Menambahkan elemen website tanpa ribet — Bootstrap menyediakan berbagai elemen yang bisa langsung Anda gunakan di website. Misalnya, navigasi, menu dropdown,           thumbnail, dan sebagainya.
- Membuat website lebih interaktif — Bootstrap juga memungkinkan Anda menggunakan plugin custom JQuery. Jadi, Anda bisa menambahkan berbagai elemen interaktif ke         website dengan mudah. Misalnya, popup, transisi, image carousel, dan sebagainya.

### Important globals 

Bootstrap menggunakan beberapa gaya dan pengaturan global yang semuanya hampir secara eksklusif diarahkan untuk normalisasi gaya lintas browser.

- Responsive meta tag 
Bootstrap dikembangkan terlebih dahulu untuk seluler, sebuah strategi untuk mengoptimalkan kode untuk perangkat seluler terlebih dahulu dan kemudian meningkatkan komponen seperlunya menggunakan kueri media CSS. Untuk memastikan rendering yang tepat dan zoom sentuh untuk semua perangkat, tambahkan tag meta area pandang responsif ke <head>.
                
                <meta name="viewport" content="width=device-width, initial-scale=1">
                
- Box-sizing 

                .selector-for-some-widget {
                box-sizing: content-box;
                }
                
- Reboot 

Untuk rendering lintas-browser yang lebih baik, kita menggunakan Reboot untuk memperbaiki ketidakkonsistenan di seluruh browser dan perangkat sambil memberikan pengaturan ulang yang sedikit lebih sesuai untuk elemen HTML umum.
