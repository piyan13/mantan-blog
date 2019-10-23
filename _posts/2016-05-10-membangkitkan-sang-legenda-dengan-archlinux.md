---
title: "Membangkitkan sang legenda dengan Arch linux"
tags:
 Linux
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
---  

<figure style="width: 200px" class="align-center">
<img src="/images/aspire.png">
<figcaption>legenda acer aspire 5315</figcaption>
</figure> 
Acer aspire 5315 adalah alat tempur yang melegenda bagi saya dengan sejarah yang cukup panjang menemani belajar dikala susah maupun senang saat di perguruan tinggi ,terhitung dari tahun 2008an hingga awal 2014 sang legenda resmi menjadi hiasan di meja gudang penuh amunisi dikarenakan salah diagnosa penyakitnya. Alhamdulilah awal bulan mei bisa ketemu ahli perangkat keras dokter spesialis muhammad ngok sekarang bukak klinik di ketintang barat memberi wacana kalau mesin aspire 5315 saya normal hanya saja lcdnya yg bermasalah jadi kepikiran untuk mempersiapkannya untuk menambah amunisi developing saya. alkisah dari dulu laptop ini sangat pemilih dalam hal operating system hanya bisa berjalan sempurna di keluarga debian seperti ubuntu dan linuxmint ada saja kendala kalau di install selain turunan debian mulai kernel panic hingga freezy itu berlaku untuk varian distro linux lama dan baru akhirnya nekat install distro arch linux karena paketnya kita build terpisah sesuai kebutuhan, seperti apa perjalanannya mari kita ulas satu persatu
{: style="text-align: justify;"}

{% include toc title="Contents" icon="file-text" %}

salah satu hal yang menarik dari menginstall operating system linux adalah
sisi dokumentasinya yang sangat lengkap keuntungan opensource dengan komunitas terbesar jadi bahan untuk bacaan atau referensi di kala terjadi error gak usah panic tinggal check di forumnya atau wikipedianya karena error itulah pelajaran baru akan dimulai pintu pengetahuan sudah menjelang di pelupuk mata kali ini referensi lengkapnya ada di [arch wiki](https://wiki.archlinux.org/){:style="text-align: justify;"} 

## Mode Partisi

{% capture arch_img %}
![Foo]({{"/assets/images/booting.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>booting liveusb</figcaption>
</figure>

**Note:** untuk persiapan booting mode liveusb dan mentahan .iso arch bisa kunjungi [download](https://www.archlinux.org/download/)
{: .notice--info}
setelah booting dengan liveusb barulah install mode command line dimulai dengan
partisi menggunakan applikasi `cfdisk`

```ruby
 root@archiso~# cfdisk
```

{% capture arch_img %}
![Foo]({{"/assets/images/cfdisk.png" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>Credit google</figcaption>
</figure>


saya cuman butuh 2 partisi yaitu partisi linux filesystem dan swap tinggal sesuain sama kebutuhan saja, kalau hardisknya sudah ada partisinya dan ragu bagian mana yang akan digunakan format atau delete bisa menggunakan `cfdisk -l` untuk melihat ada berapa partisi di hardisk

```ruby
 root@archiso~# fdisk -l
```

## Format filesystem dan Swap

partisi telah siap dan saatnya untuk format filesystem saya memimilih ext4 lalu  mount partisi root pada directory /mnt

```ruby
 root@archiso~# mkfs.ext4 /dev/sda1
 root@archiso~# mount /dev/sda1 /mnt
```
**Note:** ada beberapa type dari filesystem untuk referensi lebih lanjut bisa baca baca [arch wiki file systems](https://wiki.archlinux.org/index.php/file_systems)
atau bisa jugak ingin mengetahui perbedaan ext2 ext3 dan ext4 bisa kunjungi artikel ini [theegeekstuff](http://www.thegeekstuff.com/2011/05/ext2-ext3-ext4/)
{: .notice--info}

filesystem telah siap tinggal kita format partisi swap , apa itu swap ? silahkan ke [arch wiki swap](https://wiki.archlinux.org/index.php/Swap)

```ruby
 root@archiso~# mkswap /dev/sda2
 root@archiso~# swapon /dev/sda2
```
**peringatan:** pastikan mengingat /dev/sda dari filesystem dan swap yang sudah di alokasikan di mode partisi jangan sampai ketuker antara swap dan filesystem
{: .notice--warning}

## Waktunya download dan install package base

{% capture arch_img %}
![Foo]({{"/assets/images/pacstrap.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

```ruby
 root@archiso~# pacstrap -i /mnt base
```
**Note:** untuk terlebih dahulu memastikan ada koneksi internet karena saya memakai jaringan wifi jadi konektivitasnya di jembatani oleh `wpa_supplicant` dengan `wifi-menu` silahkan check [arch wiki internet](https://wiki.archlinux.org/index.php/Wireless_network_configuration#Getting_some_useful_information)
{: .notice--info}

**Tips:** jangan lupa untuk menginstall app dan library konektivias internet untuk selanjutnya digunakan pada saat setelah installasi selesai misal `iw`,`dialog`,`wpa_supplicant`
{: .notice--danger}

setelah terinstall dengan sempurna paket base di /mnt selanjutnya adalah configurasi fstab tugasnya file ini bisa dibaca di [arch wiki fstab](https://wiki.archlinux.org/index.php/Fstab) 


```ruby
 root@archiso~# genfstab -U /mnt >> /mnt/etc/fstab
```
{% capture arch_img %}
![Foo]({{"/assets/images/fstab.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

## Membuat password root dan user

akun root atau superuser digunakan system adminitration misal untuk melakukan kegiatan install dan akses folder folder yang ada di system, maka selain kegiatan sifatnya biasa di sarankan untuk membuat akun lagi selain root . karena arch mode liveusb menggunakan auto login maka untuk kepentingan keamanan maka kita harus memasukkan password root

{% capture arch_img %}
![Foo]({{"/assets/images/chroot.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>


```ruby
 root@archiso~# arch-chroot /mnt /bin/bash
```

setelah masuk ke mode root langsung deh `passwd` 

{% capture arch_img %}
![Foo]({{"/assets/images/passwd.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

oke beres password superuser root saatnya membuat user lain untuk tugas tugas sederhana dan untuk keamanan jugak, sebelum itu install dulu paket sudo fungsinya untuk memberikan beberapa ijin akses ke system akun user

{% capture arch_img %}
![Foo]({{"/assets/images/sudopac.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

```ruby
 sh-4.3#pacman -S sudo bash-completion
```
setelah terinstall baru kita menambah user dan memberikannya password

```ruby
 sh-4.3# useradd -m -g users -G wheel,storage,power -s /bin/bash <namauser>
 passwd <namauser>
```
{% capture arch_img %}
![Foo]({{"/assets/images/useradd.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

nah setelah itu baru kita konfigurasi user yang baru ditambahkan untuk berada dalam grup wheel agar bisa menjalankan tugas tugas yang membutuhkan akses ke system dengan sudo

```ruby
 sh-4.3#EDITOR=nano visudo
```
setelah masuk ke teks editor nano hapus tanda # `%whell ALL=(ALL) ALL` seperti pada gambar dibawah ini 

{% capture arch_img %}
![Foo]({{"/assets/images/sudoers.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>


beres dah masalah useradd langsung memasukkan nama hostname komputer berguna saat koneksi antar komputer

```ruby
 sh-4.3# echo nama komputer > /etc/hostname
```
**peringatan:** Hayo looh ingat ingat lagi password root dan user jangan sampe kelupaan
{: .notice--danger}

## konfigurasi lokasi bahasa dan zona waktu

saya menggunakan bahasa inggris saja karena sudah terbiasa bor 

```ruby
 sh-4.3#nano /etc/locale.gen
```
hapus tanda # di lokasi dan bahasa yang dipilih setelah itu klik `locale-gen` untuk mengenerate

{% capture arch_img %}
![Foo]({{"/assets/images/locale.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

selanjutanya menentukan zona waktu

```ruby
 sh-4.3#ls /usr/share/zoneinfo/
```
maka akan muncul gambar dibawah ini karena saya menggunakan jakarta dia berada di directory Asia


{% capture arch_img %}
![Foo]({{"/assets/images/zoneinfo.png" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

```ruby
 sh-4.3#ln -s /usr/share/zoneinfo/Asia/Jakarta
```
okey beres zona waktu , bahasa dan lokasi sudah terkonfigurasi dengan benar

## Install Bootloader dan beres beres 

kegiatan kali ini diawali dengan bash script mkinitcpio yang digunakan untuk membuat initial ramdisk environment [arch wiki mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio)

```ruby
 sh-4.3#mkinitcpio -p linux
```
setelah itu download dan install grub 

```ruby
 sh-4.3#pacman -S grub_bios
 sh-4.3#grub install /dev/sda
```

{% capture arch_img %}
![Foo]({{"/assets/images/grubpac.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption></figcaption>
</figure>

konfigurasi boot di grub

```ruby
 sh-4.3#grub-mkconfig -o /boot/grub/grub.cfg
```

fiuuuuuh akhirnya saat saat terakhir telah tiba dalam daya dan upaya membangkitkan sang legenda 5315 tinggal unmount dan reboot jurus pamungkasnya
check this out bor

{% capture arch_img %}
![Foo]({{"/assets/images/fiuh.gif" | relative_url}})
{% endcapture %}
<figure>
    {{arch_img | markdownify | remove: "<p>" | remove: "</p>"}}
    <figcaption>Finally</figcaption>
</figure>

Alhamdulilah sang legenda kini menampaki ruang baru dan menjadi alat tempur andalan saya lagi ini bener bener masih permulaan, ya awal yang baru sejarah baru buat aspire karena ternyata tidak hanya distro debian tapi arch jugak bisa keep badass lah pokoke