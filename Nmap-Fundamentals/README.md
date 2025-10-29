# TryHackMe: Nmap (Further Nmap) Write-up

## 1. Özet Bilgiler

* **Platform:** TryHackMe
* **Oda Adı:** Nmap (Further Nmap)
* **Zorluk:** Easy
* **Oda Linki:** [https://tryhackme.com/room/furthernmap](https://tryhackme.com/room/furthernmap)

## 2. Odanın Amacı ve Öğrenilen Beceriler

Bu oda, Nmap'in temel port tarama özelliklerinin ötesine geçmeyi amaçlamaktadır. Katılımcıların, gelişmiş tarama tekniklerini, güvenlik duvarı atlatma yöntemlerini ve Nmap Scripting Engine'i (NSE) kullanarak daha derinlemesine bilgi toplamasını hedefler.

Bu odadan kazanılan yetkinlikler:

* Farklı Nmap Tarama Türleri (`-sS`, `-sT`, `-sN`, `-sF`, `-sX`) ve aralarındaki farklar
* Host Discovery (Canlı Sistem Keşfi) ve ICMP Engelleme (`-sn`, `-Pn`)
* Zamanlama, Parçalama ve Gizlilik Teknikleri (`-f`, `--data-length`, `--scan-delay`)
* Güvenlik Duvarı (Firewall) ve IPS/IDS Davranış Analizi (`no-response` çıktısını yorumlama)
* Nmap Scripting Engine (NSE) Kullanımı (`--script`, `--script-help`, `--script-args`)
* Ağ Sorun Giderme (VPN bağlantı sorunları ve paket kaybı analizi)

## 3. Çözüm Metodolojisi (Walkthrough)

Bu bölümde, odadaki görevlerin nasıl tamamlandığı ve özellikle karşılaşılan zorlukların nasıl aşıldığı açıklanmıştır.

### Task 1-7: Teorik Bilgi ve Temel Taramalar

İlk görevler, farklı tarama türlerinin (TCP Connect, SYN, NULL, FIN, Xmas) teorik altyapısını anlamaya odaklandı. `-sn` ile ping taraması yapmayı ve CIDR notasyonunu (`/16` vs `/24`) doğru kullanmayı öğrendim.

### Task 8-10: Nmap Scripting Engine (NSE)

Bu bölümde NSE'nin gücünü keşfettim.

* `--script=vuln` komutunu kullanarak zafiyet taraması yapmayı.
* `nmap --script-help <script-name>` komutuyla bir script'in `maxlist` gibi opsiyonel argümanlarını veya `dependencies` (bağımlılıklar) bölümünü nasıl okuyacağımı öğrendim.
* `ftp-anon` ve `smb-os-discovery` gibi belirli script'leri hedefe karşı nasıl kullanacağımı uyguladım.

### Task 11-13: Güvenlik Duvarı Atlatma (Firewall Evasion)

Odanın bu bölümü, teorik bilgileri pratiğe dökerken beni en çok zorlayan kısım oldu.

* `--badsum` ve `--data-length` gibi tekniklerle paketlerin imzasını değiştirerek filtreleri atlatma mantığını kavradım.
* `-Pn` komutunun, ICMP (ping) isteklerini engelleyen Windows gibi hedefler için neden hayati olduğunu anladım.

### Task 14: Pratik Uygulama (Laboratuvar)

Bu son görev, odadaki en öğretici kısımdı ve gerçek dünya senaryolarıyla birebir örtüşüyordu.

* **ICMP Engeli:** Hedef makinenin ping'e (ICMP) cevap vermediğini (N) doğruladım. Bu, tüm taramalarımda `-Pn` kullanmamı zorunlu kıldı.
* **VPN ve Ağ Sorunları:** İlk taramalarım (`no-response`) hatasıyla sonuçlandı. Sorunu anlamak için OpenVPN loglarımı incelediğimde `bad packet ID` gibi paket kaybı ve stabilite sorunları gördüm. VPN sunucusunu değiştirerek bu ağ sorununu aştım.
* **Firewall/IPS Engeli:** Stabil VPN'e rağmen, hem Xmas (`-sX`) hem de SYN (`-sS`) taramalarım (`no-response`) hatası vermeye devam etti. Bu, hedefteki güvenlik duvarının sadece "hatalı" Xmas paketlerini değil, "gizli" SYN paketlerini de algılayıp sessizce engellediğini (drop ettiğini) gösterdi.
* **Çözüm:** Engeli aşmak için strateji değiştirdim. Güvenlik duvarını atlatmanın yolu, "gizli" davranmaktan değil, tam tersine "meşru" bir kullanıcı gibi davranmaktan geçiyordu. Tam bir TCP el sıkışması başlatan `-sT` (TCP Connect Scan) komutunu kullandım. Bu tarama, güvenlik duvarı tarafından "normal" bir bağlantı isteği olarak algılandı ve engellenmedi. Bu sayede hedef sistemdeki gerçek açık portları (Port 21, 80 vb.) başarıyla tespit ettim.

## 4. Kritik Komutlar ve Notlar

Bu odadan çıkardığım en kritik dersler ve komutlar:

* `nmap -Pn [hedef-ip]`: Bir taramada ilk kural. Hedef ping'e cevap vermiyorsa (ICMP blokluysa), bu komut olmadan tarama hiç başlamaz.
* `nmap -sS ...` vs `nmap -sT ...`: Gizli (Stealth) tarama (`-sS`) her zaman en iyisi değildir. Modern IPS/Firewall sistemleri bu taramaları tanır ve engeller. Bazen "gürültülü" ama "meşru" görünen bir Connect Taraması (`-sT`) yapmak, filtreyi aşmanın tek yolu olabilir.
* **`no-response` Çıktısı:** Bu çıktının birden fazla anlamı olabileceğini öğrendim:
    * VPN/Ağ bağlantısı yok (Paketler hedefe hiç ulaşmıyor).
    * VPN/Ağ stabil değil (Paketler kayboluyor).
    * Güvenlik duvarı/IPS paketleri aktif olarak engelliyor (drop ediyor).
* `nmap --script=vuln -p- [hedef-ip]`: Notlarımda düzelttiğim en önemli hata. Eğer tüm portları taramak istiyorsam, Nmap'e varsayılan 1000 portun dışına çıkmasını söylemek için `-p-` bayrağını mutlaka eklemeliyim.
* `nmap --script-help <script-name>`: Bir script'in tüm potansiyelini (argümanlar, bağımlılıklar) görmek için en hızlı yol.

## 5. Sonuç ve Değerlendirme

Bu oda, "Linux Fundamentals" odasından çok daha zorlayıcı ve öğreticiydi. Bu sefer mesele sadece komutu çalıştırmak değil, komut çalışmadığında veya beklenmedik bir sonuç verdiğinde "neden?" diye sormaktı.

> Bir siber güvenlik analisti olarak, araçların çıktısını körü körüne kabul edemeyeceğimi anladım. Bir taramanın `no-response` vermesi, "burada bir şey yok" demek değildir; tam tersine, "burada araştırılması gereken bir engel var" demektir.

Ağ sorunlarını, VPN loglarını ve güvenlik duvarı davranışlarını analiz ederek sorunun köküne inebilmek, bir sızma testinin başarısı ile başarısızlığı arasındaki farkı belirler. Bu odada edindiğim sorun giderme ve adaptasyon becerileri, siber güvenlik alanında uzmanlaşma ve ülkeme faydalı olma hedefimde kilit bir rol oynayacaktır.
