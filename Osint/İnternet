# Internet - OSINT Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐ Orta

## Kategori
OSINT (Open Source Intelligence)

## Puan
300 (+50 Firstblood)

## Açıklama
Turuncu bir balık ve mavi bir kedinin internet hakkında konuştuklarını duydum. Anneleri zaman tünelinde şifresini paylaşmış. Şifreyi bulabilir misin?

## Flag Formatı
`Flag{şifre}`

## Kullanılan Araçlar
- Google Search
- YouTube
- Web Browser

## Çözüm

### 1. Karakter Analizi
Challenge açıklamasındaki ipuçlarını analiz ettim:
- 🐠 Turuncu balık → **Darwin**
- 🐱 Mavi kedi → **Gumball**
- 👩 Anneleri → **Nicole**
- 🕰️ Zaman tüneli → Video içeriğinde bir sahne

Bu karakterler "The Amazing World of Gumball" çizgi filminden!

### 2. Google Araştırması
1. Google'da `gumball darwin internet` araması yaptım
2. İki farklı sonuç geldi:
   - İnternet
   - İnternet Ağı

### 3. Doğru Videoyu Bulma
1. Her iki videonun açıklamalarını kontrol ettim
2. "İnternet Ağı" bölümünün açıklamasında şu yazıyordu:
   ```
   Gumball ve Darwin'i zor bir görev bekliyor. 
   Nicole'a bilgisayarı nasıl kullanması gerektiğini öğretiyorlar.
   ```
3. ✅ Bu doğru video! Nicole bilgisayar kullanmayı öğreniyor

### 4. Video İnceleme
1. Videoyu hızlandırarak izlemeye başladım
2. **07:30** saniyesinde kritik sahneyi buldum:
   - Nicole şifresini giriyor
   - Darwin: "Şifreni zaman tünelinde paylaştın" diyor
   - 🔐 Şifre ekranda görünüyor: `7eR3$@`

### 5. Flag Oluşturma
Bulduğum şifreyi flag formatına göre düzenledim.

## Flag
```
Flag{7eR3$@}
```

## Çözüm Akışı
```
🌐 "Internet" Challenge
        ↓
🎭 Karakter analizi
   (Gumball + Darwin + Nicole)
        ↓
🔎 Google: "gumball darwin internet"
        ↓
📺 2 sonuç bulundu
        ↓
📝 "İnternet Ağı" bölümünün açıklaması
   Nicole'un bilgisayar öğrendiğini gösterdi
        ↓
🎬 Video 07:30'da şifre girişi sahnesi
        ↓
🔐 Şifre: 7eR3$@
        ↓
🚩 Flag{7eR3$@}
```

## Öğrenilenler
- Popüler kültür referanslarını tanıma
- Video içeriği analizi
- Zaman damgası (timestamp) bazlı arama
- Açıklama metinlerinden ipucu çıkarma
- Çizgi film/medya tabanlı OSINT

## Notlar
Bu challenge, OSINT'in sadece teknik kaynaklar değil, aynı zamanda medya içerikleri üzerinden de yapılabileceğini gösteriyor. Video içeriklerini dikkatlice incelemek ve doğru zaman damgasını bulmak kritik öneme sahip.
