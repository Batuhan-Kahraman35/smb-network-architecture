# Changelog / Değişiklik Günlüğü

Newest first. · En yeni en üstte.

## 2026-10-15

**EN**
- **Cut-over:** The office now runs on OPNsense. Old router switched off, OPNsense took over the same gateway address, office switch moved to the server. Wi-Fi was moved first, then a single-PC rehearsal, then the whole office. [Case study](docs/en/case-studies/cutover-to-opnsense.md)
- **Multi-WAN:** ISP A (static IP) is primary, ISP B is automatic failover. Gateway monitoring uses IPs that are not DNS servers.
- **Wi-Fi:** Access point on its own server port and segment, no access to the LAN, **MAC allow-list**: unknown devices get an address but can't reach anything until approved. ISP B modem Wi-Fi switched off.

**TR**
- **Geçiş:** Ofis artık OPNsense'te. Eski router kapatıldı, OPNsense aynı gateway adresini devraldı, ofis switch'i sunucuya taşındı. Önce Wi-Fi, sonra tek PC provası, sonra bütün ofis. [Vaka çalışması](docs/tr/case-studies/opnsense-gecisi.md)
- **Çoklu WAN:** ISP A (statik IP) ana hat, ISP B otomatik yedek. Ağ geçidi izlemesi DNS olarak kullanılmayan IP'lerle yapılıyor.
- **Wi-Fi:** Erişim noktası kendi sunucu portunda ve segmentinde, LAN'a erişimi yok, **MAC izin listesi**: Bilinmeyen cihaz adres alır ama onaylanana kadar hiçbir yere ulaşamaz. ISP B modeminin Wi-Fi'ı kapatıldı.

## 2026-10-13 → 10-14

**EN**
- **Server:** Second-hand DL360 Gen9 cleaned up and updated (iLO 2.82, System ROM 2024), controller in HBA mode, Proxmox VE 9 on ZFS installed over iLO virtual media. [Case study](docs/en/case-studies/second-hand-server-revival.md)
- **OPNsense:** VM installed with its LAN link held down until DHCP was confirmed off; updated to 26.7.7; guest agent; DHCP reservations imported from the old router.
- **Backups:** Nightly snapshot backup of the firewall VM to the NAS (7 daily + 4 weekly). First run: 1.07 GB in 2 minutes.

**TR**
- **Sunucu:** İkinci el DL360 Gen9 temizlenip güncellendi (iLO 2.82, 2024 System ROM), kontrolcü HBA modunda, Proxmox VE 9 iLO sanal medyasıyla ZFS üzerine kuruldu. [Vaka çalışması](docs/tr/case-studies/ikinci-el-sunucu.md)
- **OPNsense:** VM, LAN bağlantısı DHCP kapalı doğrulanana kadar ayrık tutularak kuruldu; 26.7.7'ye güncellendi; guest agent; DHCP rezervasyonları eski router'dan aktarıldı.
- **Yedekler:** Firewall VM'inin NAS'a gecelik snapshot yedeği (7 günlük + 4 haftalık). İlk çalışma: 2 dakikada 1,07 GB.

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
