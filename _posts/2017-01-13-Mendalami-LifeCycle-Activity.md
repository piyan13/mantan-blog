---
title: "Mendalami Activity Lifecyle"
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
Mengulang membaca buku materi tentang Lifecyle Activity semakin diulang semaki seru tentu saja merawat ingatan biar fondasi android tak terlupakan kali ini dengan App stopwatch simpel hanya dengan single  class Activity dan single layout yaitu `stopWatchActivity` dan `activity_stop_watch.xml`.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/stopwatch.gif" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>penampakan app</figcaption>
</figure>

Sama seperti stopwatch pada umumnya fungsinya untuk menghitung waktu perdetik ada 3 button disana start, stop, dan reset dengan uml seperti dibawah ini.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/StopWatchActiviy.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>uml class StopWatchActivity</figcaption>
</figure>

issue pertama muncul ketika app berjalan setelah kita touch start button trus device kita ganti rotasi jadi horizontal ditengah tengah app berjalan seketika aplikasi langsung muncul seperti awal lagi alias di destroy.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/rotationStp.gif" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>rotation</figcaption>
</figure>

apa yang terjadi adalah rotasi layar android dan ukuran layar berubah, dan itu destroy Activity, termasuk semua variabel yang digunakan method runTimer(). setelah itu method onCreate() berjalan lagi dan method runTimer() dipanggil karena Activity di re-create, variabel mSecond dan mRunning bernilai default.
{: style="text-align: justify;"}

## Launch, Running , Destroy
Main state dari activity adalah ketika dia running atau aktif. activity berjalan di foreground dari screen dan user berinteraksi dengannya, activity running setelah ter launch dan berakhir setelah di destroy. ketika activity berubah dari launched dan akan di destroy dia mentrigger method kunci Lifecyle activity yaitu onCreate() dan onDestroy(). itu adalah method Lifecyle yang kita inherit di Activity dan bisa kita override.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/activitylaunchstp.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>alur activity born to death</figcaption>
</figure>

Method onCreate() akan dipanggil setelah Activity di launch. dan method onDestroy() adalah method terakhir yang di panggil sebelum activity di destroy.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/inheritstp.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>turunan class activity</figcaption>
</figure>

karena issue rotasi terjadi karena destroy maka sebelum destroy terjadi saya mau simpan state yang sedang berjalan dengan method onSaveInstanceState() method ini bisa dipanggil sebelum method onDestroy() berjalan. method onSaveInstanceState() mempunyai satu parameter yaitu Bundle dan Bundle
membolehkan kita untuk mengumpulkan data bersama walaupun beda type data kedalam single object.
{: style="text-align: justify;"}

### onSaveInstanceState()
method onCreate akan melewati Bundle sebagai parameter. ini berarti jika saya menambah value mRunning dan mSecond ke Bundle, method onCreate() akan mengambil mereka ketika Activity dimuat ulang. aturan memakai method Bundle :
{: style="text-align: justify;"}

`bundle.put*("name", value)`

dimana `bundle` adalan nama Bundle, `*` untuk type value yang ingin disimpan, seperti ini jadinya
{: style="text-align: justify;"}


{% capture lifecycle_img %}
![Foo]({{"/assets/images/bundleput.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

akhirnya saya simpan variabel values di Bundle dan dapat menggunakan mereka berdua method di onCreate(). setelah disimpan ketika nantinya activity di destroy dan di muat ulang onCreate() jugak yang telah kita tahu memliki parameter Bundle jugak tapi dalam posisi kali ini parameter menjadi bernilai null namun bisa dibuat pengecualian jika dan simpanan di Bundle kita bisa get value yang sudah disimpan ke Activity seperti dengan method `bundle.get*("name");` 
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/bundleget.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>bundle. get</figcaption>
</figure>

alhamdulillah pecah misterinya 

{% capture lifecycle_img %}
![Foo]({{"/assets/images/rotatestop.gif" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>isuue rotate solve</figcaption>
</figure>

uml setelah penambahan method onSaveInstanceState()
{% capture lifecycle_img %}
![Foo]({{"/assets/images/stopwatchactivity2.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>isuue rotate solve</figcaption>
</figure>

## Start, Stop, Restart
issue selanjutnya adalah ketika app berjalan user klik start button tiba tiba ada telpon atau notifikasi lain misal aplikasi chat terus kita liat/buka app chat itu aplikasi stopwatch otomatis tidak terlihat dan mungkin stopwatch akan terus berjalan menghitung tiap detik tapi bagaimana kalau aku pengen ketika app stopwatch tidak terlihat dia otomatis stop alias tidak berjalan? ternyata di Lifecyle ada method onStart(), onStop(),  onRestart() ketiganya sama seperti onCreate() dan onDestroy() sama sama mewarisi dari Activity clas.
{: style="text-align: justify;"}

* onStart() akan dipanggil bila acitivity kita menjadi visible ke user.
* onStop() dipanggil ketika activity kita berhenti nampak ke user,ini berarti Activity akan di destroy tapi sebelum method onDestroy() dipanggil method onSaveInstanceState() akan dipanggil sebelum method onStop().
* onRestart() dipanggil setelah activity kita tak terliat atau invisible.


{% capture lifecycle_img %}
![Foo]({{"/assets/images/lifecycleVisible.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>visible lifetime</figcaption>
</figure>

okey saatnya override method onStop di coding kita setelah itu buatnya berhenti dengan `mRunning = false`
{: style="text-align: justify;"}

```java
@override
proctected void onStop() {
 super.onStop();
 mRunning = false;
 }
 ```
sekarang stopwatch berhenti ketika acitivity tidak lagi terlihat . lalu yang saya perlu lakukan adalah meminta stopwatch start lagi ketika activity kembali terlihat. pertama tama saya menambah variabel boolean mWasRunning untuk di digunakan method onSaveInstanceState(), dan onRestart(). 
{: style="text-align: justify;"}
```java
    private int mSecond = 0;
    private boolean mRunning;
    private boolean mWasRunning;
```
lalu menambahkannya di method onSaveInstanceState()

```java
 @Override
    public void onSaveInstanceState(Bundle outState) {
        outState.putInt("second",mSecond);
        outState.putBoolean("running",mRunning);
        outState.putBoolean("wasRunning",mWasRunning);
        super.onSaveInstanceState(outState);
    }
```
waktunya override method onStop()

```java
@Override
    protected void onStop() {
        super.onStop();
        mWasRunning = mRunning;
        mRunning = false;
    }
```

```java
 @Override
    protected void onStart() {
        super.onStart();
        if (mWasRunning){
            mRunning = true;
        }
    }
```


{% capture lifecycle_img %}
![Foo]({{"/assets/images/visiblestp.gif" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>stop visible</figcaption>
</figure>

skenarionya adalah pertama app stopwatch berjalan variabel mRunning = true ; di pertengahan saya pindah ke app todo task menjadikan app stopwatch invisible dalam posisi ini variabel mRunning menjadi false karena method onStop() dan variabel mWasRunning menjadi berniali true. selanjutnya saya kembalikan ke app stopwatch method onStart menerima variabel mWasRunning dan activity mengambil simpana data dari method onSaveInstanceState() akhinya kembali berjalan.
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/stpwasrun.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>uml setelah mWasRunning</figcaption>
</figure>

### kenapa onRestart() bukan onRestart()?
ada pertanyaan kenapa tidak memaki method onRestart()? method itu digunakan bila kita hanya ingin code berjalan ketika app dari visible yang mana sebelumnya invisible. dia tidak berjalan ketika activity visible untuk pertamakalinya. dalam kasus stopwatch ini kan saya ingin app tetap berjalan ketika app di rotasi jugak. trus perbedaane apa onRestart dan onStart? okey saat kita merubah rotasi device acitivity ter destroy dan membuat satu lagi yang baru. jika kita masukan code di method onRestart, dia tidak akan berjalan ketika activity re-created. kalau method onStart() terpanggil saat terjadi rotasi dan terjadi invisible ke visible. 
{: style="text-align: justify;"}
## onPaused()
setelah mempelajari alur activity di buat dan di destroy lalu telah melihat jugak ketika activity saat visible menjadi invisible ada beberapa method yang bisa di warisi dari superclass yang bisa dimanfaatkan tapi satu lagi yang saya harus pelajari yaitu masalah activity posisi visible tapi tidak fokus ini berarti activity menjadi paused. ini akan terjadi jika app stopwatch berjalan ada activity app lain berada ditas activity app stopwath tapi tidak keluar full screen tampilannya
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/onpauestp.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>posisi onpaused</figcaption>
</figure>

ada 2method Lifecyle activity yang bisa dimanfaatkan terkait issue ini yaitu method onPaused() dan onResume(). onPause() dipanggil ketika activity kita visible tapi ada activity lain fokus. onResume() dipanggil ketika activity kita belum berinteraksi dengan user
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/onpausedstpw.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>alur Lifecyle</figcaption>
</figure>

penjelasan dari gambar:

*1.* Activity di launch dan method onCreate() dan onStart() berjalan.lalu ketika Activity visible tidak focus

*2.* onResume () berjalan setelah onStart(). dia dipanggil ketika activity untuk pindah ke foreground setelah onResume jalan activity visible dan fokus lagi bisa interaksi dengan user

*3.* onPause() berjalan ketika acitivity berhenti menjadi foreground. setelah onPause() berjalan activity tetap visible tapi tidak fokus

*4.* jika activity berpindah menjadi foreground lagi , onResume() akan dipanggil lagi.

*5.* jika activity berhenti visible terhadap user, method onStop() akan dipanggil. setelah onStop() berjalan activity tidak lagi visible

*6.* jika activity menjadi visible ke user lagi, onRestart() dipanggil, diikuti onStart() dan onResume().

*7.* akhinya activity di destroy
## apa yg terjadi ketika onPaused device diubah rotasinya?
muncul pertanyaan selanjutnya bagaimana saat activity paused karena tidak fokus terus device kita ubah rotasinya? aslinya Lifecyle berjalan melalui  semua method dari onCreate() ke onDestroy(). activity baru dibuat ketika yang asli terdestroy karena activity baru tidak berada di foreground hanya method lifecylce onCreate() dan onStart() yang dapt dipanggil. 

jika kita memliki sebuah activity visible tapi tidak di foreground dan tidak akan punya fokus maka method onPause() dan onResume() tidak akan pernah dipanggil maka activity akan langsung pergi dari onStart() ke onStop()
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/oncreatestp.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

setelah memerhati dengan seksama ternyata kita bisa mereplace method onStop() dan onStart() dengan method onPause() dan onResume() heuheuheu namanya jugak mendalami materi jadine ada hal hal baru 
{: style="text-align: justify;"}

{% capture lifecycle_img %}
![Foo]({{"/assets/images/onresumestp.png" | relative_url}})
{% endcapture %}
<figure>
    {{lifecycle_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>uml akhir</figcaption>
</figure>
Beres sudah mendalami samudra LifeCycle Activity
