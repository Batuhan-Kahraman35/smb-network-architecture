# 1. Bağlam ve Gereksinimler

[🇬🇧 English](../en/01-context-and-requirements.md) · [← README](../../README.tr.md)

## Başlangıç noktası

Düz bir ağ üzerinde çalışan küçük bir ofis:

| Bileşen | Model | Rol |
|---|---|---|
| Router | TP-Link TL-R480T+ (donanım v9, 2018 tarihli firmware) | Çift WAN yük dengeleme, tek LAN |
| Erişim noktası | Tenda F9, AP modunda | Ofis Wi-Fi'ı |
| NAS | Zyxel NAS542, 4 × 1 TB | Dosya deposu |
| Sunucu | HPE ProLiant DL360 Gen9, 4 × 1 GbE | Kullanılmıyor, boşta |
| İnternet | Üç ayrı operatörden üç hat, biri statik IP'li | İkisi router'a bağlı, biri ayrı |

Üç ISP modeminden ikisi **kendi Wi-Fi ağlarını da yayınlıyor**.

## Gereksinimler

1. **Ağ bazlı seçimle çoklu WAN.** Üç hattı yedekli kullanmak ve hangi ağın hangi hattan çıkacağına karar verebilmek.
2. VPN ile ofis ağına **uzaktan erişim**.
3. **Wi-Fi erişim kontrolü.** Wi-Fi şifresini öğrenen birinin otomatik olarak internete çıkamaması.
4. Yeni sanal makineler için **güvenli yedekleme**.
5. **Kısıt:** şimdilik yeni ağ ekipmanına bütçe yok. Eldekiyle çözülmeli.

## Mevcut ekipmanın değerlendirmesi

### Router (TL-R480T+)

- Yük dengeleme, policy routing ve switch portlarında 802.1Q VLAN destekliyor.
- **Ancak** LAN'da tek bir IP arayüzü var. VLAN'lara ayrı alt ağ ve gateway verilemiyor, bu yüzden "X portu WAN1'den, Y portu WAN2'den çıksın" yapılamıyor. WAN seçimi yalnızca **kaynak IP aralığına** göre çalışıyor.
- **VPN sunucusu yok.**
- **Kablolu ve kablosuz** istemciyi ayırt edemiyor. MAC filtresi tüm LAN'a uygulanıyor, yani beyaz liste kablolu cihazları da dışarıda bırakıyor.

### Erişim noktası (Tenda F9)

- MAC beyaz listesi var ama kişi bazlı kimlik doğrulama yok (WPA2-Enterprise ve PPSK yok).
- AP modunda, üstteki router yeniden başladıktan sonra zaman zaman **kendi DHCP sunucusunu açıyor** ve ofis ağını bozuyor.
- Telefonlar giderek daha fazla **rastgele MAC adresi** kullanıyor. Bu ağ için kapatılmazsa MAC tabanlı kurallar kırılgan oluyor.

### ISP modemleri

- Bir modemin Wi-Fi'ına bağlanan cihaz router'daki tüm kontrolleri atlıyor. **Bu ağlar açık kaldıkça hiçbir erişim kontrolünün anlamı yok.**

### NAS (Zyxel NAS542)

- **2024'ten beri desteği bitmiş (EOL)** ve kamuya açık teknik analizleri bulunan kritik uzaktan komut çalıştırma açıklarından etkilenen bir firmware ile çalışıyor.
- Boştayken CPU sürekli %96–100.
- Ayrıntılar [vaka çalışmasında](case-studies/eol-nas-hardening.md).

### Sunucu (DL360 Gen9)

- 1U Xeon rack sunucu: bir firewall ve birkaç sanal makine için fazlasıyla yeterli. 4 dahili gigabit port ve boş PCIe yuvaları var.
- VMware ESXi 8 tarafından resmi olarak desteklenmiyor (HPE, Gen9'u en fazla ESXi 7.0 U3 için listeliyor).

## Değerlendirilen seçenekler

| Seçenek | Sonuç |
|---|---|
| TL-R480T+ ile devam edip etrafından dolaşmak | ❌ VPN yok, ağ bazlı WAN yok, Wi-Fi görünürlüğü yok |
| Hepsi bir arada cihaz (ör. TP-Link Omada ER706W) ya da Omada router + AP'ler | ✅ Teknik olarak uygun, ❌ şimdilik bütçe yok |
| DL360'ı Windows Server ile router yapmak | ❌ Bkz. [ADR-0004](adr/0004-windows-server-router-olmayacak.md) |
| **DL360 + Proxmox VE + OPNsense** | ✅ **Seçildi**. Bkz. [ADR-0001](adr/0001-mevcut-sunucunun-router-olarak-kullanilmasi.md) |

Sonraki: [2. Mimari →](02-mimari.md)
