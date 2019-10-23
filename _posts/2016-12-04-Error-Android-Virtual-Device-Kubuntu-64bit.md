---
title: "Error Android Virtual Device kubuntu 64 bit"
tags:
 Linux
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---

{% capture avd_img %}
![Foo]({{"/assets/images/kinfo.png" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>kinfo</figcaption>
</figure>

Tibalah saatnya untuk upgrade Ram ddr3l thinkpad e145 dari 4GB ke 8GB di iringi penuh drama semenjak pertama kali upgrade ke 4GB dan 8GB adalah upgrade terakhir karena thinkpad ini hanya bisa up to 8GB menurut website resminya tapi menurut kabar beredar dari mas mas di VGEN service center ini thinkpad bisa sampek 16GB tapi rasanya saya tidak sebegitu tertarik untuk coba coba pasalnya upgrade ke 8GB hanya untuk memenuhi rekomendasi system untuk [android studio di operating system linux](https://developer.android.com/studio/index.html)
{: style="text-align: justify;"}

{% capture avd_img %}
![Foo]({{"/assets/images/rekomAndro.png" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>system requirement</figcaption>
</figure>

setelah upgrade dan langsung saya coba untuk menjalankan android studio dan run code untuk dijalankan di android virtual device untuk mengetahui bagaimana performa thinkpad saat setelah upgrade ram 8GB akhirnya ketemu error `EmptyThrowable: Failed to create the SD card`.
{: style="text-align: justify;"}

{% capture avd_img %}
![Foo]({{"/assets/images/empty.png" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

saya baru inget kalau kubuntu ini baru saya install ulang gara gara percobaan install guitarix beta akhirnya banyak error dedepency yang mengakibatkan harus install ulang (ini cara terjitu hemat waktu agar gak ribet aja heuheuheu) dan jawaban dari error di atas adalah [di postingan saya bulan bulan lalu](http://www.glamvian.com/erorr-android-studio/)
okey problem `EmptyThrowable: Failed to create the SD card` terselesaikan.
{: style="text-align: justify;"}

{% capture avd_img %}
![Foo]({{"/assets/images/erorRunEmulator.jpg" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

saat run emulator muncul error baru libGL error setelah baca baca di bug reportnya google jawabanya adalah libstdc++6 yang barusan di install belom terload di sdk kita
{: style="text-align: justify;"}

```ruby
 ~:$ cd ~/Android/Sdk/tools/lib64/libstdc++
 ~:$ mv libstdc++.so.6 libstdc++.so.6.bak
 ~:$ ln -s /usr/lib64/libstdc++.so.6 ~/Android/Sdk/tools/lib64/libstdc++
```

{% capture avd_img %}
![Foo]({{"/assets/images/solved.jpg" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

akhirnya emulator bisa mengudara dan setelah compile dan run code app terinstall sempurna di emulator dan berjalan sangat smooth pengaruh ram 8GB alhamdulilah jadi tambah semangat bekarya ya
{: style="text-align: justify;"}

{% capture avd_img %}
![Foo]({{"/assets/images/emudone.gif" | relative_url}})
{% endcapture %}
<figure>
    {{avd_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>