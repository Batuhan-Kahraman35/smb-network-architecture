# Changelog / Değişiklik Günlüğü

Newest first. · En yeni en üstte.

## 2026-10-12

**EN**
- **NAS:** Assessed and rebuilt the end-of-life NAS: firmware ABAG.5 → ABAG.15 (2024 security patch), wiped, recreated RAID 10, factory reset, disabled unneeded services, created a least-privilege backup user and hidden share. CPU 96–100% → 11–20%. [Case study](docs/en/case-studies/eol-nas-hardening.md)
- **Architecture:** Target design agreed: Proxmox VE + OPNsense on an existing DL360 Gen9, three WANs, dedicated Wi-Fi segment, WireGuard. ADR-0001 to ADR-0006 recorded.
- **Docs:** Repository created.

**TR**
- **NAS:** Desteği bitmiş NAS incelenip sıfırdan kuruldu: firmware ABAG.5 → ABAG.15 (2024 güvenlik yaması), veriler silindi, RAID 10 yeniden oluşturuldu, fabrika ayarlarına dönüldü, gereksiz servisler kapatıldı, en az yetkili yedekleme kullanıcısı ve gizli paylaşım oluşturuldu. CPU %96–100 → %11–20. [Vaka çalışması](docs/tr/case-studies/eol-nas-hardening.md)
- **Mimari:** Hedef tasarım belirlendi: mevcut DL360 Gen9 üzerinde Proxmox VE + OPNsense, üç WAN, ayrı Wi-Fi segmenti, WireGuard. ADR-0001 – ADR-0006 yazıldı.
- **Dokümantasyon:** Repo oluşturuldu.

## 2026-10-08

**EN**
- **Assessment:** Reviewed the existing load-balance router and access point. Found: no VPN server, no per-network WAN selection (single LAN interface), no way to tell wired from wireless clients, the access point occasionally starting its own DHCP server, ISP modems broadcasting uncontrolled Wi-Fi.

**TR**
- **İnceleme:** Mevcut yük dengeleme router'ı ve erişim noktası incelendi. Tespitler: VPN sunucusu yok, ağ bazlı WAN seçimi yok (tek LAN arayüzü), kablolu/kablosuz istemci ayrımı yapılamıyor, erişim noktası zaman zaman kendi DHCP'sini açıyor, ISP modemleri kontrolsüz Wi-Fi yayını yapıyor.
