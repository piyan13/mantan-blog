---
title: "Bedah MVP pattern di Android"
tags:
 Android
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---

<figure style="width: 400px" class="align-center">
<img src="/images/mvp.png">
<figcaption>credit: androidpub</figcaption>
</figure> 
{% include toc title="Contents" icon="file-text" %}
Akhirnya tiba di tahun 2018 jugak, berarti  blog minimalis ini menginjak tahun ke-2 masih kurang produktif dalam menulis kebanyakan membaca.. semoga awal tahun ini lebih produktif lagi dalam tulis menulis dan bongkar membongkar tiba-tiba saja kepikiran untuk mempelajari Arsitektur android lebih jauh biar gak makin boring terinspirasi dari tulisan tulisan para developer pro di belahan dunia sana,karena begitu fleksibel dan kebebasan dalam dalam menentukan arsitektur sebuah app ini pada akhirnya memunculkan masalah masalah yang umum seperti class besar, skema penamaan yg tidak konsisten, itu membuat pengujian, pemeliharaan dan perluasan aplikasi menjadi sulit.
{:style="text-align: justify;"}

## jadi apa itu mvp?
Pengertian Model View Presenter bisa liat di [Wikipedia-MVP](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter) simplenya adalah memisahkan logic dan view. biasanya logic atau logic business ada di presenter , view bertugas menangani kerja UI, mengkonversi perintah presenter untuk UI, lalu mendengarkan aksi dari user yang di teruskan ke UI.
{:style="text-align: justify;"}

## kenapa memakai mvp?
untuk alesan testing dan pemeliharaan lah arsitektur mvp dipakai karena faktanya android activities digabungkan erat antara mekanisme UI dan akses data contohnya adalah CursorAdapter. dengan memisahakan 3layer secara independent itu berarti jugak kita bisa test mereka secara mandiri
{:style="text-align: justify;"}

## ringkasan
perbedaan pola MVC dan MVP sangat tipis dimana untuk disebut MVC ketika activity sebagai controller. MVP sangat bervareatif penggunaannya sesuai dengan masalah apa yang akan di selesaikan. dan untuk app yang berskala kecil dan tidak akan berkembang di masa depan, pendekatan pola MVP ini tidak akan banyak membantu.
{:style="text-align: justify;"}
