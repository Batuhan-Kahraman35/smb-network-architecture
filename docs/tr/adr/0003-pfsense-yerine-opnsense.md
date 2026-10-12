# ADR-0003: Firewall olarak pfSense değil OPNsense

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

OPNsense de pfSense de işlevsel ihtiyaçları karşılıyor: çoklu WAN gateway grupları, arayüz bazlı policy routing, WireGuard, kuponlu captive portal, VLAN ve FreeRADIUS paketi. İkisi aynı kökten (m0n0wall) geliyor, kavramlar birinden diğerine kolayca aktarılıyor.

## Karar

**OPNsense** kullanılacak.

## Gerekçe

| Kriter | OPNsense | pfSense |
|---|---|---|
| Ticari kullanımda lisans | Tamamen açık kaynak, ticari kullanım ücretsiz | CE ücretsiz. Plus ticari sürüm ve ücretsiz kullanımı ev/lab ile sınırlandırıldı. Şartlar zaman içinde değişti |
| Sürüm sıklığı | Yılda iki büyük sürüm, sık küçük/güvenlik güncellemeleri | CE sürümleri seyrekleşti. Yeni özellikler önce Plus'a geliyor |
| WireGuard | Çekirdekte | Paket olarak |
| REST API | Yerleşik, özelliklerin çoğunu kapsıyor | CE'de daha sınırlı |
| Topluluk ve rehberler | Geniş | Daha geniş |

Kararı belirleyenler **bir şirket ağı için öngörülebilir lisans** ve Wi-Fi cihaz eklemeyi otomatikleştirmek için planlanan **yerleşik API** oldu.

## Sonuçlar

- Ticari kullanım için lisans incelemesi gerekmiyor.
- pfSense'e göre biraz daha az üçüncü taraf rehber var, ama resmi dokümantasyon iyi.
