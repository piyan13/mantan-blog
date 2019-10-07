---
title: "Fragment di WarPlos"
tags:
 Android
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
{% include toc title="Contents" icon="file-text" %}
Studi kasus kali ini adalah tentang Fragment dengan app sederhana warplos yaitu app yang menampilkan profil usaha kecil menengah di daerah Bangkingan RW2 surabaya dimana pokok pembahasannya adalah nantik warplos bisa menyesuaikan tampilan saat di run di phone atau tablet,
{: style="text-align: justify;"}

Ketika di run di phone dia akan menampilkan terpisah antara Listview dengan FragmentDetail class maksudnya dengan 2screen yang berbeda, sebaliknya untuk tablet karena memiliki screen yang large dia ListView dan DetailFragment akan ditampilkan dalam satu screen (salah satu keunggulan Fragment).
{: style="text-align: justify;"}

## Structur app warplos
 jadi di app Warplos nantinya ada 2activity yaitu MainActivity dan WarPlosDetailActivity dimana untuk MainActivity akan berfungsi menampilkan list jika FragmentContainer tidak sama dengan null yang berarti akan menampilkan layour-large untuk tablet maka DetailFragment akan ditampilkan dalam satu screen dan jika sebalikna Fragment container null maka layout akan merujuk pada standart layout yang berart jika list di klik maka akan memanggil class WarPlosDetailActivity, kemudian app Warplos memiliki 2 class Fragment yaitu WarPlosListFragment dan WarPlosDetailFragment dimana untuk class WarPlosListFragment berisi list beberapa nama umkm dan untuk class WarPlosDetailFragment menampilkan data detail dari nama umkm yang clik di layar dan jugak di app Warplos terdapat class WarplosData dimana class tersebut berisi tentang array yang diperlukan oleh class WarPlosDetailFragment dan WarPlosListFragment.
 {: style="text-align: justify;"}

{% capture warplos_img %}
![Foo]({{"/assets/images/backsack.gif" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan warplos di tablet</figcaption>
</figure>


{% capture warplos_img %}
![Foo]({{"/assets/images/waplosanim.gif" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan warplos di di phone</figcaption>
</figure>

## langkah langkah pengerjaan WarplosApp

Langkah awal yang saya buat adalah membuat class WarplosData berisi data array nama dan descripsi dari umkm di bangkingan rw2 begini umlnya 
{: style="text-align: justify;"}

{% capture warplos_img %}
![Foo]({{"/assets/images/WarplosData.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>uml class WarplosData</figcaption>
</figure>

yang nantinya data data dari class ini dipanggil oleh class WarPlosDetailFragment dan WarPlosListFragment.

lalu saya membuat class WarPlosDetailFragment untuk mencoba menampilkan Data detail random yang diambil dari class WarplosData berikut umlnya:

{% capture warplos_img %}
![Foo]({{"/assets/images/warplosdetailFragment.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>uml class WarPlosDetailFragment</figcaption>
</figure>

setelah update layout DetailFragment dan memanggilnya di MainActivity percobaan pertama hasinya seperti gambar dibawah ini

{% capture warplos_img %}
![Foo]({{"/assets/images/testdrivedetailwarplos.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>test data deskripsi</figcaption>
</figure>

selanjutnya adalah mencoba menampilkan satu layar dengan dua Fragment untuk tampilan yang akan peruntukkan table untuk itu saya membuat class WarPlosListFragment seperti ini umlnya:


{% capture warplos_img %}
![Foo]({{"/assets/images/warploslistfragment.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>uml class WarPlosListFragment</figcaption>
</figure>
lalu update layoutnya dan memanggilnya di dalam method onCreate class MainActivity tampilannya sebagai berikut

{% capture warplos_img %}
![Foo]({{"/assets/images/separateScreenWarplos.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan 2fragment satu screen</figcaption>
</figure>
alhamdulillah semua data bisa ditampilkan di screen baik untuk list nama dan deskripsi selanjutnya adalah membuat bagaimana tampilan saat WarPlos berjalan di phone dan tablet menjadi berbeda maka saya buat folder layout lagi di directory res dengan nama layout-large dimana ini akan berisi layout activity_main.xml yang berbeda isi dengan yang terdapat di directory layout kali ini saya jugak akan membuat class WarPlosDetailActivity untuk mengontrol class WarPlosDetailFragment dan sebagai tujuan intent dari class MainActivity saat WarPlos berjalan di phone berikut umlnya
{: style="text-align: justify;"}

{% capture warplos_img %}
![Foo]({{"/assets/images/waplosdetailactivity.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>uml clas WarPlosDetailActivity</figcaption>
</figure>

dan langkah terkahir adalah update class MainActivity seperti ini umlnya

{% capture warplos_img %}
![Foo]({{"/assets/images/mainactivityWarplos.png" | relative_url}})
{% endcapture %}
<figure>
	{{warplos_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>uml clas MainActivity</figcaption>
</figure>
ada beberapa catatan misal backstack transaction di fragment gimana fungsinya adalah ketika user klik tombol kembali waprlos akan menuju list yang sebelumnya di klik , dan untuk issues ketika app berjalan tiba tiba kita ganti rotasi device itu jugak menjadi tambahan tambahan untuk pembedahan Fragment kali ini dan saya sempet jugak mengamali deadlock saat menuliskan di blog ini ..... done!
{: style="text-align: justify;"}
