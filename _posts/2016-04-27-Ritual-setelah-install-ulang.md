---
title: "Ritual setelah install Linux"
tags:
 Linux
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---

<figure style="width: 400px" class="align-center">
<img src="/images/ubuntu16.jpg">
<figcaption>credit: google</figcaption>
</figure> 

 {% include toc title="Contents" icon="file-text" %}

 Setelah denger [ubuntu](http://www.ubuntu.com) launch versi LTS 
 (long terms support) di tahun ini, langsung saya bergegas
 untuk [download](http://www.ubuntu.com/download) ,sudah menjadi kebiasaan dari dulu untuk memakai versi LTS varian distro yang satu ini ya untuk harian memang sangat stable, tapi minusnya kurang update
 inovasi terbaru dari technology opensourcenya dimana makin hari makin cepat pergerakannya, selesai install ubuntu ini beberapa ritual saya untuk menyempurnakan dekstop untuk digunakan sehari hari
 
 
## Install Driver wireless
 untuk laptop saya thinkpad edge e145 driver kebetulan wirelessnya gak langsung
 detect alias harus di install sendiri cukup konekin modem buka terminal dan

```ruby
 :~$ sudo apt-get install bcmwl-kernel-source
```
 kebetulan hardware laptop saya pakek broadcom kalau sudah terinstall otomatis akan bisa detect Acces point (SSID) terdekat 
 
## Install codecs
 menurut [wikipedia](https://en.wikipedia.org/wiki/Codec) codec adalah progam
 computer untuk encoding atau decoding data digital. biasa kalau setelah install waktu play music player atau film di video player dia akan bisu dan kalau di telusuri di terminal akan mengeluarkan pesan tidak ditemukannya codec 
 kalau di ubuntuk tinggal klik perintah ini di terminal
 
```
 :~$ sudo apt-get install ubuntu-restricted-extras
```
  kalau sudah done coba dengan memainkan mp3 dan memainkan video dalam format apapun untuk memastikan codec berjalan dengan sempurna
  
## Install Chrome
 Browser bawakan ubuntu adalah firefox dan saya lebih selera dengan Chrome 

```ruby
:~$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
:~$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## Install git
 Berikutnya adalah yang tak kalah pentingnya yang mesti di install version control [GIT](https://en.wikipedia.org/wiki/Git_%28software%29) 

```html
:~$ sudo apt-get install git 
:~$ git init
``` 
lanjut deh dengan `git clone` untuk membuat repository local projectnya

## Install Oracle Java
 Secara default Ubuntu terinstall [openjdk java](https://en.wikipedia.org/wiki/OpenJDK) untuk kebutuhan saya android studio menyarankan pakek jdk dari oracle
 maka dari itu biar gak crash nantinnya saya uninstall openjdk dulu dengan

```ruby
:~$ sudo apt-get purge openjdk-8-jre
:~$ sudo apt-get remove openjdk-8-jre-headless
:~$ sudo rm -rfv /usr/lib/jvm/java-8-openjdk-amd64/
```
 lalu kita install java versi oraclenya

```ruby
:~$ sudo add-apt-repository ppa:webupd8team/java
:~$ sudo apt-get update
:~$ sudo apt-get install oracle-java8-installer
```
untuk memastikan java suda terinstall dengan sempurna

```ruby
:~ java -version
```
kalau berhasil akan muncul seperti gambar
<figure style="width: 600px" class="align-center">
<img src="/images/bash.png">
<figcaption></figcaption>
</figure> 

## Install Jekyll

karena saya ngeblog dengan bantuan [jekyll](https://jekyllrb.com/) maka selanjutnya adalah menginstall jekyll yang dibutuhkan sebelum install jekyll adalah install [ruby](https://en.wikipedia.org/wiki/Ruby_%28programming_language%29) 

```ruby
:~$ sudo apt-get install ruby2.3 ruby2.3-dev
:~$ ruby2.3 -v
:~$ sudo apt-get install ruby-dev
```
setelah ruby terinstall dengan sempurna barulah kita bisa install jekyll

```ruby
:~$ sudo gem install jekyll
```
okeylah kalau sudah terinstall happy blogging

## Install Android studio 2
 [Android studio](http://developer.android.com/tools/studio/index.html) jugak merupakan peralatan yang paling sangat saya butuhkan penunjang kegiatan bekarya langkah awalnya adalah [mendownload](http://developer.android.com/sdk/index.html) android studionya terlebih dahulu trus exstract ke folder `/opt/` lalu panggil lewat konsole
 
```ruby
 :~$ cd /opt/android-studio/bin
 bin$ ./studio.h
```

akhirnya lengkap sudah ritualnya dan laptop siap untuk digunakan aktifitas sehari hari 