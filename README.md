# Çizgi CAD — GitHub + PWA Builder ile APK Kurulumu

## Klasördeki dosyalar
```
index.html                  (uygulamanın kendisi)
manifest.json                (PWA kimlik dosyası)
sw.js                        (offline çalışması için service worker)
icons/icon-192.png
icons/icon-512.png
icons/icon-512-maskable.png
```
Bu 3 dosya ve icons klasörü birbirine göre **aynı klasörde, aynı isimlerle** kalmalı — index.html içindeki bağlantılar (`manifest.json`, `sw.js`, `icons/...`) göreli (relative) yollarla yazıldı.

## 1) GitHub'a yükleme
1. GitHub'da yeni bir repo oluştur (örn. `cizgi-cad`), **Public** olmalı (GitHub Pages ücretsiz kullanım için).
2. Bu klasördeki tüm dosyaları (index.html, manifest.json, sw.js, icons/) repoya yükle — repo kökünde dursunlar, alt klasöre koyma.
3. Repo → **Settings → Pages** → "Build and deployment" → Source: **Deploy from a branch** → Branch: `main` / `/ (root)` → Save.
4. Birkaç dakika sonra GitHub sana bir adres verecek, örn:
   `https://kullaniciadin.github.io/cizgi-cad/`

Bu adrese tarayıcıdan girip uygulamanın açıldığını, mikrofonun ve kaydetmenin çalıştığını doğrula (artık https olduğu için mikrofon izni de kalıcı kalacak, önceki `file://` sorunundan farklı olarak).

## 2) PWA Builder ile APK üretme
1. https://www.pwabuilder.com adresine git.
2. Yukarıdaki GitHub Pages adresini (`https://kullaniciadin.github.io/cizgi-cad/`) kutuya yapıştır, "Start".
3. PWA Builder manifest ve service worker'ı otomatik tarayacak — "Manifest" ve "Service Worker" için yeşil tik görmen lazım.
4. "Package for stores" → **Android** seç.
5. Paket adı (`com.seninadin.cizgicad` gibi), sürüm no vs. formu doldur, "Generate" de.
6. İndirdiğin .zip içinde imzalı/imzasız APK (ve isteğe bağlı AAB) çıkacak. Test cihazına o APK'yı kurabilirsin.

## Notlar
- PDF dışa aktarma özelliği (jsPDF) internetten bir CDN script'i çekiyor (`cdnjs.cloudflare.com`) — APK içinde bu adrese erişim gerekecek, yani PDF dışa aktarmak için cihazın internete çıkması lazım. Çizim, kaydetme, CSV dışa aktarma tamamen offline çalışır (service worker sayesinde).
- Sesli ölçü girişi (mikrofon) Android WebView/PWA paketlerinde bazen kısıtlı olabilir; APK'da mikrofon çalışmazsa, PWA Builder'ın Android paket ayarlarında "Microphone" iznini elle eklemek gerekebilir (Android Studio ile paketi açıp AndroidManifest.xml'e `<uses-permission android:name="android.permission.RECORD_AUDIO"/>` eklenir).
- localStorage'da tuttuğun çizim verisi (`cizgi_cad_v14_data` anahtarı) tarayıcıya/cihaza özeldir; GitHub Pages'e geçince veya APK'ya geçince eski verin otomatik gelmez, sıfırdan başlarsın.
