---
title: "AsyncTask 🆚 AsyncTaskLoader"
tags:
 Android
header:
    image: Asyntask.png
    caption: "android developer"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
selamat datang di bulan ramadhan akhirnya satu tahun mengudara blog minimalis ini.Beberapa hari ini saya terlibat praktek terlalu dalam dengan LifeCycle android termasuk mengenai dasar Thread seperti diketahui android support multitasking dimana setiap aplikasi android dibagi menjadi multiple thread semuanya dapat berjalan bersamaan dalam satu kali eksekusi,eksekusi multiple thread ini di jadwalkan oleh operating system untuk berjalan di core CPU yang berbeda, untuk mempermudah developer android apps di android memiliki satu thread user interface yang mana bertanggung jawab untuk getting event dari berbagai macam sensor dan selanjutnya mengatur frame untuk menggambarnya agar berjalan pada 60 frame per detik karena itulah belajar thread LifeCycle ini sangat fundamental sekali dalam kasus yang saya temui sebisa mungkin meminimkan proses di thread utama karena akan membuat aplikasi menjadi force close.
{: style="text-align: justify;"}
{% include toc title="Contents" icon="file-text" %}
<figure style="width: 600px" class="align-center">
<img src="/images/Fetchweater.png">
<figcaption></figcaption>
</figure> 
## AsyncTask ##
Dan class AsyncTask adalah jawaban bagaimana cara meminimalisir proses Thread utama terlebih di aplikasi yang saya bangun memuat atau mengambil data dari internet, dengan method doInBackground() proses pengambilan data Json dari server tidak membuat lama Thread utama memproses yang berakibat di destroynya activiy secara default sesuai LifeCycle android setelah 5 detik mengabaikan input pengguna android akan menganjurkan pengguna untuk menutup aplikasi.
{: style="text-align: justify;"}
<figure style="width: 400px" class="align-center">
<img src="/images/explainAsynctask.gif">
<figcaption></figcaption>
</figure> 
AsyncTask adalah generic class berarti membutuhkan type berparameter di constructornya, setiap generic parameter adalah untuk menemukan argument variable java secara teknis di passing sebagai array karena mempunya 3 titik, ada tiga type generic AsyncTask :

 **1.** `params`, type yang dikirim ke task setelah eksekusi
 
 **2.** `progress`,type yang di publikasikan untuk memperbarui progress digunakan di komputasi Background
 
 **3.** `result`, adalah hasil dari komputasi di Background
 
**Note:** tidak semua type slalu digunakan untuk menandai yang tidak digunakan cukup gunakan type `Void`
{: .notice--info}

```java
private class MyTask extends AsyncTask<Void, Void, Void>{...}
```
 untuk menjalankan AsyncTask cukup dengan memanggil `execute` dengan parameter selanjutnya AsyncTask menjalankan beberapa langkah:
 
 **1.** memanggil `onPreExecute` di thread UI jadi kita dapat menginisialisasi apapun yang inginkan di thread ui sebelum task background dimulai
 
 **2.** kemudian akan memanggil `doInBackground` di thread lain tempat tas yang berjalan lama di eksekusi ini satu satunya method yanag harus di override untuk menggunakan class dan cara memanggilnya menggunakan parameter yang kita passing ke `execute` jika kita ingin memperbarui ui dengan `progress` dari task yang di eksekusi dalam waktu yang lama kita akan memanggil `publishProgrees` dengan parameter ini akan menyebabkan `onProgressUpdate` di panggil di thread ui
 {: style="text-align: justify;"}
 
 **3.** dan terakhir return hasil task yang dijalankan di `doInBackground` dan menyebabkan method `onPostExecute` di panggil di thread ui dengan hasil yang telah di return dari doInBackground
 
okey untuk membuat asynchronous dari thread background dan thread user interface memang berhasil dan membuat thread utama tidak sampai force close jugak awal yang baik untuk coding dengan thread yang banyak akan tetapi masalah baru muncul ketika device di putar atau di rotate diwaktu transaksi jaringan sedang berlangsung dalam hal ini parse json dari server maka activity dimulai ulang, activity yang baru akan membuat asynctask lain untuk melakukan task dibackground dan akan ada penggunaan jaringan tambahan karena kedua thread berjalan secara paralel juga memerlukan waktu yang lebih lama lagi untuk user agar bisa melihat hasil pemuatan parahnya lagi akan menambah tekanan di memory itulah kenapa AsyncTask tidak slalu benar dalam semua kondisi
 {: style="text-align: justify;"}
 
<figure style="width: 600px" class="align-center">
<img src="/images/asyncproblem.gif">
<figcaption></figcaption>
</figure> 
## AsyncTaskLoader ##
 Untuk permasalahan diatas terjawab dengan `Loader` yang menyediakan Framework untuk asynchronous loading data didaftarkan oleh id dengan komponen yang disebut `LoadManager` yang memungkinkan mereka hidup diluar LifeCycle activity yang terkait mencegah pemuatan duplikat yang terjadi secara paralel jika ingin load data di thread background kita dapat mengimplentasikan Loader yang disebut `AsyncTaskLoader`yang jugak mengimplementasikan fungsi yang sama dengan `AsyncTask` tapi dengan siklus yang berbeda.
 {: style="text-align: justify;"}
 <figure style="width: 600px" class="align-center">
<img src="/images/asyncloader.gif">
<figcaption></figcaption>
</figure> 
`Loader` terikat LifeCycle app secara otomatis menangani perubahan seperti rotasi, loader di desain untu memuat ulang jika user keluar dari activity kita dapat menghindari beban tambahan jika tida suka dengan mencache dan mengirimkan kembali hasil yang ada.
