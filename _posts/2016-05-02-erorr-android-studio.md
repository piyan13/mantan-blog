---
title: "Erorr Android Studio 2.0 di linux 64bit"
tags:
 Linux
header:
  image: header.png
  caption: "kinfocenter kde"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
--- 
<figure style="width: 400px" class="align-center">
<img src="/images/android-studio.png">
<figcaption>android studio</figcaption>
</figure> 

Hal yang menarik dari [Android studio 2.0](http://developer.android.com/sdk/index.html) yang terinstall di linux 64bit adalah kutemukan
error disana sini, mungkin waktu kemarin masih pakek [linuxmint 17](https://www.linuxmint.com/) android studio dari versi 1.7 langsung ku update ke versi 2.0 jadi tidak ku temukan error yang berarti setelah migrasi ke [ubuntu 16.04](http://www.ubuntu.com/) dan install dari awal android studio 2.0 barulah berjumpa dengan errornya, apa saja errornya? ini dia check this out

## Unable to run mksdcard

<figure style="width: 600px" class="align-center">
<img src="/images/unable.png">
<figcaption></figcaption>
</figure> 
setelah finish install lalu pilih setting default atau custom dan munculah pesan error ini dikarenakan android studio yang terisntall di linux 64 bit butuh library 38bit,library apa saja yang diperlukan:

  * [lib32stdc++6](https://packages.debian.org/sid/libstdc++6) adalah library untuk progam C++ 
  * [lib32z1](https://packages.debian.org/jessie/lib32z1) adalah library implentasi method kompresi deflate yg terdapat di gzip dan pkzip

langsung buka terminal dan langsung install

```ruby
 ~:$ sudo apt-get install lib32stdc++6
 ~:$ sudo apt-get install lib32z1
```
<figure style="width: 600px" class="align-center">
<img src="/images/no-error.gif">
<figcaption></figcaption>
</figure>
  problem solved untuk unable to run mksdcard

## R layout cannot resolved

<figure style="width: 600px" class="align-center">
<img src="/images/eee.png">
<figcaption></figcaption>
</figure>
setelah error pertama unable to run mksdcard teratasi kemudian bukak project
gradle sync dan yooooott error kedua muncul mulai dari gradle build yang prosesnya lama sampai R layout cannot resolved. ada beberapa akibat munculnya error ini diantaranya adalah:

 * [gradle](http://gradle.org/) crash akibat tersimpannya beberapa versi gradle 
 * tidak singkronya antara sdk build tools project dengan sdk build tools yang dimiliki 
 
solusi untuk point yang pertama dengan menghapus salah satu versi gradle letaknya di /home/.AndroidStudio2.0/ setelah itu restart android studio.


<figure style="width: 600px" class="align-center">
<img src="/images/projectstruct.gif">
<figcaption></figcaption>
</figure>
 dan solusi untuk point yang kedua adalah merubah sdk build tools project dengan cara buka sdk build tools yang telah terisntall (ctrl+alt+shift+s) atau File--project Structure--app--Build Tools Version (seperti gambar diatas)  rubah dengan yang telah di install cara mengetahui build tools yang terisntall liat saja di sdk manager.
 <br>
akhirnya teratasi sudah, semua ERROR-nya mungkin kamu kelebihan error dari apa yang saya jumpai monggo ayo kita diskusikan