---
title: "dpkg: unrecoverable fatal error"
tags:
 Linux Pemrogaman
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---

{% capture linux_img %}
![Foo]({{"/assets/images/hello_world_01.jpg" | relative_url}})
{% endcapture %}
<figure>
    {{linux_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

hello 2015 heuheuheu waktunya turun gunung beroooo vakum coret blog dari 2012 dimulai dari bercandaan tuhan dengan syntak :

{% capture linux_img %}
![Foo]({{"/assets/images/eroer.png" | relative_url}})
{% endcapture %}
<figure>
    {{linux_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

dpkg:unrecoverable fatal error dan seterusnya kedengerannya serem gak?
setelah cari sana sini ternyata itu bug yang sudah ada sejak beberapa varian
turunan keluarga besar debian. 
itu terjadi karena ada corupt file di `/var/lib/dpkg/info/` berulang ulang karena beberapa tidak memiliki final new line.
{: style="text-align: justify;"}
```python
import os  
  
dpkg_path = '/var/lib/dpkg/info/'  
paths = os.listdir(dpkg_path)  
for path in paths:  
    path = dpkg_path + path  
    f = open(path, 'a+')  
    data = f.read()  
    if len(data) > 1 and data[-1:] != '\n':  
        f.write('\n')  
        print 'added newline character to:', path  
    f.close()     
```

baris code python diatas adalah jawabannya berooo copy paste code diatas ke 
knote,nano,vi dll trus save dengan apapun misal kalau aku dengan `mbuh.py`
jangan lupa waktu nge run pake sebagai root misal :~> sudo python mbuh.py


{% capture linux_img %}
![Foo]({{"/assets/images/eroer1.png" | relative_url}})
{% endcapture %}
<figure>
    {{linux_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

beres sudah masalah beroo ayoo lekas update dan upgrade atau install sesuatu yang tadi eroor kayak aku yg gagal install git sekarang sudah lancar..... 
thanks to pokerbirch for python scripth.. hapy coding everyone
