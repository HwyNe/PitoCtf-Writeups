# 📱 Giriş Kapalı – Mobile Challenge

## Platform

CTF Yarışması

## Zorluk

⭐⭐ Orta

## Kategori

📱 Mobile (APK Reverse Engineering)

## Puan

300 Puan (+50 Firstblood)

## Challenge Dosyası

📥 **GirislerBurdan.apk** (Google Drive)

## Açıklama

Giriş yapabilirsen bir link bıraktım.

## Flag Formatı

`Flag{link}`

## İpuçları

* 🔐 Login ekranı mevcut
* 📝 Giriş bilgileri hardcoded
* 🔗 Başarılı girişten sonra link gösteriliyor

---

## 🛠️ Kullanılan Araçlar

* **file** – Dosya tipi doğrulama
* **unzip** – APK içeriğini çıkartma
* **jadx** – APK → Java decompile
* **cat** – Kaynak kod ve XML okuma
* **grep / find** – Dosya ve string arama
* **Linux Terminal** – Komut çalıştırma ortamı

---

## 🔍 Çözüm

### 1. APK İndirme ve İlk Analiz

APK dosyasının Android paketi olduğu doğrulandı.

```bash
file GirislerBurdan.apk
```

**Çıktı:**

```
Android package (APK), with APK Signing Block
```

**Analiz:**

* Dosya geçerli bir Android APK
* Reverse engineering için uygun

---

### 2. APK’yı Unzip Etme

APK içeriği çıkartıldı.

```bash
unzip GirislerBurdan.apk -d GirislerBurdan
```

**Önemli Dosyalar:**

* `AndroidManifest.xml`
* `classes.dex`
* `res/`

---

### 3. APK Decompile (JADX)

APK Java koduna dönüştürüldü.

```bash
jadx GirislerBurdan.apk -d GirislerBurdan_decompiled
```

**Bulunan Sınıflar:**

* `MainActivity.java`
* `UserPage.java`
* `AccountPage.java`
* `ProfilePage.java`

---

### 4. MainActivity Analizi

Login ekranı burada tanımlı.

```java
((MaterialButton) findViewById(R.id.loginbtn))
    .setOnClickListener(new a(this,
        (TextView) findViewById(R.id.username),
        (TextView) findViewById(R.id.password)));
```

**Analiz:**

* Username ve password alanları mevcut
* Login kontrolü `w0.a` sınıfında

---

### 5. Login Kontrolü – w0/a.java

Hardcoded giriş bilgileri tespit edildi.

```java
this.f3300a.getText().toString().equals("admin");
this.f3301b.getText().toString().equals("us0m14S3!!");
```

**Bulunan Bilgiler:**

* **Username:** `admin`
* **Password:** `us0m14S3!!`

Başarılı girişte `UserPage` açılıyor.

---

### 6. UserPage ve Buton Analizi

İki buton mevcut:

* Profile
* Account

Butonlar `w0.b` sınıfı tarafından yönetiliyor.

---

### 7. AccountPage & ProfilePage İncelemesi

Java kodlarında ek mantık yok.

**Sonuç:**

* Link XML layout dosyalarında aranmalı

---

### 8. Layout Dosyalarının Analizi

```bash
cd GirislerBurdan_decompiled/resources/res/layout/
cat activity_account_page.xml
```

**Bulunan Metin:**

```
Your account is Admin. If you want to flag, you can go to this link:
/bdc6a9d55a26ee383a9b5e7bf8e42c83.php
```

---

### 9. Flag Oluşturma

**Bulunan Link:**

```
/bdc6a9d55a26ee383a9b5e7bf8e42c83.php
```

**Flag:**

```
Flag{bdc6a9d55a26ee383a9b5e7bf8e42c83.php}
```

---

## 🚩 Flag

```
Flag{bdc6a9d55a26ee383a9b5e7bf8e42c83.php}
```

---

## 🧠 Öğrenilenler

* APK dosyaları zip formatındadır
* Hardcoded credential’lar Java kodunda kolayca bulunabilir
* XML layout dosyaları hassas veri içerebilir
* Mobile CTF’lerde statik analiz çoğu zaman yeterlidir

---

## 💡 Notlar

* Giriş bilgileri client-side kontrol ediliyor
* Bu yapı gerçek uygulamalarda ciddi güvenlik açığıdır
* Hassas bilgiler asla APK içine gömülmemelidir

---

## 🔁 Alternatif Çözüm Yöntemleri

### 1. Dynamic Analysis

* APK emülatörde çalıştırılarak giriş denenebilir

### 2. Smali Analizi

* `classes.dex` → smali dönüşümü ile manuel inceleme yapılabilir

---

## 📊 Çözüm Akışı

```
📱 Challenge Başlangıç
        ↓
📥 APK indirildi
        ↓
🔍 file → Android APK
        ↓
📦 unzip → içerik çıkarıldı
        ↓
🔨 jadx → Java kodu
        ↓
🔐 Hardcoded login bulundu
        ↓
📐 XML layout analizi
        ↓
🔗 Link bulundu
        ↓
🚩 FLAG elde edildi
```
