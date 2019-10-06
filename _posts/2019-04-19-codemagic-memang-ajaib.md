---
title: "CodeMagic memang ajaib"
tags:
 - Android
 - Flutter
 - CodeMagic
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
![codemagic image]({{site.url}}{{site.baseurl}}/assets/images/Codemagic.jpg)

{% include toc title="Contents" icon="file-text" %}

Akhirnya menulis lagi! kali ini aku ingin menulis tentang CI/CD (continous integration dan develivery) risetnya demi kebutuhan di kerjaan, tujuannya adalah efisiensi waktu development product product forca mobile , kebetulan di team RnD forcaErp pt sisi (sinergi informatika semen indonesia) mobile developer belom menerapkan tools CI/CD berbeda dengan ForcaErp devnya yang sudah memakai jenkins. monggo check [erpForca](http://forca.id/) , riset kali ini menggunakan tools [CodeMagic](https://codemagic.io/) tools yang benar benar fokus CI/CD untuk [Flutter](https://flutter.dev/).
Tl;dr dibawah ini adalah setup di workflow default dari codemagic dengan tujuan akhir delivery google play console android dan app store untuk ios.
{:style="text-align: justify;"}


## Setting Repository Project
setelah login dengan git version control dimana codemagic mengakomodir github, gitlab dan bitbucket secara otomatis semua repository kita di load dihalaman codemagic.
{:style="text-align: justify;"}
![codemagic home]({{site.url}}{{site.baseurl}}/assets/images/home.png){: .full}


pilih setting project yang akan kita setup untuk CI/CD di codemagic.

## Setup Triggers Branch
build triggers disini adalah branch repository project yang di jadikan pemicu untuk codemagic melakukan build normalnya adalah branch master tapi tergantung kita jugak.
{:style="text-align: justify;"}
![codemagic build trigger]({{site.url}}{{site.baseurl}}/assets/images/build_triggers.png){: .full}

disana ada pilihan pattern yang bisa di gunakan misal * menunjuk ke semua branch menjadi trigger untuk keterangan lengkapnya bisa klik show pattern examples jugak ada opsi dibawahnya untuk automatic triggering buildnya otomatis setiap waktu ada push ke branch atau setiap ada pull request ke branch.
{:style="text-align: justify;"}
{% capture code_img %}
![Foo]({{"/assets/images/build_master.png" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>show pattern</figcaption>
</figure>

![codemagic build every push]({{site.url}}{{site.baseurl}}/assets/images/buil_everypush.png){: .full}

## Test 
test disini adalah mengarah ke package test di project flutter karena di project saya tidak memakai unit test jadi disini tidak saya centang untuk enable flutter test dan stop build if test fail yang berarti flow test ini saya lewati
![codemagic test]({{site.url}}{{site.baseurl}}/assets/images/test.png){: .full}

## Build
di flow build ini kita bisa memilih bakal di build versi flutter berapa repository kita :
{:style="text-align: justify;"}
{% capture code_img %}
![Foo]({{"/assets/images/flowbuild_version.png" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>pilihan build flutter version</figcaption>
</figure>
sesuai goal dari riset saya yang mau jadikan dua mobile platform sekaligus yaitu ios dan android maka dibawah ini adalah setup flow build saya

![codemagic build ios]({{site.url}}{{site.baseurl}}/assets/images/build_an_ios.gif){: .full}

## Publish
{% capture code_img %}
![Foo]({{"/assets/images/flowPublish.png" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan flow publish</figcaption>
</figure>
di dalam flow publish ini  ada beberapa setting yang harus di setup saran saya adalah pastikan dulu apakah CI bekerja dengan lancar setelah itu baru setting untuk CD, setting apa saja yang untuk CI ?:

### settingan flow publish untuk CI

1.android code signing untuk menghasilkan apk.release apa saja yang diperlukan kunjungi [link ini](https://developer.android.com/studio/publish/app-signing?hl=id)
{:style="text-align: justify;"}

![codemagic setting flow]({{site.url}}{{site.baseurl}}/assets/images/android_code_signing.png){: .full}

2.ios code signing kalau ingin menghasilkan file .ipa disana ada pilihan automatic dan manual, saya sarankan untuk manual karena automatic percobaan saya beberapa kali masih menemui error. di manual cukup isi dengan code signing certificate dalam format .p12 dan isi provsioning profiles (disarankan yang distribution) dengan format .mobileprovision, kalau belom tahu cara untuk mendapatkan certificate .p12 bisa kunjungi [link ini](https://support.magplus.com/hc/en-us/articles/203808748-iOS-Creating-a-Distribution-Certificate-and-p12-File), jika ios code signing ini tidak di isi maka artifactnya dalam bentum runner.
{:style="text-align: justify;"}

![codemagic ios signing]({{site.url}}{{site.baseurl}}/assets/images/ios_signing.png){: .full}

3.setup email yang akan dijadikan archive artifact dan jugak untuk notifikasi baik ketika build failed ataupun sukses, ada opsi jugak untuk slack selain email

![codemagic email]({{site.url}}{{site.baseurl}}/assets/images/flowPublish_email.png){: .full}

### Test CI (Continous Integration) 
{% capture code_img %}
![Foo]({{"/assets/images/startNewBuild.png" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>start new build</figcaption>
</figure>

pilih start new build pojok kanan atas
{% capture code_img %}
![Foo]({{"/assets/images/testCi_succes.gif" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan ketika berhasil test CI</figcaption>
</figure>

jika berhasil check email/slack yang sudah di setup seperti dibawah ini:
{% capture code_img %}
![Foo]({{"/assets/images/email_ios_android.gif" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan email</figcaption>
</figure>
setelah memastikan CI berjalan dengan semestinya selanjutnya adalah setup CD

### settingan flow publish untuk CD
ketika settingan CI kita sudah tidak menemui kendala berikutnya tinggal setup flow publish google play untuk android dan apple store connect untuk ios yang mana tujuannya adalah automate delivery ketika build sukses.

#### google play
yang dibutuhkan untuk menyinkronkan codemagic ke google play adalah credential api google play file format dalam bentuk .json bagaimana cara untuk mendapatkan file ini bisa di sima [artikel ini](https://support.appinstitute.com/hc/en-us/articles/115002827469-How-to-Get-Your-Google-Play-JSON-Key),
setelah itu centang publish even if test fail.

![codemagic googleplay]({{site.url}}{{site.baseurl}}/assets/images/googleplay.png){: .full}

#### apple store connect
isi dengan apple id yang sudah terdaftar di apple development untuk password saya sarankan memakai application-spesific password bagaimana cara membuatnya? kunjungi appleid.apple.com --> security --> authentiiasi two factor . sertakan app id lalu uncentang publish even if test fail

![codemagic iostore]({{site.url}}{{site.baseurl}}/assets/images/iosstore_connect.png){: .full}

## Test CI/CD
bisa kembali ke ide repository local untuk push ke git atau tombol start new build di pojok kanan atas di codemagic
{% capture code_img %}
![Foo]({{"/assets/images/testCD_prosess.gif" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>proses build</figcaption>
</figure>

kalau sudah sukses akan seperti dibawah ini

![codemagic build succes]({{site.url}}{{site.baseurl}}/assets/images/succes_CD.gif){: .full}

untuk memastikan bisa di check di google play console dan ios store
{% capture code_img %}
![Foo]({{"/assets/images/succes_andro_delivery.gif" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan di google play</figcaption>
</figure>

{% capture code_img %}
![Foo]({{"/assets/images/succes_ios_delivery.gif" | relative_url}})
{% endcapture%}
<figure>
	{{code_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>tampilan di apple store</figcaption>
</figure>

seperti itulah setup codemagic yang telah saya riset masih banyak keluwesan yang ada di codemagic yang belom aku explore, mungkin tulisan berikutnya akan lebih dalem lagi semoga tentang codemagic, disini saya belom sempet membandingkan dengan tool sejenis apakah perfomance, security dll. mungkin di kemudian hari bakal bisa kasih testi kalau sudah cobain tool selain codemagic, okey terimakasih semoga bermanfaat.

referensi :
- [continous integration vs delivery vs deployment](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)
- [what is cicd concept in continous integration and deployment](https://medium.com/@nirespire/what-is-cicd-concepts-in-continuous-integration-and-deployment-4fe3f6625007)
- [Flutter: CI CD With Codemagic Automate Your Workflow](https://www.youtube.com/watch?v=GsQ5MHHVNqQ)

