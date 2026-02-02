# Byte - Reverse Engineering Challenge

## Platform
CTF Yarışması

## Zorluk
⭐ Kolay

## Kategori
Reverse Engineering

## Puan
200 (+50 Firstblood)

## Challenge Dosyası
**📥 Google Drive - chall.txt**

## Kullanılan Araçlar
- Python 3
- base64 (Python modülü)
- cryptography (Fernet)
- Text editor

## Çözüm

### 1. Dosyayı İndirme
1. Google Drive'dan `chall.txt` dosyasını indirdim
2. Dosya içeriği: Python bytecode disassembly çıktısı

### 2. Bytecode Analizi
Bytecode'u incelediğimde şu bilgileri çıkardım:

**Import edilen kütüphaneler:**
```python
import base64
from cryptography.fernet import Fernet
```

**Encrypted mesaj:**
```python
encMessage = b'gAAAAABj7Xd90ySo11DSFyX8t-9QIQvAPmU40mWQfpq856jFl1rpwvm1kyE1w23fyyAAd9riXt-JJA9v6BEcsq6LNroZTnjExjFur_tEp0OLJv0c_8BD3bg='
```

**Base64 encoded key:**
```python
encoded_key = b'7PXy9PSZmf/r5pXB79LW1cj/7JT6ltPEmfjk8sHljfr6x/LyyfjymNXR5Z0='
```

**XOR işlemi:**
```python
# Her byte 160 ile XOR'lanıyor
for k_b in key_bytes:
    key.append(k_b ^ 160)
```

### 3. Python Koduna Dönüştürme
Bytecode'u analiz ederek orijinal Python kodunu yeniden yazdım:

```python
import base64
from cryptography.fernet import Fernet

# Encrypted message
encMessage = b'gAAAAABj7Xd90ySo11DSFyX8t-9QIQvAPmU40mWQfpq856jFl1rpwvm1kyE1w23fyyAAd9riXt-JJA9v6BEcsq6LNroZTnjExjFur_tEp0OLJv0c_8BD3bg='

# Base64 decode the key
key_bytes = base64.b64decode(b'7PXy9PSZmf/r5pXB79LW1cj/7JT6ltPEmfjk8sHljfr6x/LyyfjymNXR5Z0=')

# XOR each byte with 160
key = []
for k_b in key_bytes:
    key.append(k_b ^ 160)

key = bytes(key)

# Decrypt with Fernet
fernet = Fernet(key)
decMessage = fernet.decrypt(encMessage).decode()

print(decMessage)
```

### 4. Çözüm Scripti
**solve.py:**
```python
import base64
from cryptography.fernet import Fernet

# Encrypted message
encMessage = b'gAAAAABj7Xd90ySo11DSFyX8t-9QIQvAPmU40mWQfpq856jFl1rpwvm1kyE1w23fyyAAd9riXt-JJA9v6BEcsq6LNroZTnjExjFur_tEp0OLJv0c_8BD3bg='

# Encoded key
encoded_key = b'7PXy9PSZmf/r5pXB79LW1cj/7JT6ltPEmfjk8sHljfr6x/LyyfjymNXR5Z0='

# Base64 decode
key_bytes = base64.b64decode(encoded_key)

# XOR with 160
key = []
for k_b in key_bytes:
    key.append(k_b ^ 160)

key = bytes(key)

print(f"Decoded Key: {key}")

# Decrypt
fernet = Fernet(key)
decMessage = fernet.decrypt(encMessage).decode()

print(f"\nFlag: {decMessage}")
```

### 5. Script Çalıştırma
**Kurulum:**
```bash
# Gerekli kütüphaneyi yükle
pip install cryptography
```

**Çalıştırma:**
```bash
python3 solve.py
```

**Çıktı:**
```
Decoded Key: b'LURTT99_KF5aOrvuh_L4Z6sd9XDRaE-ZZgRRiXR8uqE='

Flag: FLAG{FLY_L1k3_0xR4V3N}
```

## Flag
```
FLAG{FLY_L1k3_0xR4V3N}
```

## Çözüm Akışı
```
🔄 Bytecode Challenge: "Byte"
            ↓
📥 chall.txt indirildi
            ↓
🔍 Bytecode analiz edildi
            ↓
📝 Python koduna dönüştürüldü
            ↓
🔑 Key: Base64 decode + XOR 160
            ↓
🔓 Fernet ile decrypt
            ↓
🚩 FLAG{FLY_L1k3_0xR4V3N}
```

## Teknik Detaylar

### Şifreleme Akışı (Challenge tarafı)
```
1. Orijinal Key → Base64 Encode
2. Base64 Key → XOR 160 ile şifrele
3. Flag → Fernet ile şifrele
4. Bytecode olarak derle
```

### Decrypt Akışı (Bizim tarafımız)
```
1. Bytecode'u oku
2. Base64 Decode yap
3. XOR 160 ile decrypt et
4. Fernet key'i elde et
5. Encrypted message'ı decrypt et
6. Flag'i al
```

## Öğrenilenler
- Python bytecode okuma ve analizi
- Bytecode'dan Python koduna reverse engineering
- Base64 encoding/decoding
- XOR encryption/decryption
- Fernet symmetric encryption kullanımı
- Python disassembly çıktısını yorumlama

## Notlar
Bu challenge, Python bytecode'unun nasıl analiz edileceğini ve basit şifreleme algoritmalarının nasıl reverse edileceğini gösteriyor. XOR ile 160 değeri kullanılması ve Fernet encryption katmanı challenge'ı ilginç kılıyor. Bytecode analizi yapabilmek reverse engineering için önemli bir beceri.

## İpuçları
- Bytecode'da LOAD_CONST değerleri önemli veriyi gösterir
- XOR işlemleri genellikle basit obfuscation için kullanılır
- Fernet Python'da symmetric encryption için standart bir yöntemdir
- Base64 genellikle binary data'yı text formatına çevirmek için kullanılır
