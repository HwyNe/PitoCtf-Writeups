# Kayıp Video - OSINT Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐ Orta

## Kategori
OSINT (Open Source Intelligence)

## Puan
300 (+50 Firstblood)

## Açıklama
Bu kanalda bir video vardı. Şimdi bulamıyorum.

**YouTube Kanalı:** Russian Top Secret Service

## Kullanılan Araçlar
- Wayback Machine (Archive.org)
- YouTube
- QR Scanner (qrscanner.net)
- Google Maps
- Screenshot tool

## Çözüm

### 1. İlk Keşif
1. Verilen YouTube kanalına gittim:
   ```
   https://www.youtube.com/@RussianTopSecretServiceUnOff
   ```
2. Kanal incelemesi:
   - ❌ Kanal boş görünüyor
   - ❌ Hiçbir video yok
   - 💡 Video silinmiş olabilir - Wayback Machine kullanmalıyım!

### 2. Wayback Machine Analizi
1. Archive.org sitesine gittim:
   ```
   https://web.archive.org/
   ```
2. Kanal URL'sini yapıştırdım:
   ```
   https://www.youtube.com/@RussianTopSecretServiceUnOff
   ```
3. Takvimi inceledim:
   - 📅 2023 yılı en son kayıtlar
   - 🗓️ **20 Mayıs 2023** tarihinde yoğun aktivite
   - 🔵 3 mavi nokta (normal snapshot)
   - 🟢 1 yeşil nokta (farklı snapshot) → **17:00:42**
   
4. 🎯 **Yeşil nokta = İçerikte değişiklik var!**

### 3. Snapshot Seçimi
1. 17:00:42 saatine tıkladım:
   ```
   https://web.archive.org/web/20230520170043/https://www.youtube.com/@RussianTopSecretServiceUnOff
   ```
2. Karşıma çıkan:
   - ✅ 1 video var!
   - ⏱️ Video süresi: 42 saniye
3. Videoyu açmak için sağ tık → `Open link in new tab`

### 4. Video Analizi
1. Videoyu dikkatle izlemeye başladım
2. Gözlemlerim:
   - 💬 Yorumlar bot gibi görünüyor
   - 🎥 Video içeriğini frame by frame inceliyorum
3. **30. saniyede:**
   - ⚡ 1 saniyelik QR kod görünüyor!
   - 🎯 QR kodu yakalamak için video durdurdum
   - Screenshot aldım
   - QR kod net görünüyor ✅

### 5. QR Kod Decode
1. QR Scanner sitesine gittim:
   ```
   https://qrscanner.net/tr
   ```
2. QR kod screenshot'unu yükledim
3. Çıkan link:
   ```
   https://goo.gl/maps/NThoxkDWDfB1fHEj6
   ```

### 6. Google Maps Lokasyonu
1. Linke tıklayıp açtım:
   ```
   https://goo.gl/maps/NThoxkDWDfB1fHEj6
   ```
2. Lokasyon bilgisi:
   ```
   📍 Russian Foreign Intelligence Service (SVR)
   🇷🇺 Rusya Dış İstihbarat Servisi
   ```
3. 💡 Challenge adı "Russian Top Secret Service" - Lokasyon bağlantısı mantıklı!

### 7. Yorumları İnceleme
1. Google Maps yorumlarını kontrol ettim
2. **DPWD PLAYZ** adlı kullanıcının yorumunu buldum:
   ```
   HACKME{PEŞİNDEYİZVESENİİYİTANIYORUZ}
   ```
3. 🚩 FLAG BULUNDU!

## Flag
```
HACKME{PEŞİNDEYİZVESENİİYİTANIYORUZ}
```

## Çözüm Akışı
```
📹 "Kayıp Video" Challenge
            ↓
🔍 YouTube kanalı boş
            ↓
🕰️ Wayback Machine'e git
            ↓
📅 2023 - Mayıs 20 analizi
            ↓
🟢 17:00:42 snapshot seçildi
            ↓
🎬 42 saniyelik video bulundu
            ↓
⏱️ 30. saniyede QR kod keşfedildi
            ↓
📱 QR Scanner ile decode
            ↓
🗺️ Google Maps linki
            ↓
📍 Russian Foreign Intelligence Service
            ↓
💬 DPWD PLAYZ yorumunda flag
            ↓
🚩 HACKME{PEŞİNDEYİZVESENİİYİTANIYORUZ}
```

## Kullanılan Siteler
```
🕰️ https://web.archive.org/
📱 https://qrscanner.net/tr
🗺️ https://goo.gl/maps/
📹 https://www.youtube.com/@RussianTopSecretServiceUnOff
```

## Öğrenilenler
- Wayback Machine kullanımı ve snapshot analizi
- Silinmiş içerikleri arşivden kurtarma
- Video frame analizi ve QR kod yakalama
- QR kod decode teknikleri
- Google Maps üzerinden bilgi toplama
- Snapshot tarih seçiminde yeşil/mavi nokta farkı
- Deleted content recovery teknikleri

## Notlar
Bu challenge, OSINT'te arşiv araçlarının ne kadar önemli olduğunu gösteriyor. Silinmiş bir video bile Wayback Machine sayesinde bulunabilir. QR kodun sadece 1 saniye göründüğü için dikkatli video analizi gerekliydi. Ayrıca Google Maps yorumlarında flag saklamak yaratıcı bir gizleme yöntemi.

## İpuçları
- Wayback Machine'de yeşil noktalar önemli değişiklikleri gösterir
- Video analizinde her saniye önemlidir, frame by frame izleyin
- QR kodlar genellikle kısa süre gösterilir, screenshot almaya hazır olun
- Google Maps yorumları da flag saklama yeri olabilir
