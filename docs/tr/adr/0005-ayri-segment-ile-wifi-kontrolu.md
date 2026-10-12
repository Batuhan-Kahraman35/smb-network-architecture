# ADR-0005: Wi-Fi erişim kontrolü ayrı bir fiziksel segment üzerinden

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

Ofis, Wi-Fi şifre paylaşımını durdurmak istiyor. Tek erişim noktası, VLAN etiketleyemeyen ve kişi bazlı kimlik doğrulama yapamayan ev tipi bir cihaz. Yönetilebilir switch yok. AP (köprü) modunda firewall kablosuz istemcileri kendi MAC'leriyle görüyor ve kablolulardan ayıramıyor. İki ISP modemi de kendi Wi-Fi'ını yayınlıyor.

## Karar

- Erişim noktaları **yalnızca** firewall'daki ayrı bir porta (WIFI arayüzü) bağlanacak. Ofis switch'ine asla bağlanmayacak.
- O arayüzde: yalnızca bilinen MAC'lere DHCP, MAC beyaz listesi firewall kuralı, misafirler için kuponlu captive portal ve özel ağlara erişim yok.
- **Tüm ISP modemlerinde Wi-Fi kapatılacak.**

Ayrıntılar: [3. Wi-Fi erişim kontrolü](../03-wifi-erisim-kontrolu.md)

## Değerlendirilen alternatifler

| Alternatif | Neden değil |
|---|---|
| PPSK / WPA2-Enterprise destekli yönetilebilir AP'ler | En iyi çözüm, henüz bütçe yok |
| AP üzerinde MAC beyaz listesi | Görünürlük yok, modem Wi-Fi'larını kapsamıyor |
| VLAN'lar | VLAN destekli AP ve yönetilebilir switch gerekiyor, ikisi de yok |
| AP'yi router (NAT) moduna almak | Tüm istemcileri tek MAC'in arkasına gizliyor, cihaz bazlı kontrol kayboluyor |

## Sonuçlar

**Olumlu:** Wi-Fi istemcileri tamamen görünür, cihaz bazlı kontrol, misafir kuponları, AP'nin ara sıra açtığı sahte DHCP kendi segmentinde kalıyor ve Wi-Fi'ın kendi çıkış hattı oluyor.

**Olumsuz:** MAC tabanlı, yani taklit edilebilir. Kullanıcıların bu ağ için MAC rastgeleleştirmesini kapatması gerekiyor. Bir sunucu portu bu işe ayrılıyor.

**Tekrar değerlendirme:** WPA2/3-Enterprise ya da PPSK destekli, VLAN uyumlu erişim noktaları alındığında.
