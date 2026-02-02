# 🗼 Kız Kulesi – Mobile Challenge

## Platform

CTF Yarışması

## Zorluk

⭐⭐ Orta

## Kategori

📱 Mobil (Reverse Engineering)

## Puan

500 Puan (+50 Firstblood)

## Challenge Dosyası

📥 **kizkulesi.apk** (Google Drive)

## Açıklama

Flagi emülatörde bulduğunuz şekilde aynen girmeniz gerekiyor. Özel bir format eklenmemiştir.

---

## 🛠️ Kullanılan Araçlar

* **apktool** – APK decompile
* **grep** – Smali ve XML içinde pattern arama
* **cat** – Dosya okuma
* **Python 3** – Script yazımı
* **PyCryptodome** – AES çözme
* **Linux Terminal** – Analiz ortamı

---

## 🔍 Çözüm

### 1. APK İndirme ve Dosya Kontrolü

```bash
wget "https://drive.google.com/uc?export=download&id=15XLNWR0yjFqcqeLWFzORll36NmVCANtr" -O kizkulesi.apk
file kizkulesi.apk
```

**Çıktı:**

```
Zip archive data
```

APK doğrulandı.

---

### 2. APK Decompile (apktool)

```bash
apktool d kizkulesi.apk -o kizkulesi_decompiled
cd kizkulesi_decompiled
```

**İçerik:**

* AndroidManifest.xml
* smali/
* smali_classes2/
* smali_classes3/
* res/

---

### 3. Encrypted String Analizi

```bash
cat res/values/strings.xml | grep -i encrypted
```

Bulunan Base64 stringler:

* encrypted_1
* encrypted_1_1
* encrypted_2
* encrypted_3

---

### 4. MainActivity.smali İncelemesi

```bash
cd smali_classes3/com/example/kizkulesi/
cat MainActivity.smali
```

**Önemli Bulgular:**

* `areYouAtKizKulesi()` metodu **her zaman false** döndürüyor
* Buna rağmen flag işleme bloğu çalışıyor

---

### 5. Flag İşleme Mantığı

Smali analizine göre:

* `encrypted_1` → 3x Base64 decode → `v0`
* `encrypted_2` → 5x Base64 decode → `v2`
* `encrypted_3` → 4x Base64 decode → `v3`
* `v6 = v0 + v2 + v3`

Sonrasında:

* AES/CBC/PKCS5Padding ile decrypt
* Key: `1234567890123456`
* IV: `0000000000000000`

---

### 6. Python Script ile Flag Çözme

```python
import base64
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

enc1 = "VG1rNWRVOUhSa3hpUm1oT1VrWm5ORnBHY0ZwaU1uaERVMFZ6ZUZWUlBUMD0="
enc2 = "Vm14a2QxVXhWblJVYmtwVFlXeGFiMVZyWkc5amJGcFhWbXR3YkZacmJEVmFWVlpYVlcxRmVGZHVXbGRXZWtaMVZGUkdkMDB4UWxWTlJEQTk="
enc3 = "VmtSQk5XRldiRFZrU0VKcllsWktlVll3WkZaa01WSnhVV3BhYTFaVmIzaFdWM1JxVUZFOVBRPT0="

key = b"1234567890123456"
iv = bytes(16)

def multi_decode(data, n):
    for _ in range(n):
        data = base64.b64decode(data).decode()
    return data

v0 = multi_decode(enc1, 3)
v2 = multi_decode(enc2, 5)
v3 = multi_decode(enc3, 4)
v6 = v0 + v2 + v3

cipher = AES.new(key, AES.MODE_CBC, iv)
flag_inner = unpad(cipher.decrypt(base64.b64decode(v6)), 16).decode()

flag = "FLAG={HM2023-" + flag_inner + "}"
print(flag)
```

---

## 🚩 Flag

```
FLAG={HM2023-561feccec11015b8be7ff470e15d5c1e}
```

---

## 📊 Çözüm Akışı

```
🗼 Kız Kulesi Challenge
        ↓
📱 APK indirildi
        ↓
🔓 apktool ile decompile
        ↓
🔍 strings.xml → encrypted veriler
        ↓
📂 MainActivity.smali analizi
        ↓
🔢 Base64 decode zinciri
        ↓
🔐 AES decrypt
        ↓
🚩 FLAG elde edildi
```

---

## 🧠 Öğrenilenler

* Smali kodu flag mantığını tamamen ortaya çıkarabilir
* Location/emulator kontrolleri bazen bilinçli olarak bypass edilir
* Çoklu Base64 + AES kombinasyonu mobil CTF’lerde yaygındır

---

## 💡 Notlar

* Emülatör şartı misleading amaçlı eklenmiş
* Statik analiz ile tamamen çözülebilir
* Hardcoded key ve IV ciddi güvenlik açığıdır
