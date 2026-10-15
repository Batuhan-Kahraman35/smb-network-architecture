# Vaka Çalışması: İkinci El Bir Rack Sunucuyu Sanallaştırma Sunucusuna Dönüştürmek

[🇬🇧 English](../../en/case-studies/second-hand-server-revival.md) · [← README](../../../README.tr.md)

**Donanım:** HPE ProLiant DL360 Gen9 (2 × Xeon E5-2620 v4, 64 GB DDR4, Smart Array P440ar, iLO 4 Advanced) · **Tarih:** 2026-10-12 → 10-13

## Başlangıç durumu

Sunucu uzun süredir kullanılmıyordu ve ikinci el alınmıştı. iLO üzerinden uzaktan yapılan inceleme:

| Bulgu | Risk |
|---|---|
| Açılış döngüsü: *"No bootable devices were detected"* | Önceki sahibi **bütün diskleri sökmüş** |
| 2017 tarihli System ROM, 2020 tarihli iLO firmware'i | İşlemci mikrokod düzeltmeleri (Spectre/Meltdown dönemi) ve iLO güvenlik yamaları eksik |
| Önceki sahipten kalanlar: sunucu adı, tam yetkili bir izleme hesabı, dinamik DNS kaydı | Yönetim kartında tam yetkili, bilinmeyen hesaplar |
| Elektrik kesilince saat 01/01/1970'e dönüyor | Zayıf CMOS pili. Yanlış saat TLS, VPN ve log'ları bozar |

## Yapılanlar

1. **Yönetim kartı temizliği.** Kalan yönetici hesabı ve DDNS kaydı silindi, yerel yönetici şifresi değiştirildi.
2. **Firmware.** iLO 4 2.77 → **2.82**, System ROM P89 v2.42 → **v3.40 (2024)**. Paketler yalnız üreticiden indirildi ve yüklemeden önce **SHA-256** ile doğrulandı. ROM güncellemesinde yedek ROM bankası eski sürümde kaldı; gerekirse geri dönüş yolu bu.
3. **Diskler.** Elde başka bir sunucudan çıkan iki adet 2,5" 10K SAS disk vardı. Kasanın kızakları 3,5". Bir disk yerine oturtuldu; ikincisi için konnektörü taşıyan bir 2,5"→3,5" dönüştürücü gerekiyor (düz braket işe yaramaz).
4. **Kontrolcü HBA modunda.** P440ar, sanallaştırma katmanı diskleri doğrudan görsün diye RAID modundan **HBA moduna** alındı. Kurulumda doğrulandı: Kurulum programı diski *"LOGICAL VOLUME"* olarak değil, model adıyla listeledi.
5. **Proxmox VE 9**, ISO **iLO sanal medyasına URL ile bağlanarak** kuruldu. USB bellek ya da sunucunun başında birinin olması gerekmedi. Şimdilik tek diskte ZFS; ikinci disk sonradan ayna olarak eklenecek (`zpool attach`), yeniden kurulum gerekmiyor.
6. **Kurulum sonrası:** Topluluk deposu, tam güncelleme, NTP kontrolü, ZFS/SMART kontrolü, elektrik kesintisinden sonra otomatik açılma, NAS'ın yedek deposu olarak eklenmesi.

## Sorunlar ve çözümleri

| Sorun | Neden | Çözüm |
|---|---|---|
| Takılan disk iLO'da görünmüyor | iLO envanteri yalnız POST'ta yeniliyor. Ayrıca 3,5" kızaktaki 2,5" disk backplane'e ulaşmıyordu | Disk yeniden oturtuldu, sunucu yeniden başlatıldı. İkinci disk için dönüştürücü sipariş edildi |
| İkinci güç kaynağında "input voltage lost" | Ön yuvalarda çalışırken güç kablosu çıkmış | Yeniden takıldı, yedeklilik geri geldi |
| Kurulumdan sonra root şifresi kabul edilmiyor | HTML5 uzak konsol, kurulum sırasında klavye düzenini farklı çevirdi | Açılış menüsünden (`init=/bin/bash`) sıfırlandı, sonra web arayüzünden güçlü şifre verildi. **Ders:** Uzak konsoldan yazılacak şifrelerde yalnız harf ve rakam kullanılmalı |
| Üretici yansısından ISO indirmesi ofis hattında sürekli kopuyor | Çoklu bağlantılı bir indirme yöneticisinden sonra ofis IP'si yansı sunucusu tarafından sınırlandı | Başka bir sunucuya indirildi, **resmi SHA-256 listesiyle** doğrulandı, geçici olarak yalnız ofis IP'sine açıldı |

## Sonuç

| | |
|---|---|
| Sanallaştırma | Proxmox VE 9.2, kernel 7.0, ZFS |
| Disk sağlığı | SMART OK, 0 hatalı sektör. Ama **yaklaşık 61.000 saat çalışmış**; bu yüzden ayna ve gecelik yedek öncelikli |
| Yönetim | iLO yalnız LAN'da, firmware güncel, bilinmeyen hesap yok |
| Bilinen kısıtlar | Ofis switch'i yüzünden LAN portu 100 Mb/s. Dönüştürücü gelene kadar tek disk |

İlgili: [ADR-0001 Mevcut sunucunun router olarak kullanılması](../adr/0001-mevcut-sunucunun-router-olarak-kullanilmasi.md) · [ADR-0002 ESXi yerine Proxmox](../adr/0002-esxi-yerine-proxmox.md)
