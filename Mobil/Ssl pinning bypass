# Ssl pinning bypass - Mobil Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐ Orta

## Kategori
Mobile (Android)

## Puan
300 (+50 Firstblood)

## Challenge Dosyası
**📥 Google Drive - Sslpinnng.apk**

## Kullanılan Araçlar
- file (Dosya analizi)
- apktool (APK decompile)
- grep (Text arama)
- Python 3 (Key reconstruction & AES decryption)
- PyCryptodome (AES encryption library)

## Çözüm

### 1. Dosya Analizi
APK dosyasını indirip kontrol ettim:

```bash
file Sslpinnng.apk
```

**Çıktı:**
```
Sslpinnng.apk: Zip archive data, at least v2.0 to extract
```

✅ **Android APK dosyası doğrulandı!**

### 2. APK Decompile
Apktool kullanarak APK'yı decompile ettim:

```bash
apktool d Sslpinnng.apk -o Sslpinnng_decoded
cd Sslpinnng_decoded
```

**Çıktı:**
```
I: Using Apktool 2.7.0 on Sslpinnng.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...
```

**Dizin yapısı:**
```
AndroidManifest.xml
apktool.yml
res/
smali/
```

✅ **Decompile başarılı!**

### 3. MainActivity.smali Analizi
Ana sınıfı buldum:

```bash
find . -name "*MainActivity*"
```

**Sonuç:**
```
./smali/com/example/pinned/MainActivity.smali
./smali/com/example/pinned/MainActivity$a.smali
```

MainActivity.smali'yi inceledim:

```bash
cat ./smali/com/example/pinned/MainActivity.smali | head -100
```

**Önemli bulgular:**
- 🌐 **URL:** `https://pinned.com:443/pinned.php`
- 👤 **Username:** `bnavarro`
- 🔑 **Password:** `1234567890987654`
- 🔐 **Encrypted password (Base64):** `zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9a84hYDGfQn/gdiEvh`

### 4. Yardımcı Fonksiyonları Analiz Etme
`c/b/a/` klasöründeki fonksiyonların return değerlerini buldum:

```bash
for f in {a,b,c,d,e,f,h,i}; do
  tail -15 ./smali/c/b/a/$f.smali | grep -E "const|get|return"
done
```

**h.b() fonksiyonu için:**
```bash
grep -A 40 "method public static b()" ./smali/b/q/h.smali
```

Her fonksiyon bir ArrayList oluşturup belirli bir index döndürüyor:

| Fonksiyon | Return Index | Return Value |
|-----------|--------------|--------------|
| a.a()     | 1            | "fQn/gdiEvh" |
| b.a()     | 8            | "dEd0Xv"     |
| c.a()     | 5            | "98Cc1Z"     |
| d.a()     | 2            | "zlg4rj"     |
| e.a()     | 2            | "lw+sVs"     |
| f.a()     | 1            | "k3ED9a"     |
| h.a()     | 0            | "wel80q"     |
| h.b()     | 6            | "2jOu89"     |
| i.a()     | 9            | "1TPpCs"     |

### 5. AES Key Reconstruction
MainActivity.smali satır 524-710 arası key oluşturma algoritmasını analiz ettim:

```smali
invoke-static {}, Lb/q/h;->b()Ljava/lang/String;  # h.b() -> "2jOu89"
invoke-virtual {v9, v10}, Ljava/lang/String;->charAt(I)C  # charAt(3)

invoke-static {}, Lc/b/a/c;->a()Ljava/lang/String;  # c.a() -> "98Cc1Z"
invoke-virtual {v12, v11}, Ljava/lang/String;->charAt(I)C  # charAt(0)

# ... (devam ediyor)
```

**Python scripti ile key hesapladım:**

**calculate_aes_key.py:**
```python
#!/usr/bin/env python3

# String arrays
hb = "2jOu89"
ha = "wel80q"
ca = "98Cc1Z"
aa = "fQn/gdiEvh"
ia = "1TPpCs"
ba = "dEd0Xv"
ea = "lw+sVs"
da = "zlg4rj"

# Key construction
key = ""
key += hb[3]           # 'u'
key += hb[0]           # '2'
key += ca[0]           # '9'
key += aa[8].upper()   # 'V'
key += ha[1]           # 'e'
key += hb[0]           # '2'
key += ia[5].upper()   # 'S'
key += aa[7]           # 'E'
key += ba[4]           # 'X'
key += ea[4]           # 'V'
key += ea[4]           # 'V'
key += ia[5].upper()   # 'S'
key += da[3]           # '4'
key += da[5]           # 'j'
key += ha[1]           # 'e'
key += ha[1]           # 'e'

print(f"🔑 AES Key: {key}")
print(f"🔑 Key Length: {len(key)} characters")
```

**Çıktı:**
```
🔑 AES Key: u29Ve2SEXVVS4jee
🔑 Key Length: 16 characters
```

✅ **16 byte AES key başarıyla oluşturuldu!**

### 6. AES Decryption
Python decryption script yazdım:

**decrypt_aes.py:**
```python
#!/usr/bin/env python3
from Crypto.Cipher import AES
import base64

# AES Key
aes_key = "u29Ve2SEXVVS4jee"

# Encrypted password (Base64)
encrypted_b64 = "zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9a84hYDGfQn/gdiEvh"
encrypted_data = base64.b64decode(encrypted_b64)

print(f"[*] AES Key: {aes_key}")
print(f"[*] Encrypted length: {len(encrypted_data)} bytes")

# AES/ECB/PKCS5Padding (Java default)
cipher = AES.new(aes_key.encode('utf-8'), AES.MODE_ECB)
decrypted = cipher.decrypt(encrypted_data)

# Decrypted data
result = decrypted.decode('utf-8', errors='ignore')

print(f"\n{'='*70}")
print(f"🎉 DECRYPTED PASSWORD: {result}")
print(f"{'='*70}")
```

**Çalıştırdım:**
```bash
python3 decrypt_aes.py
```

**Çıktı:**
```
[*] AES Key: u29Ve2SEXVVS4jee
[*] Encrypted length: 48 bytes

======================================================================
🎉 DECRYPTED PASSWORD: HTB{trust_n0_1_n0t_3v3n_@_c3rt!}2eXv 
======================================================================
```

🎉 **FLAG BULUNDU!**

## Flag
```
HTB{trust_n0_1_n0t_3v3n_@_c3rt!}
```

## Çözüm Akışı
```
🔐 "SSL Pinning Bypass" Challenge
            ↓
📥 Sslpinnng.apk indirildi
            ↓
📄 file Sslpinnng.apk → Android APK
            ↓
🔧 apktool d Sslpinnng.apk
            ↓
📂 MainActivity.smali bulundu
            ↓
🔍 Encrypted password tespit edildi:
   zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9a84hYDGfQn/gdiEvh
            ↓
📊 c/b/a/ fonksiyonları analiz edildi
   → 9 farklı fonksiyon return değerleri bulundu
            ↓
🔑 AES key algoritması çözüldü
   → Smali charAt() çağrıları reverse edildi
            ↓
🧮 Key: u29Ve2SEXVVS4jee
            ↓
🔓 AES/ECB decrypt
   → PyCryptodome kullanıldı
            ↓
🎉 HTB{trust_n0_1_n0t_3v3n_@_c3rt!}
            ↓
🚩 FLAG: HTB{trust_n0_1_n0t_3v3n_@_c3rt!}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file Sslpinnng.apk

# APK decompile
apktool d Sslpinnng.apk -o Sslpinnng_decoded

# MainActivity analizi
cat ./smali/com/example/pinned/MainActivity.smali

# Fonksiyon return değerlerini bulma
for f in {a,b,c,d,e,f,h,i}; do
  tail -15 ./smali/c/b/a/$f.smali | grep -E "const|get|return"
done

# h.b() fonksiyonu
grep -A 40 "method public static b()" ./smali/b/q/h.smali

# Key reconstruction ve decryption
python3 calculate_aes_key.py
python3 decrypt_aes.py
```

## Teknik Detaylar

### AES Encryption Nedir?
**AES (Advanced Encryption Standard):**
- Symmetric encryption algoritması
- Block cipher (128-bit blocks)
- Key sizes: 128, 192, 256 bit
- Bu challenge'da: 128-bit (16 byte)

**Modes:**
- **ECB (Electronic Codebook)**: Bu challenge'da kullanılan, en basit mod
- CBC, CTR, GCM: Daha güvenli alternatifler

### Smali charAt() Analizi
```smali
# String objesi al
invoke-static {}, Lb/q/h;->b()Ljava/lang/String;
move-result-object v9

# charAt(index) çağır
const/4 v10, 0x3        # index = 3
invoke-virtual {v9, v10}, Ljava/lang/String;->charAt(I)C
move-result v11

# Sonucu StringBuilder'a ekle
invoke-virtual {v1, v11}, Ljava/lang/StringBuilder;->append(C)Ljava/lang/StringBuilder;
```

**Java equivalent:**
```java
String str = h.b();        // "2jOu89"
char c = str.charAt(3);    // 'u'
key.append(c);
```

### Key Construction Algorithm
```
h.b()[3] = '2jOu89'[3] = 'u'
h.b()[0] = '2jOu89'[0] = '2'
c.a()[0] = '98Cc1Z'[0] = '9'
a.a()[8].upper() = 'fQn/gdiEvh'[8].upper() = 'V'
h.a()[1] = 'wel80q'[1] = 'e'
...

Final Key: u29Ve2SEXVVS4jee
```

### PyCryptodome Kullanımı
```python
from Crypto.Cipher import AES

# AES cipher oluştur (ECB mode)
cipher = AES.new(key.encode('utf-8'), AES.MODE_ECB)

# Decrypt
plaintext = cipher.decrypt(ciphertext)
```

**Kurulum:**
```bash
pip install pycryptodome
```

## Öğrenilenler
- Android APK reverse engineering (advanced)
- Smali bytecode analizi
- AES encryption/decryption
- Dynamic key construction reverse engineering
- Base64 encoding
- Python cryptography
- SSL Pinning bypass concepts

## Notlar
Bu challenge, gerçek dünyada SSL Pinning bypass senaryolarını simüle ediyor. Challenge adı "SSL Pinning Bypass" olsa da, asıl zorluk AES key reconstruction ve decryption.

**SSL Pinning nedir?**
- Certificate pinning: Uygulamanın sadece belirli sertifikaları kabul etmesi
- MITM (Man-in-the-Middle) attack'lere karşı koruma
- Bypass: Frida, objection, apktool ile modifikasyon

Flag içeriği: "trust_n0_1_n0t_3v3n_@_c3rt!" (kimseye güvenme, sertifikaya bile!)

## İpuçları
- Smali kodunda string construction göründüğünde dikkatli ol
- charAt() çağrıları key/password construction'a işaret edebilir
- ArrayList return değerlerini manuel trace et
- AES Java'da default olarak ECB mode kullanır
- Base64 encoded string'ler encryption işareti
- Python script'le automation her zaman iyi fikir

## SSL Pinning Bypass Teknikleri

### 1. Apktool + Manual Patching
```bash
apktool d app.apk
# Remove pinning code from smali
apktool b app -o app_patched.apk
```

### 2. Frida (Runtime)
```javascript
Java.perform(function() {
    var CertificatePinner = Java.use("okhttp3.CertificatePinner");
    CertificatePinner.check.overload('java.lang.String', 'java.util.List').implementation = function() {
        console.log("SSL Pinning bypassed!");
        return;
    };
});
```

### 3. Objection
```bash
objection -g com.example.app explore
android sslpinning disable
```

### 4. Burp Suite + User Certificate
```
1. Install Burp CA certificate
2. Move to system certificates
3. Intercept traffic
```

## Alternatif Çözüm Yöntemleri

### 1. JADX ile Java Kodu
```bash
jadx Sslpinnng.apk -d output
# Java kodunu okumak daha kolay
```

**MainActivity.java:**
```java
String key = "" + h.b().charAt(3) + h.b().charAt(0) + ...;
AESCipher cipher = new AESCipher(key);
String decrypted = cipher.decrypt(encryptedPassword);
```

### 2. Dynamic Analysis (Frida)
```javascript
// Hook AES encryption function
Java.perform(function() {
    var MainActivity = Java.use("com.example.pinned.MainActivity");
    MainActivity.getDecryptedPassword.implementation = function() {
        var result = this.getDecryptedPassword();
        console.log("Decrypted: " + result);
        return result;
    };
});
```

### 3. Debugger (Android Studio)
```
1. Import APK as project
2. Set breakpoint at key construction
3. Run in debug mode
4. Inspect variables
```

## Key Reconstruction Manual Trace

**Smali code analysis:**
```
Line 524: invoke-static {}, Lb/q/h;->b()
→ h.b() = "2jOu89"

Line 527: const/4 v10, 0x3
→ index = 3

Line 528: invoke-virtual {v9, v10}, charAt
→ "2jOu89".charAt(3) = 'u'

...repeat for all 16 characters
```

## AES Decryption Details
```python
# Base64 decode
encrypted_bytes = base64.b64decode(encrypted_b64)
# 48 bytes

# AES decrypt (ECB mode, no IV needed)
cipher = AES.new(key, AES.MODE_ECB)
plaintext = cipher.decrypt(encrypted_bytes)
# PKCS5 padding removed automatically

# Result
HTB{trust_n0_1_n0t_3v3n_@_c3rt!} + padding
```

## Security Best Practices
Bu challenge'dan çıkarımlar:
1. ❌ Hardcoded AES keys
2. ❌ Predictable key construction
3. ❌ ECB mode (weak)
4. ❌ Client-side encryption alone
5. ✅ Use server-side encryption
6. ✅ Implement proper SSL pinning
7. ✅ Use CBC/GCM modes
8. ✅ Generate random keys
9. ✅ Use ProGuard/R8 obfuscation

## Flag Meaning
```
HTB{trust_n0_1_n0t_3v3n_@_c3rt!}

Translation:
"Trust no one, not even a certificate!"

Security lesson:
- Even SSL certificates can be compromised
- Defense in depth
- Multiple security layers
```
