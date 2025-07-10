# TryHackMe: Linux Fundamentals Write-up

![Linux Fundamentals Banner](https://assets.tryhackme.com/room-badges/825c62f4a7e18b354f33ba58318c3358.png) ## 1. Özet Bilgiler

* **Platform:** TryHackMe
* **Oda Adı:** Linux Fundamentals (Part 1)
* **Zorluk:** Easy
* **Oda Linki:** [https://tryhackme.com/room/linuxfundamentalspart1](https://tryhackme.com/room/linuxfundamentalspart1)

## 2. Odanın Amacı ve Öğrenilen Beceriler

Bu oda serisi, Linux işletim sisteminin temel komut satırı kullanımını, dosya sistemi hiyerarşisini ve temel görev otomasyonunu öğretmeyi amaçlamaktadır.

**Bu odadan kazanılan yetkinlikler:**
* Temel Bash komutları (`ls`, `cd`, `pwd`, `cat`, `echo`)
* Dosya ve dizin yönetimi (`mkdir`, `touch`, `rm`, `cp`, `mv`)
* Metin arama ve işleme (`grep`, `find`)
* İzin yönetimi (`chmod`)
* Komut operatörleri (`>`, `&&`)
* SSH ile uzaktan bağlantı

## 3. Çözüm Metodolojisi (Walkthrough)

Bu bölümde, odadaki her bir görevin (task) nasıl tamamlandığı adım adım açıklanmıştır.

### Task 1 & 2: Giriş ve Teorik Bilgi
Bu ilk görevlerde, sanal makine başlatıldı ve Linux'un tarihi, dağıtımları gibi temel teorik sorular cevaplandı. Pratik bir komut kullanımı bulunmamaktadır.

### Task 3: Dosya İçeriklerini Görüntüleme
Bu görevde, `cat [dosya_adı]` komutu kullanılarak bir metin dosyasının içeriğini terminale yazdırma hedefine ulaşıldı. Bu, loglar veya yapılandırma dosyaları gibi verileri hızlıca incelemek için temel bir adımdır.

### Task 4: Konum ve Kimlik Tespiti
Bu görevde, `pwd` komutu kullanılarak mevcut çalışma dizininin tam yolunu öğrenme hedefine ulaşıldı. Ayrıca, `whoami` komutu ile o anki kullanıcı kimliğimin ne olduğunu doğruladım. Karşılaşılan zorluk, karmaşık bir sistemde kaybolma ihtimaliydi; bu komutlar sayesinde nerede ve kim olduğumu anlama yaklaşımı benimsendi.

### Task 5: Dosya Arama
Bu görevde, `find` komutu kullanılarak dosya sisteminde belirli bir isme sahip dosyaları bulma hedefine ulaşıldı. Örneğin, `find -name "access.log"` komutu ile sisteme dağılmış log dosyalarını tespit etme pratiği yapıldı.

### Task 6: Komut Operatörlerine Giriş
Bu görevde, `>` (yönlendirme) ve `&&` (ve) operatörlerinin kullanımı öğrenildi. `echo "Dosyaya Yaz" > yeni_dosya.txt` komutu ile bir komutun çıktısını bir dosyaya yazdırma hedefine ulaşıldı. `&&` operatörü ise, bir komutun ancak bir önceki başarılı olursa çalışmasını sağlayarak script'lerde koşullu otomasyon mantığını anlamamı sağladı.

### Task 7: Final Görevi - Bayrağı Bulma
Bu son görevde, önceki adımlarda öğrenilen tüm becerileri birleştirme hedefine ulaşıldı. `find` komutu ile sistemin derinliklerindeki `tryhackme.txt` dosyası bulundu, `cd` ile o dosyanın bulunduğu dizine gidildi ve son olarak `cat` komutu ile dosyanın içeriği okunarak odanın bayrağı ele geçirildi. Karşılaşılan zorluk, dosyanın standart dizinlerin dışında bir yerde olmasıydı; bu nedenle sistematik bir arama yaklaşımı benimsendi.

## 4. Kritik Komutlar ve Notlar

Bu odada öğrenilen en kritik komutlar ve konseptler:

* `find / -type f -name important.txt`: Tüm dosya sisteminde "important.txt" adında bir dosya aramak.
* `grep "password" logs.txt`: Bir dosya içinde belirli bir metni aramak.
* SSH bağlantısı için `ssh user@ip_adresi` komutunun kullanımı.

## 5. Sonuç ve Değerlendirme

Bu odayı tamamladıktan sonra, Linux'un siber güvenlikteki rolünün ne kadar merkezi olduğunu bir kez daha anladım. Öğrendiğim her komut, bir analistin günlük görevlerinde ne kadar hayati olduğunu bana gösterdi. Sunuculara bağlanmaktan log dosyalarında kanıt aramaya, bir sızma testi esnasında sistemde iz bırakmadan gezinmeye kadar her adımda komut satırı yetkinliğinin vazgeçilmez olduğunu düşünüyorum. Bu temel beceriler, siber güvenlik alanında Türkiye'ye faydalı olma hedefime ulaşmamda benim için sağlam bir temel oluşturuyor.

---
