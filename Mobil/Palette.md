# Palette - Mobil Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐ Orta

## Kategori
Mobile (Android)

## Puan
300 (+50 Firstblood)

## Challenge Dosyası
**📥 varit.apk**

## İpucu
"Hep jadx bakmayın istedim. apk tool da güzel. Renkleri daha güzel :)"

## Kullanılan Araçlar
- file (Dosya analizi)
- unzip (APK extraction)
- apktool (APK decode)
- Python/PIL (Resim analizi)
- Image viewer

## Çözüm

### 1. Dosya Analizi
APK dosyasını indirip kontrol ettim:

```bash
file varit.apk
```

**Çıktı:**
```
varit.apk: Android package (APK), with APK Signing Block
```

✅ **Android APK dosyası doğrulandı!**

### 2. APK İçeriğini Extract Etme
APK dosyası aslında bir ZIP arşivi, basit unzip ile açtım:

```bash
unzip varit.apk -d varit_extracted
cd varit_extracted
ls -la
```

**Çıktı:**
```
AndroidManifest.xml
classes.dex
classes2.dex
classes3.dex
classes4.dex
resources.arsc
res/
META-INF/
```

**Analiz:**
- 📄 AndroidManifest.xml: Uygulama yapılandırması (binary format)
- 🔢 classes.dex: Derlenmiş Java/Kotlin kodu
- 📊 resources.arsc: Uygulama kaynakları (binary format)
- 📁 res/: Resim, layout ve diğer kaynak dosyaları

### 3. Resim Dosyalarını İnceleme
Drawable klasöründeki resimleri listeledim:

```bash
find varit_extracted/res/drawable/ -type f -name "*.png"
```

**Önemli dosyalar:**
```
varit_extracted/res/drawable/palette.png    ← 🎯 Dikkat!
varit_extracted/res/drawable/header.png
varit_extracted/res/drawable/icon.png
varit_extracted/res/drawable/ctf_logo.png
```

🎨 **palette.png dosyası şüpheli görünüyor!**

### 4. Palette.png Analizi
Resmi görüntülemek için kopyaladım:

```bash
cp varit_extracted/res/drawable/palette.png .
xdg-open palette.png
# veya
eog palette.png
```

**Resimde ne var:**
- 📝 Üstte: "STMCTF{" yazısı
- 🎨 6 renkli kutu (2 sıra x 3 sütun)
- 📝 Sağ altta: "}" işareti

💡 Flag formatı gösteriliyor ama renk kodları ne?

### 5. Python ile Renk Analizi
Resimden renkleri çıkarmak için Python kullandım:

**analyze_palette.py:**
```python
from PIL import Image
import collections

# Palette.png'yi aç
img = Image.open('varit_extracted/res/drawable/palette.png')
img_rgb = img.convert('RGB')

# Tüm pikselleri al ve renkleri say
pixels = list(img_rgb.getdata())
color_counts = collections.Counter(pixels)

# En çok kullanılan renkleri göster
print("En çok kullanılan renkler:")
for i, (color, count) in enumerate(color_counts.most_common(10), 1):
    hex_color = '#{:02x}{:02x}{:02x}'.format(color[0], color[1], color[2])
    print(f"{i}. RGB{color} = {hex_color} - {count} piksel")
```

**Çıktı:**
```
En çok kullanılan renkler:
1. RGB(220, 219, 210) = #dcdbd2 - 1310189 piksel   ← Background
2. RGB(255, 255, 255) = #ffffff - 151252 piksel    ← Beyaz çerçeve
3. RGB(183, 183, 164) = #b7b7a4 - 33856 piksel     ← Renk 5
4. RGB(221, 190, 169) = #ddbea9 - 33672 piksel     ← Renk 2
5. RGB(165, 165, 141) = #a5a58d - 33672 piksel     ← Renk 4
6. RGB(107, 112, 92) = #6b705c - 33672 piksel      ← Renk 6
7. RGB(203, 153, 126) = #cb997e - 33489 piksel     ← Renk 1
8. RGB(255, 232, 214) = #ffe8d6 - 33489 piksel     ← Renk 3
```

### 6. Renk Kutularının Konumlarını Bulma
Renklerin resimde hangi sırada olduğunu bulmak için konumlarına baktım:

```python
# Renk kutularını konum bazlı bul
for y in range(0, height, 100):
    for x in range(0, width, 100):
        color = img_rgb.getpixel((x, y))
        # Background ve beyaz değilse
        if color not in [(220, 219, 210), (255, 255, 255)]:
            hex_color = '#{:02x}{:02x}{:02x}'.format(color[0], color[1], color[2])
            print(f"Konum ({x:4d}, {y:4d}): {hex_color}")
```

**Çıktı:**
```
Konum ( 200,  700): #cb997e
Konum ( 400,  700): #ddbea9
Konum ( 700,  700): #ffe8d6
Konum ( 200, 1000): #a5a58d
Konum ( 400, 1000): #b7b7a4
Konum ( 700, 1000): #6b705c
```

**Sıralı renkler (yukarıdan aşağıya, soldan sağa):**
1. #cb997e
2. #ddbea9
3. #ffe8d6
4. #a5a58d
5. #b7b7a4
6. #6b705c

### 7. İlk Flag Denemesi
Bulduğum renkleri flag formatına sokdum:

```
STMCTF{cb997e_ddbea9_ffe8d6_a5a58d_b7b7a4_6b705c}
```

**Deneme:**
```
❌ YANLIŞ!
```

🤔 Neden yanlış? İpucunu tekrar okudum: **"apk tool da güzel. Renkleri daha güzel"**

### 8. APKTool ile Decode (Doğru Yöntem)
İpucu bize APKTool kullanmamızı söylüyor:

```bash
apktool d varit.apk -o varit_decoded
```

💡 **APKTool binary XML dosyalarını okunabilir hale getirir!**

Decode edilen dosyaları inceledim:

```bash
cd varit_decoded
ls res/values/
```

**Çıktı:**
```
colors.xml      ← 🎯 Bingo!
strings.xml
styles.xml
```

### 9. colors.xml İnceleme
colors.xml dosyasını açtım:

```bash
cat res/values/colors.xml
```

**İlgili kısımlar:**
```xml
<color name="one">#ffcb997e</color>
<color name="two">#ffddbea9</color>
<color name="three">#ffffe8d6</color>
<color name="four">#ffa5a58d</color>
<color name="five">#ffb7b7a4</color>
<color name="six">#ff6b705c</color>
```

🚩 **EUREKA! Renkler #ff ile başlıyor!**

**Android renk formatı analizi:**
```
#AARRGGBB
 ││├─┴─ Blue (BB)
 │├──── Green (GG)
 ├───── Red (RR)
 └───── Alpha/Opacity (AA)

#ff = 255 (tam opak)
```

### 10. Flag Oluşturma
**Doğru renk kodları (alpha channel ile):**
```
one:   ffcb997e
two:   ffddbea9
three: ffffe8d6
four:  ffa5a58d
five:  ffb7b7a4
six:   ff6b705c
```

**Flag:**
```
STMCTF{ffcb997e_ffddbea9_ffffe8d6_ffa5a58d_ffb7b7a4_ff6b705c}
```

**Deneme:**
```
✅ DOĞRU!
```

## Flag
```
STMCTF{ffcb997e_ffddbea9_ffffe8d6_ffa5a58d_ffb7b7a4_ff6b705c}
```

## Çözüm Akışı
```
🎨 "Palette" Challenge
            ↓
📥 varit.apk indirildi
            ↓
📄 file varit.apk → Android APK
            ↓
📦 unzip → res/drawable/palette.png bulundu
            ↓
🖼️ palette.png → STMCTF{ + 6 renk + }
            ↓
🐍 Python/PIL → Renk kodları çıkarıldı
   #cb997e, #ddbea9, #ffe8d6, #a5a58d, #b7b7a4, #6b705c
            ↓
❌ STMCTF{cb997e_...} → YANLIŞ!
            ↓
💡 İpucu hatırlandı: "apk tool da güzel"
            ↓
🔧 apktool d varit.apk
            ↓
📝 res/values/colors.xml bulundu
            ↓
🎨 Android color format: #AARRGGBB
   one=#ffcb997e, two=#ffddbea9, ...
            ↓
🚩 FLAG: STMCTF{ffcb997e_ffddbea9_ffffe8d6_ffa5a58d_ffb7b7a4_ff6b705c}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file varit.apk

# APK extraction (basit yöntem)
unzip varit.apk -d varit_extracted

# Resim dosyalarını bulma
find varit_extracted/ -name "palette.png"

# Python ile renk analizi
python3 analyze_palette.py

# APKTool ile decode (doğru yöntem!)
apktool d varit.apk -o varit_decoded

# colors.xml inceleme
cat varit_decoded/res/values/colors.xml
```

## Teknik Detaylar

### Android Color Format
Android'de renkler **#AARRGGBB** formatında tanımlanır:

```
#AARRGGBB

AA = Alpha (transparency)
   00 = Tamamen şeffaf
   FF = Tamamen opak (255)

RR = Red (0-255)
GG = Green (0-255)
BB = Blue (0-255)
```

**Örnekler:**
```xml
#FF0000   = Kırmızı (opak değil, eski format)
#FFFF0000 = Kırmızı (opak, yeni format)
#80FF0000 = Yarı şeffaf kırmızı
#00FF0000 = Tamamen şeffaf kırmızı
```

### APKTool vs Unzip Farkı

**Unzip (basit extraction):**
- Binary XML dosyalarını çıkarır
- AndroidManifest.xml binary formatta
- colors.xml gibi resource'lar binary
- ❌ Okunabilir değil

**APKTool (decode):**
- Binary XML'i text'e çevirir
- AndroidManifest.xml okunabilir
- colors.xml düz metin olarak
- ✅ Okunabilir ve düzenlenebilir

### PIL/Pillow Kullanımı
```python
from PIL import Image

# Resmi aç
img = Image.open('palette.png')

# RGB moduna çevir
img_rgb = img.convert('RGB')

# Piksel okuma
color = img_rgb.getpixel((x, y))  # (R, G, B) tuple

# RGB to Hex
hex_color = '#{:02x}{:02x}{:02x}'.format(R, G, B)
```

## Öğrenilenler
- Android APK resource structure
- APKTool vs unzip farkı
- Android color format (#AARRGGBB)
- Python PIL/Pillow image processing
- colors.xml resource file
- Challenge ipuçlarını dikkatlice okuma

## Notlar
Bu challenge'ın güzel bir twist'i var! İlk bakışta resimden renkleri çıkarıp flag oluşturacağını düşünüyorsun, ama Android'in alpha channel kullandığını hesaba katman gerekiyor.

İpucu çok önemli: **"Hep jadx bakmayın istedim. apk tool da güzel."** - Bu direkt olarak APKTool kullanmamızı söylüyor. JADX Java kodu gösterir ama resource dosyalarını decode etmez.

Flag'in alpha channel içermesi (ff prefix) Android development bilgisi gerektiriyor.

## İpuçları
- Challenge ipuçlarını her zaman dikkatlice oku
- "palette" = renk paleti → colors.xml
- Android resource'ları binary format'ta
- APKTool XML decode için gerekli
- Alpha channel (AA) unutulmamalı
- RGB ≠ ARGB

## Python Script - Tam Versiyon
```python
from PIL import Image
import collections

def analyze_palette(image_path):
    """Palette.png'den renkleri çıkar"""
    img = Image.open(image_path)
    img_rgb = img.convert('RGB')
    width, height = img.size
    
    print(f"[*] Image size: {width}x{height}")
    
    # Tüm pikselleri say
    pixels = list(img_rgb.getdata())
    color_counts = collections.Counter(pixels)
    
    # En yaygın renkleri göster
    print("\n[*] Most common colors:")
    for i, (color, count) in enumerate(color_counts.most_common(10), 1):
        hex_color = '#{:02x}{:02x}{:02x}'.format(color[0], color[1], color[2])
        print(f"    {i}. {hex_color} - {count} pixels")
    
    # Renk kutularını bul
    print("\n[*] Color boxes (by position):")
    colors = []
    for y in range(0, height, 100):
        for x in range(0, width, 100):
            color = img_rgb.getpixel((x, y))
            # Background ve beyaz olmayan renkler
            if color not in [(220, 219, 210), (255, 255, 255)]:
                hex_color = '#{:02x}{:02x}{:02x}'.format(color[0], color[1], color[2])
                print(f"    Position ({x:4d}, {y:4d}): {hex_color}")
                colors.append(hex_color[1:])  # # işareti olmadan
    
    # Flag oluştur (alpha channel olmadan)
    flag_without_alpha = f"STMCTF{{'_'.join(colors)}}"
    print(f"\n[!] Flag without alpha: {flag_without_alpha}")
    print("[!] But this is WRONG! Check colors.xml for alpha channel!")
    
    return colors

if __name__ == "__main__":
    analyze_palette('varit_extracted/res/drawable/palette.png')
```

## Alternatif Çözüm Yöntemleri

### 1. Android Studio
```
1. Open Android Studio
2. Build → Analyze APK
3. Select varit.apk
4. Navigate to res/values/colors.xml
5. Read color values
```

### 2. Online APK Decompiler
```
1. Upload APK to online decompiler
2. Download decoded resources
3. Open colors.xml
4. Extract color values
```

### 3. Manual XML Parsing
```bash
# Extract resources.arsc
unzip varit.apk resources.arsc

# Use aapt tool
aapt dump resources varit.apk | grep color
```

### 4. JADX + Manual Search
```bash
jadx varit.apk -d output
grep -r "cb997e" output/
# Find where colors are defined
```

## Security Best Practices
Bu challenge'dan öğrenilen güvenlik prensipleri:
1. ❌ Hassas data resource dosyalarında
2. ❌ Plaintext color codes
3. ✅ Obfuscate resource names
4. ✅ Encrypt sensitive data
5. ✅ Use ProGuard/R8

## Challenge Tasarım Analizi
Bu challenge iyi tasarlanmış çünkü:
1. ✅ İpucu veriliyor ama direkt cevap değil
2. ✅ Multiple step solution
3. ✅ Tool knowledge test (APKTool)
4. ✅ Android platform knowledge (ARGB format)
5. ✅ Red herring (görsel renk analizi yanlış sonuç)

## colors.xml Örnek Yapısı
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- Material Design Colors -->
    <color name="colorPrimary">#FF6200EE</color>
    <color name="colorPrimaryDark">#FF3700B3</color>
    
    <!-- CTF Colors -->
    <color name="one">#ffcb997e</color>
    <color name="two">#ffddbea9</color>
    <color name="three">#ffffe8d6</color>
    <color name="four">#ffa5a58d</color>
    <color name="five">#ffb7b7a4</color>
    <color name="six">#ff6b705c</color>
</resources>
```
