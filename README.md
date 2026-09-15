# SentinelBackup-Releases

BT Operasyon Kontrol Sistemi için **imzalı yayın paketleri** deposu. Kurulu sunucular otomatik güncellemeyi bu depodaki en son yayından alır:

```
https://api.github.com/repos/SentinelBt/SentinelBackup-Releases/releases/latest
```

Kaynak kod bu depoda **yoktur**. Burada yalnızca dağıtılmak üzere hazırlanmış paketler bulunur.

## Güncelleme zinciri

1. Sunucu açılışta ve 6 saatte bir bu depodaki son yayına bakar. Paketin imzası doğrulanırsa panelde **"Sunucu için yeni sürüm hazır"** uyarısı gösterilir.
2. Yönetici **Şimdi güncelle** dediğinde sunucu paketleri indirir, imzayı ve SHA-256 değerini yeniden doğrular. Server paketini sunucu makinesindeki Agent kurar.
3. Sunucu yeni sürümle açıldığında Client paketi tüm onaylı bilgisayarlara arka planda dağıtılır.
4. Mobil uygulama, sunucu o sürüme geçtikten sonra yeni APK'yı önerir. Kurulumu kullanıcı onaylar.

## Her yayında bulunan dosyalar

| Dosya | Açıklama |
|---|---|
| `BTOperasyonKontrolSistemiServerSetup-<sürüm>.exe` | Sunucu (yönetim paneli + Agent) kurulumu |
| `BTOperasyonKontrolSistemiClientSetup-<sürüm>.exe` | Client (kullanıcı bilgisayarı) kurulumu |
| `release-signature-server.json`, `release-signature-client.json` | RSA-SHA256 yayın imzaları (sürüm, build, SHA-256, boyut, dosya adı) |
| `BTOperasyonKontrolSistemiAndroid-<sürüm>.apk`, `release-android.json` | Mobil uygulama ve SHA-256/boyut bilgisi |
| `SHA256SUMS.txt` | Tüm dosyaların SHA-256 değerleri |
| `update-public-key.pem` | İmzaları doğrulayan açık anahtar (Agent'a gömülü anahtarla aynı) |

## Güvenlik

- Paketler, bu depodan bağımsız ve çevrimdışı tutulan bir özel anahtarla imzalanır. Özel anahtar hiçbir zaman buraya yüklenmez.
- Sunucular ve Agent'lar imzası, SHA-256 değeri ya da boyutu tutmayan paketi **reddeder**. Bu depoya yetkisiz bir dosya eklenmesi kurulum yaptırmaz.
- Sunucu paketleri yalnızca GitHub'ın HTTPS adreslerinden indirir.

## Yayın yapmak (yalnızca yetkili ekip)

Kaynak depoda paketler üretildikten sonra:

```powershell
powershell -ExecutionPolicy Bypass -File tools\publish-github-release.ps1
```

Script, yüklemeden önce `tools/verify-release-folder.mjs` ile tüm dosyaları doğrular. Doğrulama geçmezse hiçbir dosya yüklenmez.
