# Mesaj - Crypto Challenge

## Platform
CTF Yarışması

## Zorluk
⭐ Kolay

## Kategori
Crypto

## Puan
300 (+50 Firstblood)

## Açıklama
Cihan hocanın size bir mesajı var:

```
FLAG{ 00110 10101 00110 10000 00011 10000 00110 10010 10010 00001 01010 10110 11000 01010 10000 00101 10011 00110 11010 11010 00001 01010 10010 00011 11001 00101 11000 01100 00101 00001 10010 00011 11100 00101 11000 01010 00111 00101 00111 01100 00011 00101 00111 00101 10010 00111 10110 00011 01010 00011 01100 10000 00001 10001 10010 00001 01010 10010 00001 01110 11000 01100 01101 00110 11010 11010 11000 01100 01001 00001 01010 }
```

**İpucu:** Flag içinde Selam sorusuna ipucu var.

## Kullanılan Araçlar
- dCode.fr (Cipher Identifier)
- dCode.fr (Baudot Code Decoder)
- Text editor

## Çözüm

### Adım 1: Binary Verisini İnceleme
Flag içeriğini temizledim:

```
00110 10101 00110 10000 00011 10000 00110 10010 10010 00001 01010 10110 11000 01010 10000 00101 10011 00110 11010 11010 00001 01010 10010 00011 11001 00101 11000 01100 00101 00001 10010 00011 11100 00101 11000 01010 00111 00101 00111 01100 00011 00101 00111 00101 10010 00111 10110 00011 01010 00011 01100 10000 00001 10001 10010 00001 01010 10010 00001 01110 11000 01100 01101 00110 11010 11010 11000 01100 01001 00001 01010
```

**Gözlemler:**
- 📊 Toplam 70 adet binary grup var
- 🔢 Her grup 5 bit uzunluğunda (örn: 00110, 10101)
- 🤔 Normal ASCII binary kodlaması 8 bit kullanır
- 💡 5 bitlik özel bir encoding sistemi olmalı!

**İlk 10 grubu analiz:**
```
00110 → 6  (decimal)
10101 → 21 (decimal)
00110 → 6  (decimal)
10000 → 16 (decimal)
00011 → 3  (decimal)
```

💭 5 bit binary, 0-31 arası sayıları temsil edebilir. Bu özel bir cipher olmalı!

### Adım 2: Cipher Identifier ile Analiz
dCode.fr Cipher Identifier kullandım:

**URL:** https://www.dcode.fr/cipher-identifier

Binary veriyi yapıştırdım:
```
00110 10101 00110 10000 00011 10000 00110 10010 10010 00001 01010 10110 11000 01010 10000 00101 10011 00110 11010 11010 00001 01010 10010 00011 11001 00101 11000 01100 00101 00001 10010 00011 11100 00101 11000 01010 00111 00101 00111 01100 00011 00101 00111 00101 10010 00111 10110 00011 01010 00011 01100 10000 00001 10001 10010 00001 01010 10010 00001 01110 11000 01100 01101 00110 11010 11010 11000 01100 01001 00001 01010
```

**Cipher Identifier sonucu:**
```
🎯 Detected: Baudot Code (98% confidence)
```

**Baudot Code nedir?**
- 📡 Telgraf sistemlerinde kullanılan 5-bitlik encoding
- 🔢 Her karakter 5 bit ile temsil edilir (0-31 arası)
- 📜 1870'lerde Émile Baudot tarafından geliştirildi
- 🌐 Modern teleprinter ve teletypewriter'ların temeli

✅ **Baudot Code tespit edildi! Şimdi decode edelim.**

### Adım 3: Baudot Code Decoder
dCode.fr Baudot Code Decoder'a gittim:

**URL:** https://www.dcode.fr/baudot-code

Binary veriyi "Decode" kısmına yapıştırdım ve "DECODE" butonuna tıkladım.

**🎉 Çıktı:**
```
IYITATILLERPORTSWIGGERLABSONSELAMSORUSUNASUSLUPARANTEZLERLECONFIGGONDER
```

**Decode başarılı! ✅**

### Adım 4: Mesajı Yorumlama
Decode edilen metni anlamlı parçalara ayırdım:

```
IYI TATILLER
PORT SWIGGER LAB
SON SELAM SORUSUNA
SUSLU PARANTEZLERLE
CONFIG GONDER
```

**Anlamı:**
- 🎄 İyi tatiller
- 🔬 PortSwigger Lab (Web Security Academy)
- 📝 Son "Selam" sorusuna
- {} Süslü parantezlerle
- ⚙️ CONFIG gönder

**💡 Bu başka bir challenge için ipucu!**
- "Selam" isimli bir challenge olmalı
- O challenge'da `{CONFIG}` veya `{SECRET_KEY}` gibi bir payload kullanılacak
- Muhtemelen template injection veya format string vulnerability

## Flag
```
FLAG{IYITATILLERPORTSWIGGERLABSONSELAMSORUSUNASUSLUPARANTEZLERLECONFIGGONDER}
```

## Çözüm Akışı
```
🔐 "Mesaj" Crypto Challenge
            ↓
📊 Flag içeriği analizi
   → 70 adet 5-bitlik binary grup
   → Normal binary 8 bit olur!
            ↓
🔎 dCode Cipher Identifier
   → https://www.dcode.fr/cipher-identifier
   → Binary veriyi yapıştır
            ↓
🎯 Baudot Code tespit edildi!
   → %98 confidence
   → 5-bit telgraf encoding sistemi
            ↓
🔓 Baudot Code Decoder
   → https://www.dcode.fr/baudot-code
   → Binary → Text decode
            ↓
📝 Decode sonucu:
   IYITATILLERPORTSWIGGERLABSONSELAMSORUSUNASUSLUPARANTEZLERLECONFIGGONDER
            ↓
🧩 Mesajı ayırma:
   → IYI TATILLER
   → PORT SWIGGER LAB
   → SON SELAM SORUSUNA
   → SUSLU PARANTEZLERLE
   → CONFIG GONDER
            ↓
💡 Selam challenge'ı için ipucu!
   → {CONFIG} veya {SECRET_KEY} payload kullanılacak
   → Format string / Template injection olabilir
            ↓
🏁 FLAG{IYITATILLERPORTSWIGGERLABSONSELAMSORUSUNASUSLUPARANTEZLERLECONFIGGONDER}
```

## Kullanılan Web Araçları
```
🔍 https://www.dcode.fr/cipher-identifier
🔓 https://www.dcode.fr/baudot-code
```

## Teknik Detaylar

### Baudot Code Nasıl Çalışır?
Baudot Code, 5 bitlik bir karakter encoding sistemidir:

**Özellikler:**
- 5 bit = 2^5 = 32 farklı kombinasyon
- Letters mode ve Figures mode olmak üzere iki mod
- Mod değiştirme için özel kontrol karakterleri
- ITA1 ve ITA2 (International Telegraph Alphabet) varyantları

**Örnek encoding:**
```
Letter 'A' → 00011
Letter 'E' → 00001
Letter 'I' → 00110
Letter 'O' → 11000
Letter 'U' → 00111
```

### Neden 5 Bit?
1. **Telgraf teknolojisi**: Erken telgraf sistemleri mekanik sınırlamalar nedeniyle 5 bit kullandı
2. **Yeterli karakter seti**: 32 kombinasyon, harfler ve özel karakterler için yeterli
3. **Mod switching**: Letters/Figures arasında geçiş yaparak daha fazla karakter

### Modern Kullanım
- Artık kullanılmıyor (ASCII ve UTF-8 tarafından değiştirildi)
- Tarihsel önem
- CTF'lerde eğitim amaçlı

## Öğrenilenler
- Baudot Code cipher sistemi
- 5-bit binary encoding
- dCode.fr cipher identifier kullanımı
- Telgraf şifreleme tarihi
- Multi-challenge ipucu sistemi
- Binary veri analizi

## Notlar
Bu challenge, iki farklı kavramı öğretiyor:
1. **Baudot Code**: Tarihsel bir encoding sistemi
2. **Challenge chaining**: Bir challenge'ın çıktısı diğer challenge için ipucu

"Selam sorusuna ipucu var" ifadesi çok önemli - bu başka bir challenge için hint veriyor. Decode edilen mesaj, başka bir challenge'da kullanılacak payload'ı açıklıyor.

## İpuçları
- 5-bit binary grupları görünce Baudot Code düşün
- dCode.fr'nin Cipher Identifier'ı çok güçlü bir araç
- Challenge açıklamalarını dikkatlice oku - ipuçları saklı olabilir
- Decode edilen mesaj her zaman direkt flag olmayabilir
- PortSwigger Lab reference'ı web security challenge'ı işaret ediyor

## Baudot Code Varyantları
1. **ITA1 (Baudot-Murray)**: Orijinal Baudot sistemi
2. **ITA2**: Geliştirilmiş versiyon, daha yaygın
3. **CCITT-2**: Uluslararası standart
4. **US TTY**: Amerikan teletypewriter standardı

## Benzer Encoding Sistemleri
1. **Morse Code**: Nokta ve çizgilerle encoding
2. **Bacon Cipher**: 5-bit binary, A/B ile temsil
3. **Gray Code**: Binary varyant, single-bit değişim
4. **Hamming Code**: Error detection ile binary

## Alternatif Çözüm Yöntemleri
1. **Manual decode**: Baudot Code tablosu ile elle decode
2. **Python script**: Custom Baudot decoder yaz
3. **CyberChef**: Baudot Code recipe'si kullan
4. **Online tools**: Başka Baudot decoder siteleri

## Python Decode Script Örneği
```python
# Baudot ITA2 lookup table (simplified)
baudot_ita2 = {
    '00011': 'A', '11001': 'B', '01110': 'C', '01001': 'D',
    '00001': 'E', '01101': 'F', '11010': 'G', '10100': 'H',
    '00110': 'I', '01011': 'J', '01111': 'K', '10010': 'L',
    '11100': 'M', '01100': 'N', '11000': 'O', '10110': 'P',
    '10111': 'Q', '01010': 'R', '00101': 'S', '10000': 'T',
    '00111': 'U', '11110': 'V', '10011': 'W', '11101': 'X',
    '10101': 'Y', '10001': 'Z'
}

binary_data = "00110 10101 00110 10000 00011 10000 ..."
groups = binary_data.split()

decoded = ""
for group in groups:
    if group in baudot_ita2:
        decoded += baudot_ita2[group]

print(f"Flag: FLAG{{{decoded}}}")
```

## PortSwigger Lab İpucu
Decode edilen mesajda "PORTSWIGGERLAB" ve "CONFIG" kelimelerinin geçmesi şunları işaret ediyor:
- Web security challenge olabilir
- Server-Side Template Injection (SSTI)
- `{{config}}` payload'ı kullanılmalı
- Flask/Jinja2 template engine
- Selam challenge'ında dene!
