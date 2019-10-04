---
title: "Lebih dalam tentang RecyclerView"
tags:
 Android
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
<figure style="width: 700px" class="align-center">
<img src="/images/udaRecycle.gif">
<figcaption>RecyclerView shunshine udacity</figcaption>
</figure>
{% include toc title="Contents" icon="file-text" %}
Mei tahun 2017 begitu membara cukup terseok seok hanya untuk sekedar istiqomah dan waktunya untuk mengupas habis beberapa ketertinggalan diving in biar gak makin tertinggal okey RecyclerView di perkenalkan pada saat android L adalah sebuah ViewGroup baru yang di persiapkan untuk render view adapter menurut mentor di udacity RecyclerView menjadi inti dari banyak aplikasi dengan alesan untuk hal hal yang compleks penangannya daripada ListView isu utama dari RecylcerView adalah mengenai garbage collection memory yang terpakai saat menampilkan data dengan banyak nan kompleks
{: style="text-align: justify;"}

Modular hampir di setiap bagian dari RecyclerView dan untuk mendalaminya saya harus merasa nyaman dengan element elementnya kayak `RecyclerView.Adapter` , `LayoutManager` , `ItemAnimator`  dll berangkat dari sana mencoba untuk menelaah satu persatu 
{: style="text-align: justify;"}
## Adapter
Adapter dipanggil RecyclerView untuk membuat item baru dalam bentuk ViewHolder,adapter jugak mengumpulkan atau menggabungkan item dengan data dan return informasi mengenai tentang data seperti banyaknya item di dalam source data, Adapter meminta kita untuk meng override tida method yaitu:
{: style="text-align: justify;"}
 * onCreateViewHolder yang dipanggil ketika RecyclerView menginstanisasi intance ViewHolder
  {: style="text-align: justify;"}
 * onBindViewHolder method ini dipanggil ketika RecyclerView ingin mengisi view dengan data 
 * getItemCount yang return jumlah dari item ke dalam data source

<figure style="width: 700px" class="align-center">
<img src="/images/methodAdapter.gif">
<figcaption>ilustrasi override method Adapter</figcaption>
</figure>
{: style="text-align: justify;"}

## LayoutManager
LayoutManager yang menentukan colletion item ditampilkan dan LayoutManager menjadi bagian terpenting cara mendaur ulang bekerja di RecyclerView karena LayoutManager yang menentukan kapan mendaur ulang tampilan item yang tidak lagi terlihat saat pengguna menggulirkan layar
{: style="text-align: justify;"}

<figure style="width: 700px" class="align-center">
<img src="/images/GreenRecyclerView.gif">
<figcaption>app RecyclerView</figcaption>
</figure>

Memang betul dalam prakteknya konsep ViewGroup RecyclerView sulit tapi setidaknya dengan pemaparan di udacity hasilnya seperti gambar diatas saya jadi sedikit banyak terbantu karena fungsinya sangat penting dari sekedar ListView jadi makin tak sabar untuk explore lebih jau lagi dan lagi 

