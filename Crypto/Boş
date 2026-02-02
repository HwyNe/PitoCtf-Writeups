# Boş - Crypto Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐ Orta

## Kategori
Crypto / Steganography

## Puan
300 (+50 Firstblood)

## Challenge Dosyası
**📥 GitHub - bombos.docx**

## Kullanılan Araçlar
- file (Dosya analizi)
- unzip (Docx açma)
- Python 3 (XML parsing & decode)
- xml.etree (XML analiz)
- Text editor

## Çözüm

### 1. Dosyayı İndirme ve İlk Analiz
Dosya tipini kontrol ettim:

```bash
file bombos.docx
```

**Çıktı:**
```
bombos.docx: Microsoft Word 2007+
```

Word dosyasını açtığımda:
```
Boş
Not Visible
[Görünüşte boş sayfa]
```

🤔 **"Not Visible"** → Gizli bir şey var!

### 2. Docx Yapısını İnceleme
**Docx = ZIP arşivi!**

```bash
unzip -l bombos.docx
```

**Çıktı:**
```
Archive:  bombos.docx
  Length      Date    Time    Name
---------  ---------- -----   ----
     1322  1980-01-01 00:00   [Content_Types].xml
      593  1980-01-01 00:00   _rels/.rels
    18120  1980-01-01 00:00   word/document.xml  ← İçerik burada!
...
```

### 3. Document.xml İnceleme
İçeriği extract ettim:

```bash
unzip bombos.docx
cat word/document.xml
```

**XML içeriğinde pattern:**
```xml
<w:p><w:r><w:t xml:space="preserve">   </w:t></w:r><w:r><w:tab /></w:r>...</w:p>
<w:p><w:r><w:tab /></w:r></w:p>
<w:p><w:r><w:t xml:space="preserve">     </w:t></w:r><w:r><w:tab /></w:r>...</w:p>
```

💡 **Space ve Tab kombinasyonları!** Bu **Whitespace Steganography!**

### 4. Whitespace Pattern Çıkarma
**Python script ile extract:**

**extract_patterns.py:**
```python
import xml.etree.ElementTree as ET

tree = ET.parse('word/document.xml')
root = tree.getroot()

ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}

# Tüm paragrafları işle
for para in root.findall('.//w:p', ns):
    line = ""
    for elem in para.iter():
        if elem.tag.endswith('}t'):
            if elem.text:
                line += elem.text.replace(' ', 'S')
        elif elem.tag.endswith('}tab'):
            line += 'T'
    
    if line and line != 'T':  # Separator değilse
        print(line)
```

**Çıkan pattern'ler:**
```
SSSTSSSTTS
SSSSSTTSTTSS
SSSSSTTSSSST
SSSSSTTSSTTT
SSSSSTTTTSTT
...
```

### 5. Whitespace Decode
**Pattern analizi:**
- Her satır 5 Space (SSSSS) ile başlıyor → Padding
- Geri kalan kısım: Space (S) ve Tab (T) kombinasyonu
- Her pattern bir karakter encode ediyor!

**Decoding kuralı:**
- İlk 5 S'yi çıkar (padding)
- T = 1, S = 0 (binary)
- 7-bit ASCII olarak decode et

**decode_whitespace.py:**
```python
patterns = [
    "SSSTSSSTTS",
    "SSSSSTTSTTSS",
    "SSSSSTTSSSST",
    "SSSSSTTSSTTT",
    "SSSSSTTTTSTT",
    # ... tüm pattern'ler
]

decoded = ""
for pattern in patterns:
    # İlk padding'i çıkar
    if pattern.startswith("SSSSS"):
        data = pattern[5:]
    elif pattern.startswith("SSS"):
        data = pattern[3:]
    else:
        data = pattern
    
    # Binary'ye çevir (T=1, S=0)
    binary = data.replace('T', '1').replace('S', '0')
    
    # 7-bit ASCII
    binary = binary.ljust(7, '0')[:7]
    decoded += chr(int(binary, 2))

print(decoded)
```

**Çıktı:**
```
Flag{whitespaceNeredenOgrendinKral}
```

## Flag
```
Flag{whitespaceNeredenOgrendinKral}
```

## Çözüm Akışı
```
📄 Crypto Challenge: "Boş"
            ↓
📥 bombos.docx indirildi
            ↓
👁️ Görünür: "Boş" + "Not Visible"
            ↓
🔍 Docx = ZIP → Unzip
            ↓
📝 word/document.xml analiz
            ↓
🔤 Space + Tab pattern'leri bulundu
            ↓
💡 Whitespace Steganography!
            ↓
🔓 Pattern decode (T=1, S=0)
            ↓
📊 7-bit ASCII decode
            ↓
🚩 Flag{whitespaceNeredenOgrendinKral}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file bombos.docx
unzip -l bombos.docx

# İçerik çıkarma
unzip bombos.docx
cat word/document.xml

# Pattern extraction
python3 extract_patterns.py

# Decode
python3 decode_whitespace.py
```

## Teknik Detaylar

### Whitespace Steganography Nedir?
Whitespace steganography, görünmez karakterler (boşluk, tab, satır sonu) kullanarak veri gizleme tekniğidir.

**Çeşitleri:**
- **Space/Tab encoding**: S=0, T=1 veya tersi
- **SNOW**: Concealment program
- **Stegsnow**: Linux tool
- **Unicode spaces**: Farklı genişlikte boşluklar

### Docx Dosya Yapısı
```
docx dosyası
    ├── [Content_Types].xml
    ├── _rels/
    │   └── .rels
    ├── word/
    │   ├── document.xml      ← İçerik burada
    │   ├── styles.xml
    │   ├── settings.xml
    │   └── _rels/
    └── docProps/
```

### XML Namespace
```python
ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}
```
- Office Open XML standardı
- Word namespace: `w:`
- Text elementi: `<w:t>`
- Tab elementi: `<w:tab>`
- Paragraph: `<w:p>`

### Binary Encoding
```
Pattern: SSSTSSSTTS
Remove padding (SSSSS): TSSSTTS
Binary: 1000110
Decimal: 70
ASCII: 'F'
```

## Öğrenilenler
- Whitespace steganography teknikleri
- Docx dosya formatı (ZIP arşivi)
- XML parsing ile Python
- Office Open XML standardı
- Binary to ASCII conversion
- Steganography detection
- Hidden data extraction

## Notlar
Bu challenge, steganography'nin ilginç bir uygulaması. Whitespace karakterler normalde görünmez olduğu için, mesaj tamamen gizli kalıyor. "Boş" challenge adı çok yerinde - görünüşte boş ama aslında dolu!

Gerçek dünyada, whitespace steganography kod içine backdoor gizlemek veya copyright watermark eklemek için kullanılabilir.

## İpuçları
- Dosya adı genellikle ipucu verir ("Boş" → görünmez karakterler)
- "Not Visible" mesajı stenography işareti olabilir
- Docx her zaman ZIP olarak açılabilir
- XML parsing için `xml.etree` güvenli ve kolay
- Whitespace pattern'leri genellikle binary encoding kullanır
- 7-bit ASCII yaygın encoding formatı

## Benzer Steganography Teknikleri
1. **LSB Steganography**: Image'lerin least significant bit'lerinde veri
2. **Null Cipher**: Sadece belirli karakterleri oku
3. **Unicode Steganography**: Benzer görünen farklı Unicode karakterler
4. **Audio Steganography**: Ses dosyalarında gizli mesaj
5. **Network Steganography**: Paket header'larında veri

## Alternatif Çözüm Yöntemleri
1. **SNOW tool**: Whitespace steganography detector/decoder
2. **StegSnow**: Linux command-line tool
3. **Manual analysis**: Hex editor ile whitespace karakterleri bul
4. **Online tools**: Whitespace decoder web siteleri
5. **Regex parsing**: Pattern matching ile extract et

## Tam Çözüm Scripti
```python
import xml.etree.ElementTree as ET

# Extract patterns
tree = ET.parse('word/document.xml')
root = tree.getroot()
ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}

patterns = []
for para in root.findall('.//w:p', ns):
    line = ""
    for elem in para.iter():
        if elem.tag.endswith('}t'):
            if elem.text:
                line += elem.text.replace(' ', 'S')
        elif elem.tag.endswith('}tab'):
            line += 'T'
    
    if line and line != 'T':
        patterns.append(line)

# Decode patterns
decoded = ""
for pattern in patterns:
    # Remove padding
    if pattern.startswith("SSSSS"):
        data = pattern[5:]
    elif pattern.startswith("SSS"):
        data = pattern[3:]
    else:
        data = pattern
    
    # Binary conversion (T=1, S=0)
    binary = data.replace('T', '1').replace('S', '0')
    
    # 7-bit ASCII
    binary = binary.ljust(7, '0')[:7]
    decoded += chr(int(binary, 2))

print(f"Flag: {decoded}")
```

## Detection İpuçları
Bir dosyada whitespace steganography olup olmadığını anlamak için:
1. Dosya boyutunu kontrol et (beklenenden büyük mü?)
2. Hex editor'de whitespace pattern'leri ara
3. "Not visible", "Hidden", "Boş" gibi ipuçları
4. Normalde boş görünen alanlar
5. Beklenmedik whitespace karakterler
