# Runbook: Eski Router'a Dönüş

[🇬🇧 English](../../en/runbooks/fallback-to-legacy-router.md) · [← README](../../../README.tr.md)

**Ne zaman:** Sunucu, Proxmox ya da OPNsense VM'i arızalandı, kısa sürede düzelmeyecek ve ofisin hemen internete çıkması gerekiyor.
**Hedef süre:** 10 dakika.

## Önceden hazırlananlar

- [ ] Eski router **ayarları korunarak** adaptörüyle birlikte rafta duruyor.
- [ ] Yapılandırma yedeği NAS'ta.
- [ ] OPNsense'in LAN adresi eski router'ın LAN adresiyle **aynı**, istemcilerde değişiklik gerekmiyor.
- [ ] Bu sayfanın çıktısı rack'in yanında asılı.

## Adımlar

1. Sunucuyu kapatın ya da WAN ve LAN kablolarını sunucudan çıkarın.
2. Eski router'ı açın.
3. Kabloları takın:
   - ISP A → router port 1 (WAN1)
   - ISP B → router port 2 (WAN2)
   - Ofis switch'i → router port 3 (LAN)
4. Erişim noktasını geçici olarak ofis switch'ine takın. **Bu süre boyunca Wi-Fi erişim kontrolü devre dışıdır.** Not alın.
5. Bir bilgisayarda: DHCP'yi yenileyin, gateway'i kontrol edin, internete çıkışı doğrulayın.
6. NAS'a erişilebildiğini doğrulayın.
7. Geri dönüşü değişiklik günlüğüne yazın: tarih, saat, sebep.

## Geri dönüş modunda çalışmayanlar

- Wi-Fi beyaz listesi ve captive portal
- WireGuard VPN
- Üçüncü WAN hattı

## Normale dönüş

Sunucu sağlıklı olduğunda adımları tersine uygulayın, her WAN'ı ve Wi-Fi segmentini doğrulayın ve günlüğe kaydedin.
