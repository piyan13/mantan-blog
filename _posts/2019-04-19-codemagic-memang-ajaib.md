---
title: "CodeMagic memang ajaib"
tags:
 Android
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
<figure style="width: 450px" class="align-center">
<img src="/images/Codemagic.jpg">
<figcaption>credit: nevercode</figcaption>
</figure> 
{% include toc title="Contents" icon="file-text" %}

Akhirnya menulis lagi! kali ini aku ingin menulis tentang CI/CD (continous integration dan develivery) risetnya demi kebutuhan di kerjaan, tujuannya adalah efisiensi waktu development product product forca mobile , kebetulan di team RnD forcaErp pt sisi (sinergi informatika semen indonesia) mobile developer belom menerapkan tools CI/CD berbeda dengan ForcaErp devnya yang sudah memakai jenkins. monggo check [erpForca](http://forca.id/) , riset kali ini menggunakan tools [CodeMagic](https://codemagic.io/) tools yang benar benar fokus CI/CD untuk [Flutter](https://flutter.dev/).
Tl;dr dibawah ini adalah setup di workflow default dari codemagic dengan tujuan akhir delivery google play console android dan app store untuk ios.
{:style="text-align: justify;"}


## Setting Repository Project
setelah login dengan git version control dimana codemagic mengakomodir github, gitlab dan bitbucket secara otomatis semua repository kita di load dihalaman codemagic.
{:style="text-align: justify;"}
<figure style="width: 450px" class="align-center">
<img src="/images/home.png">
<figcaption></figcaption>
</figure>
pilih setting project yang akan kita setup untuk CI/CD di codemagic.

## Setup Triggers Branch
build triggers disini adalah branch repository project yang di jadikan pemicu untuk codemagic melakukan build normalnya adalah branch master tapi tergantung kita jugak.
{:style="text-align: justify;"}
<figure style="width: 450px" class="align-center">
<img src="/images/build_triggers.png">
<figcaption>build triggers</figcaption>
</figure>
disana ada pilihan pattern yang bisa di gunakan misal * menunjuk ke semua branch menjadi trigger untuk keterangan lengkapnya bisa klik show pattern examples jugak ada opsi dibawahnya untuk automatic triggering buildnya otomatis setiap waktu ada push ke branch atau setiap ada pull request ke branch.
{:style="text-align: justify;"}
<figure style="width: 450px" class="align-center">
<img src="/images/build_master.png">
<figcaption>show pattern</figcaption>
</figure>

<figure style="width: 450px" class="align-center">
<img src="/images/buil_everypush.png">
<figcaption></figcaption>
</figure>

## Test 
test disini adalah mengarah ke package test di project flutter karena di project saya tidak memakai unit test jadi disini tidak saya centang untuk enable flutter test dan stop build if test fail yang berarti flow test ini saya lewati
<figure style="width: 450px" class="align-center">
<img src="/images/test.png">
<figcaption></figcaption>
</figure>

## Build
di flow build ini kita bisa memilih bakal di build versi flutter berapa repository kita :
<figure style="width: 450px" class="align-center">
<img src="/images/flowbuild_version.png">
<figcaption>pilihan build flutter version</figcaption>
</figure>
sesuai goal dari riset saya yang mau jadikan dua mobile platform sekaligus yaitu ios dan android maka dibawah ini adalah setup flow build saya
<figure style="width: 450px" class="align-center">
<img src="/images/build_an_ios.gif">
<figcaption></figcaption>
</figure>

## Publish
<figure style="width: 450px" class="align-center">
<img src="/images/flowPublish.png">
<figcaption>tampilan flow publish</figcaption>
</figure>
di dalam flow publish ini  ada beberapa setting yang harus di setup saran saya adalah pastikan dulu apakah CI bekerja dengan lancar setelah itu baru setting untuk CD, setting apa saja yang untuk CI ?:

### settingan flow publish untuk CI

1.android code signing untuk menghasilkan apk.release apa saja yang diperlukan kunjungi [link ini](https://developer.android.com/studio/publish/app-signing?hl=id)
{:style="text-align: justify;"}
<figure style="width: 450px" class="align-center">
<img src="/images/android_code_signing.png">
<figcaption></figcaption>
</figure>
2.ios code signing kalau ingin menghasilkan file .ipa disana ada pilihan automatic dan manual, saya sarankan untuk manual karena automatic percobaan saya beberapa kali masih menemui error. di manual cukup isi dengan code signing certificate dalam format .p12 dan isi provsioning profiles (disarankan yang distribution) dengan format .mobileprovision, kalau belom tahu cara untuk mendapatkan certificate .p12 bisa kunjungi [link ini](https://support.magplus.com/hc/en-us/articles/203808748-iOS-Creating-a-Distribution-Certificate-and-p12-File), jika ios code signing ini tidak di isi maka artifactnya dalam bentum runner.
{:style="text-align: justify;"}
<figure style="width: 450px" class="align-center">
<img src="/images/ios_signing.png">
<figcaption></figcaption>
</figure>
3.setup email yang akan dijadikan archive artifact dan jugak untuk notifikasi baik ketika build failed ataupun sukses, ada opsi jugak untuk slack selain email
<figure style="width: 450px" class="align-center">
<img src="/images/flowPublish_email.png">
<figcaption></figcaption>
</figure>

### Test CI (Continous Integration) 
<figure style="width: 450px" class="align-center">
<img src="/images/startNewBuild.png">
<figcaption>start new build</figcaption>
</figure>

pilih start new build pojok kanan atas

<figure style="width: 450px" class="align-center">
<img src="/images/testCi_succes.gif">
<figcaption>tampilan ketika berhasil test CI</figcaption>
</figure>

jika berhasil check email/slack yang sudah di setup seperti dibawah ini:

<figure style="width: 450px" class="align-center">
<img src="/images/email_ios_android.gif">
<figcaption>tampilan email</figcaption>
</figure>
setelah memastikan CI berjalan dengan semestinya selanjutnya adalah setup CD

### settingan flow publish untuk CD
ketika settingan CI kita sudah tidak menemui kendala berikutnya tinggal setup flow publish google play untuk android dan apple store connect untuk ios yang mana tujuannya adalah automate delivery ketika build sukses.

#### google play
yang dibutuhkan untuk menyinkronkan codemagic ke google play adalah credential api google play file format dalam bentuk .json bagaimana cara untuk mendapatkan file ini bisa di sima [artikel ini](https://support.appinstitute.com/hc/en-us/articles/115002827469-How-to-Get-Your-Google-Play-JSON-Key),
setelah itu centang publish even if test fail.
<figure style="width: 450px" class="align-center">
<img src="/images/googleplay.png">
<figcaption></figcaption>
</figure>

#### apple store connect
isi dengan apple id yang sudah terdaftar di apple development untuk password saya sarankan memakai application-spesific password bagaimana cara membuatnya? kunjungi appleid.apple.com --> security --> authentiiasi two factor . sertakan app id lalu uncentang publish even if test fail
<figure style="width: 450px" class="align-center">
<img src="/images/iosstore_connect.png">
<figcaption></figcaption>
</figure>

## Test CI/CD
bisa kembali ke ide repository local untuk push ke git atau tombol start new build di pojok kanan atas di codemagic

<figure style="width: 450px" class="align-center">
<img src="/images/testCD_prosess.gif">
<figcaption>proses build </figcaption>
</figure>

kalau sudah sukses akan seperti dibawah ini

<figure style="width: 450px" class="align-center">
<img src="/images/succes_CD.gif">
<figcaption></figcaption>
</figure>

untuk memastikan bisa di check di google play console dan ios store

<figure style="width: 450px" class="align-center">
<img src="/images/succes_andro_delivery.gif">
<figcaption>tampilan di google play</figcaption>
</figure>

<figure style="width: 450px" class="align-center">
<img src="/images/succes_ios_delivery.gif">
<figcaption>tampilan di apple store</figcaption>
</figure>

seperti itulah setup codemagic yang telah saya riset masih banyak keluwesan yang ada di codemagic yang belom aku explore, mungkin tulisan berikutnya akan lebih dalem lagi semoga tentang codemagic, disini saya belom sempet membandingkan dengan tool sejenis apakah perfomance, security dll. mungkin di kemudian hari bakal bisa kasih testi kalau sudah cobain tool selain codemagic, okey terimakasih semoga bermanfaat.

referensi :
- [continous integration vs delivery vs deployment](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)
- [what is cicd concept in continous integration and deployment](https://medium.com/@nirespire/what-is-cicd-concepts-in-continuous-integration-and-deployment-4fe3f6625007)
- [Flutter: CI CD With Codemagic Automate Your Workflow](https://www.youtube.com/watch?v=GsQ5MHHVNqQ)

