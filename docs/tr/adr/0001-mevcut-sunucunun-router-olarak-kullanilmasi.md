# ADR-0001: Mevcut rack sunucuyu router olarak kullanmak

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

Ofisin ağ bazlı hat seçimiyle çoklu WAN'a, bir VPN sunucusuna ve Wi-Fi erişim kontrolüne ihtiyacı var. Mevcut yük dengeleme router'ı bunların hiçbirini sağlamıyor (bkz. [Bağlam](../01-baglam-ve-gereksinimler.md)). Şu an yeni ağ ekipmanı için bütçe yok. Dört gigabit portu olan bir HPE ProLiant DL360 Gen9 kullanılmadan duruyor.

## Karar

Router, **DL360 üzerinde Proxmox VE altında sanal makine (OPNsense)** olarak çalıştırılacak. Üç WAN, LAN ve ayrı bir Wi-Fi segmentine yetecek port için ikinci el 4 portlu bir Intel ağ kartı eklenecek.

## Değerlendirilen alternatifler

| Alternatif | Neden (şimdilik) değil |
|---|---|
| Hepsi bir arada cihaz (ör. TP-Link Omada ER706W) | Uygun: çoklu WAN, VPN, dahili Wi-Fi 6, merkezi yönetim. Yalnızca bütçe nedeniyle reddedildi |
| Omada router + yönetilebilir AP'ler + controller | Uzun vadede en iyi Wi-Fi çözümü (PPSK, SSID bazlı VLAN). Aynı bütçe sorunu |
| OPNsense'i doğrudan sunucuya kurmak (bare metal) | Arıza senaryoları daha basit, ama sunucu boşa gider ve Windows Server ya da başka VM'lere yer kalmaz |
| Mevcut router ile devam | Gereksinimleri karşılayamıyor |

## Sonuçlar

**Olumlu**

- Neredeyse sıfır harcamayla (tek bir ikinci el ağ kartı) tüm gereksinimler karşılanıyor.
- Kurumsal seviyede firewall özellikleri: gateway grupları, captive portal, WireGuard, API.
- VM snapshot ve yedekleri sayesinde firewall değişiklikleri geri alınabiliyor.

**Olumsuz**

- **Tek arıza noktası.** Sunucu kapanırsa internet gider. Önlem: [geri dönüş runbook'u](../runbooks/eski-routera-donus.md) ve ayarlarıyla rafta bekleyen eski router.
- Hazır bir cihaza göre daha yüksek enerji tüketimi, ses ve ısı.
- Hipervizör bakımı ağı durduruyor. Host güncellemeleri bakım penceresinde yapılıyor.

**Tekrar değerlendirme:** yönetilebilir erişim noktaları ya da ayrı bir cihaz için bütçe oluştuğunda.
