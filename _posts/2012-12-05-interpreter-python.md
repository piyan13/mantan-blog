---
title: "Interpreter Python"
tags:
 Pemrogaman
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---

{% capture tasbih_img %}
![Foo]({{"/assets/images/pytthon.png" | relative_url}})
{% endcapture %}
<figure>
    {{tasbih_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>credit: google.com</figcaption>
</figure>
lagi pingin nerusin bikin desain logo button app lakok malas datang iseng buka koleksi buku  eh tiba tiba menyelinap buku pemrogaman python.Menurutku ini bahasa pemrogaman yang tak kalah fenomena dari bahasa pemrogaman java kenapa? bayangkan hanya beberapa tahun saja bahasa pemrogaman python sudah masuk ranking 5besar bahasa pemrogaman yang sering dipakai para progammers heuheuehue tak heran sih karena orientasinya yang OPEN SOURCE membuat komunitasnya tumbuh subur. Diantara kelebihan bahasa pemrogaman obyek dinamis ini adalah :

* Dapat berjalan di banyak platform/sistem operati serperti linux/unix,Macos,palm dll
* Cepat dan powerfull karena terdiri dari modul modul
* Dapat berintegrasi dengan Component Object Model(COM),.NET dan Common Object Request Broker Architecture (CORBA)
* Dokumentasi yg lengkap dari dukungan komunitas.
* Tentunya OPEN SOURCE heuheuheu bahkan utk kepentingnan komersil

yang paling meneyenangkan bagi pengguna operating system Linux seperti saya adalah python dilengkapi fasilitas interpreter seperti shell yang tertanam secara default di `/usr/bin/python`. ini memungkinkan  user Linux mencoba python secara interaktif. Mari langsung saja untuk mencobanya tekan ctrl+alt+t (buka console)

```console
 glamvian@glamvian ~ $ python
```

maka akan muncul:

```python
Python 2.7.2+ (default, Oct  4 2011, 20:03:08) 
[GCC 4.6.1] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
tanda >>> adalah perintah prompt utama python .
sekarang mari mencoba nya

```python
>>> p=10  
>>>p     
10  
>>> l=15  
>>>l  
15  
>>> n=p*l  
>>> print 'luas: ',n en  
luas :  150  
```
keren gak? coba lagi ageh

```python
>>> a = 1  
> > > if a == 1:  
... print "Nilai a adalah 1"  
...  
Nilai a adalah 1  
> > > def fac(n):  
... if n < 2:  
... return 1  
... else:  
... return n * fac(n-1)  
...  
> > > fac(8)  
40320  
> > > fac(10)  
3628800  
>>>  
```

tanda ... adalah secondary prompt untuk keluar bisa dengan cara ctrl+D.
Ini contoh untuk cara interaktif aja dan kelemahannya adalah kita tidak bisa menyimpan di media penyimpanan karena mode interaktif ini hanya untuk mencoba beberapa script yang akan kita pakai. kalau mau ngoding benerannya ya harus pake Text Editor misal kalau di linux pake gedit.
saya kasih contoh tulis ini di gedit atau texteditor yang anda suka
```python
a=10
b=15
c=a*b

print 'hasil dari axb: ',c
```
save degan belajar.py nah panggil deh lewat console(ctrl+alt+t) dengan perintah

`glamvian@glamvian ~ $ python belajar.py`
liat hasilnya, selamat mengeksplorisasi bahasa pemrogaman yang fenomena ini dan akan sangat menjanjikan di masa yang akan datang.

