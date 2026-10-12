# ADR-0006: Desteği bitmiş NAS izole yedek deposu olarak kullanılacak

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

Tek NAS (Zyxel NAS542) 2024'te destek ömrünü tamamladı. Kritik uzaktan komut çalıştırma açıklarından etkilenen bir firmware ile çalışıyordu ve boştayken CPU'su %96–100'deydi. Yeni tasarımda sanal makineler için bir yedek deposu gerekiyor ve yeni NAS için bütçe yok.

## Karar

NAS kullanılmaya devam edecek, ama yalnızca tam bir temizlikten sonra ve yalnızca **izole bir yedek deposu** olarak:

1. 2024 güvenlik yamasını içeren son resmi firmware'e güncelleme.
2. Volume ve yapılandırmayı silip sıfırdan kurma.
3. SMB yedekleme için gerekmeyen tüm servisleri kapatma.
4. En az yetkili, ayrı bir kullanıcı ve gizli bir paylaşım.
5. Firewall'da NAS'ın internete ve Wi-Fi segmentinin NAS'a erişimini engelleme.
6. Kritik verilerin dış konumda bir kopyası.

Ayrıntılı yazı: [Vaka çalışması: desteği bitmiş NAS'ın sertleştirilmesi](../case-studies/eol-nas-hardening.md)

## Sonuçlar

- Yedeklerin artık maliyetsiz bir yeri var.
- **Kalan risk:** gelecekteki açıklar kapatılmayacak. İzolasyon riski azaltıyor, ama cihazın değiştirilmesi (desteklenen bir NAS ya da TrueNAS) yol haritasında kalıyor.
- Diskler masaüstü sınıfı, biri markasız. S.M.A.R.T. her ay kontrol ediliyor.
