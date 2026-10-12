# ADR-0002: Hipervizör olarak VMware ESXi 8 değil Proxmox VE

- **Durum:** Kabul
- **Tarih:** 2026-10-12

## Bağlam

Sunucu, firewall sanal makinesini ve gerekirse bir Windows Server sanal makinesini çalıştıracak. ESXi ve Proxmox VE ikisi de değerlendirildi; ekibin daha önce ESXi deneyimi var.

## Karar

**Proxmox VE** kullanılacak ve sunucunun RAID dizisine kurulacak (USB belleğe değil).

## Gerekçe

| Kriter | ESXi 8 (ücretsiz sürüm) | Proxmox VE |
|---|---|---|
| Lisans | Ücretsiz sürüm 2025'te geri geldi (8.0 U3e), kısıtlamalarla. Şartları birden fazla kez değişti | Ücretsiz ve açık kaynak, isteğe bağlı ücretli destek |
| Sınırlar | VM başına 8 vCPU, vCenter yok | Yok |
| Yedekleme | Ücretsiz sürümde yedekleme API'leri yok, Veeam gibi araçlar VM yedeği alamıyor | Yerleşik vzdump, artımlı yedek için Proxmox Backup Server |
| Donanım desteği | HPE, Gen9'u en fazla **ESXi 7.0 U3** için destekliyor. ESXi 8 çalışıyor ama desteklenmiyor | Gen9'da sorunsuz |
| Firewall VM ağ yapısı | Her port için vSwitch: uygun | Her port için Linux bridge ya da PCI passthrough: uygun |

Kararı belirleyen **yedekleme** oldu. Kendi sanal makinelerini ücretsiz yedekleyemeyen bir hipervizör, ofisin tek router'ı için zayıf bir temel.

## Proxmox neden USB'ye kurulmuyor

ESXi USB/SD karttan açılmak üzere tasarlanmıştı. Proxmox ise sistem diskine sürekli log, küme yapılandırması ve RRD metrikleri yazar ve bir USB belleği birkaç hafta ila birkaç ay içinde aşındırır. USB yalnızca **kurulum medyası** olarak kullanılıyor.

## Sonuçlar

- Lisans riski yok, yedekleme yerleşik.
- Ekip Proxmox'u öğrenmek zorunda. ESXi'den gelen biri için öğrenme eğrisi makul.
