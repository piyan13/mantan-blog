---
title: "Install ReactNative dengan banyak ERORR"
tags:
 Android
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---
{% capture rn_img %}
![Foo]({{"/assets/images/react6.png" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>credit: codeburst</figcaption>
</figure>

{% include toc title="Contents" icon="file-text" %}

Bulan puasa tahun ini di awali dengan suka cita diberi kesempatan kembali untuk berkreasi di dunia progamming baru beberapa hari dan terkesan sangat cepat updatenya mulai takjub dengan Contraints layout, project kompleks dengan usia 2tahun dan si boss yang lagi seru serunya dengan ReactNative beberapa minggu sebelumnya tanpa sengaja saya install flutter walaupun belum sempet cobak cobak kecepatan build antara ReactNative dan fluuter kalau diliat dari video video yutub sama sama cepatnya kali ini karena di sela sela beradaptasi dengan pekerjaan baru yang gak baru baru banget sudah idola sejak masih duduk di bangku perkuliahan saya install ReactNative di mesin linux andalan saya terkejutnya banyak menemui ERORR, apa saja erorrnya mari kita uraikan sebelumnya saya telah install android studio tentunya java dan nodeJs( lupa install ini buat apa sebelumnya ) untuk kali jelas buat mesin utama ReactNative, saya secara langsung mengikuti panduan langkah demi langkah install ReactNative dari [blog ini](https://www.moveoapps.com/blog/how-to-install-react-native-on-ubuntu-17-10/) mulailah menemui error ketika install watchman dimana watchman diperlukan untuk melihat file system
{:style="text-align: justify;"}

## install wathcman
peringatan awal ketika melalui langkah autogen.sh
```
 ~/watchman $ ./autogen.sh
 your system lacks libtoolize

```
### install libtool
yang berarti harus install dulu `libtool`
```
$ sudo apt-get install libtool

```
{% capture rn_img %}
![Foo]({{"/assets/images/react1.png" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>./autogen.sh</figcaption>
</figure>

kiraen output begitu pertanda semua ok maka saya teruskan dengan langkah selanjutnya yaitu `$./configure` karena dengan tidak adanya pesan peringatan yang berarti saya lanjutkan dengan `$ make` langsung keluar pesan error 
```
ContentHash.cpp:13:25: fatal error: openssl/sha.h: No such file or directory
compilation terminated.
Makefile:3143: recipe for target 'watchman-ContentHash.o' failed
make[1]: *** [watchman-ContentHash.o] Error 1
make[1]: Leaving directory '/home/glmvn/react_native/watchman'
Makefile:1101: recipe for target 'all' failed
make: *** [all] Error 2

```
### install libssl
itu yang berarti tidak ditemukannya library ssl langsung saja install `$ sudo apt-get install libssl-dev` dan dicoba `make` ulang dan muncul pesan error baru
```
watchman-ContentHash.o: In function `watchman::ContentHashCache::computeHashImmediate(watchman::ContentHashCacheKey const&) const':
/home/glmvn/react_native/watchman/ContentHash.cpp:65: undefined reference to `SHA1_Init'
/home/glmvn/react_native/watchman/ContentHash.cpp:78: undefined reference to `SHA1_Update'
/home/glmvn/react_native/watchman/ContentHash.cpp:81: undefined reference to `SHA1_Final'
collect2: error: ld returned 1 exit status
Makefile:1566: recipe for target 'watchman' failed
make[1]: *** [watchman] Error 1
make[1]: Leaving directory '/home/glmvn/react_native/watchman'
Makefile:1101: recipe for target 'all' failed
make: *** [all] Error 2

```
setelah mencari clue di github tempat repository watchman berada saya menemukan jawaban [setelah install libssl untuk melakukan ./autogen.sh dan ./configure ulang sebelum make](https://github.com/facebook/watchman/issues/514) setelah saya ikuti emang bener siih tapi muncul pesan erorr baru 
```
cd python && /usr/bin/python ./setup.py clean build_py -c -d . build_ext -i
/usr/lib/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'zip_safe'
  warnings.warn(msg)
/usr/lib/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'test_suite'
  warnings.warn(msg)
running clean
running build_py
byte-compiling ./pywatchman/compat.py to compat.pyc
byte-compiling ./pywatchman/capabilities.py to capabilities.pyc
byte-compiling ./pywatchman/__init__.py to __init__.pyc
byte-compiling ./pywatchman/encoding.py to encoding.pyc
byte-compiling ./pywatchman/load.py to load.pyc
byte-compiling ./pywatchman/pybser.py to pybser.pyc
running build_ext
building 'pywatchman.bser' extension
creating build
creating build/temp.linux-x86_64-2.7
creating build/temp.linux-x86_64-2.7/pywatchman
x86_64-linux-gnu-gcc -pthread -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fno-strict-aliasing -Wdate-time -D_FORTIFY_SOURCE=2 -g -fstack-protector-strong -Wformat -Werror=format-security -fPIC -I/usr/include/python2.7 -c pywatchman/bser.c -o build/temp.linux-x86_64-2.7/pywatchman/bser.o
pywatchman/bser.c:31:20: fatal error: Python.h: No such file or directory
compilation terminated.
error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
Makefile:5060: recipe for target 'py-build' failed
make[1]: *** [py-build] Error 1
make[1]: Leaving directory '/home/glmvn/react_native/watchman'
Makefile:1101: recipe for target 'all' failed
make: *** [all] Error 2
```
### install python-dev
ternyata package build pythonnya belom ada langsung saja saya install `$ sudo apt-get install python-dev` terus ulangi lagi ./autogen.sh , ./configure dan akhirnya 
```
cd python && /usr/bin/python ./setup.py clean build_py -c -d . build_ext -i
/usr/lib/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'zip_safe'
  warnings.warn(msg)
/usr/lib/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'test_suite'
  warnings.warn(msg)
running clean
removing 'build/temp.linux-x86_64-2.7' (and everything under it)
removing 'build'
running build_py
running build_ext
building 'pywatchman.bser' extension
creating build
creating build/temp.linux-x86_64-2.7
creating build/temp.linux-x86_64-2.7/pywatchman
x86_64-linux-gnu-gcc -pthread -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fno-strict-aliasing -Wdate-time -D_FORTIFY_SOURCE=2 -g -fstack-protector-strong -Wformat -Werror=format-security -fPIC -I/usr/include/python2.7 -c pywatchman/bser.c -o build/temp.linux-x86_64-2.7/pywatchman/bser.o
x86_64-linux-gnu-gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions -Wl,-Bsymbolic-functions -Wl,-z,relro -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -Wdate-time -D_FORTIFY_SOURCE=2 -g -fstack-protector-strong -Wformat -Werror=format-security -Wl,-Bsymbolic-functions -Wl,-z,relro -Wdate-time -D_FORTIFY_SOURCE=2 -g -fstack-protector-strong -Wformat -Werror=format-security build/temp.linux-x86_64-2.7/pywatchman/bser.o -o /home/glmvn/react_native/watchman/python/pywatchman/bser.so
make[1]: Leaving directory '/home/glmvn/react_native/watchman'
```
tidak aku temukan pesan erorr yang berarti maka langsung tanpa basa basi `$ sudo make install`

{% capture rn_img %}
![Foo]({{"/assets/images/react2.png" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>versi watchman</figcaption>
</figure>

## install flow
beres jugak install watchman lanjut boor heuheuheu lanjutnya adalah install flow cukup download di 
[github flow]( https://github.com/facebook/flow/releases/download/v0.62.0/flow-linux64-v0.62.0.zip) setelah di unzip move ke `usr/local/bin/flow` baru setelah flow tiada halangan yang berarti dilanjutkan dengan install ReactNative
{:style="text-align: justify;"}
## install react native cli
```
$npm install -g react-native-cli

```
waktunya testing dengan membuat project baru `$reactreact-native init TestingApp` langsung deh colok device android dan pastikannya dengan `lsusb` setelah pasti terbaca pastikan konek ke adb dengan `adb devices`masuk ke folder testingApp dan cuss run `react-native run-android`

{% capture rn_img %}
![Foo]({{"/assets/images/react3.png" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>react native run android</figcaption>
</figure>

saatnya buka file `App.js` di IDE ATOM 

{% capture rn_img %}
![Foo]({{"/assets/images/react4.gif" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>react native run android</figcaption>
</figure>

kelebihan reactnative edit,save trus teka `RR` di emulator ajaib

{% capture rn_img %}
![Foo]({{"/assets/images/react5.gif" | relative_url}})
{% endcapture%}
<figure>
	{{rn_img | markdownify | remove: "<p>" | remove: "</p>"}}
	<figcaption>react build</figcaption>
</figure>


okey sekian dulu erorr dan percobaan reactnative kemungkinan besar kesempatan untuk build app mobile dengan react native sangat besar mengingat banyak sekali keuntungannya semangat boor 
{:style="text-align: justify;"}
