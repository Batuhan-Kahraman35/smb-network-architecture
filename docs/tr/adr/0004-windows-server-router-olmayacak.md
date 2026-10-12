# ADR-0004: Windows Server router olarak kullanılmayacak

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

Sunucunun işletim sistemi olarak Windows Server 2022 önerildi; yönlendirme ve VPN'i RRAS rolü yapacaktı.

## Karar

Windows Server router ya da firewall **olmayacak**. Gerekirse (Active Directory, dosya paylaşımı, NPS/RADIUS) OPNsense'in **yanında bir sanal makine** olarak çalışacak.

## Gerekçe

| Gereksinim | Windows Server (RRAS) |
|---|---|
| Çoklu WAN yük dengeleme / yedekleme | Gerçek yük dengeleme yok. Yedekleme yalnızca rota önceliğiyle (metric) |
| Ağ bazlı WAN seçimi | Pratikte kaynak bazlı policy routing yok |
| VPN | SSTP / IKEv2 / L2TP var, yerleşik WireGuard yok |
| İnternete bakan saldırı yüzeyi | Amaca özel firewall dağıtımlarından çok daha büyük |
| Güncellemeler | Aylık toplu güncellemeler genelde yeniden başlatma istiyor, yani **her ay tüm ofiste internet kesintisi** |
| Maliyet | Sunucu lisansı + CAL'lar, karşısında ücretsiz |

Windows Server problemin bir parçası için *daha iyi* araç: Active Directory kullanıcılarıyla WPA2-Enterprise için **NPS (RADIUS)**. Bu yüzden tasarımda opsiyonel bir sanal makine olarak duruyor.

## Sonuçlar

- Yönlendirme ve güvenlik, amaca özel ve sade bir sistemde kalıyor.
- Windows güncellemeleri artık internet bağlantısını etkilemiyor.
