# Logcat - Mobil Challenge

## Platform
CTF Yarışması

## Zorluk
⭐ Kolay

## Kategori
Mobile (Android)

## Puan
200 (+50 Firstblood)

## Challenge Dosyası
**📥 Google Drive - Logcat.apk**

## Kullanılan Araçlar
- file (Dosya analizi)
- unzip (APK extraction)
- strings (String analizi)
- apktool (APK decompile)
- grep (Text arama)
- find (Dosya bulma)

## Çözüm

### 1. Dosya Analizi
APK dosyasını indirip kontrol ettim:

```bash
file Logcat.apk
```

**Çıktı:**
```
Logcat.apk: Android package (APK), with APK Signing Block
```

✅ **Android APK dosyası doğrulandı!**

### 2. APK İçeriğini İnceleme
APK dosyası aslında bir ZIP arşivi:

```bash
unzip Logcat.apk -d Logcat
cd Logcat
ls -la
```

**Çıktı:**
```
AndroidManifest.xml
classes.dex
classes2.dex
classes3.dex
resources.arsc
res/
META-INF/
```

**Analiz:**
- 📝 AndroidManifest.xml: Uygulama yapılandırması
- 🔢 classes.dex: Derlenmiş Java/Kotlin kodu
- 📊 resources.arsc: Uygulama kaynakları

### 3. String Analizi
Önce strings ile hızlı bakış:

```bash
strings Logcat.apk | grep -i skydays
```

**Çıktı:**
```
skydays2025
Base.Theme.Skydays2025
Theme.Skydays2025
```

Flag aramayı denedim:

```bash
strings Logcat.apk | grep -i "skydays{"
```

**Sonuç:**
```
(Boş - Flag görünmüyor)
```

💡 **Flag string'lerde değil, kod içinde gizli olabilir!**

### 4. Apktool ile Decompile
Apktool kullanarak APK'yı decompile ettim:

```bash
cd ~/Desktop
apktool d Logcat.apk -o Logcat_decoded
```

**Çıktı:**
```
I: Using Apktool 2.7.0-dirty on Logcat.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Baksmaling classes3.dex...
I: Baksmaling classes2.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...
```

✅ **Decompile başarılı!**

### 5. Smali Dosyalarını Bulma
Dizin yapısını inceledim:

```bash
cd Logcat_decoded
find . -name "*.smali" | head -20
```

**Önemli dosyalar:**
```
./smali_classes3/ytu/skydays2025/mobile2/MainActivity$1.smali
./smali_classes3/ytu/skydays2025/mobile2/MainActivity.smali
```

🎯 **MainActivity - Ana uygulama sınıfı!**

### 6. Flag Arama
Skydays ile ilgili tüm dosyalarda arama:

```bash
grep -r "skydays" .
```

Flag için özel arama:

```bash
grep -r "flag" . | grep -i sky
```

**En önemli sonuç:**
```
./smali_classes3/ytu/skydays2025/mobile2/MainActivity$1.smali:
    const-string v4, "Doğru pin, flag:SKYDAYS{logcatkullanin}"
```

🚩 **FLAG BULUNDU!**

### 7. MainActivity$1.smali İnceleme
Dosyayı açıp inceledim:

```bash
cat ./smali_classes3/ytu/skydays2025/mobile2/MainActivity\$1.smali | grep -A5 -B5 "SKYDAYS"
```

**İlgili kod bölümü:**
```smali
.method public onClick(Landroid/view/View;)V
    .locals 8
    
    # PIN kontrolü
    iget-object v0, p0, Lytu/skydays2025/mobile2/MainActivity$1;->val$pinInput:Landroid/widget/EditText;
    invoke-virtual {v0}, Landroid/widget/EditText;->getText()Landroid/text/Editable;
    
    # Database sorgusu
    iget-object v2, p0, Lytu/skydays2025/mobile2/MainActivity$1;->val$db:Landroid/database/sqlite/SQLiteDatabase;
    
    # Doğru PIN ise
    const-string v4, "Doğru pin, flag:SKYDAYS{logcatkullanin}"
    
    # TextView'e yazdır
    iget-object v3, p0, Lytu/skydays2025/mobile2/MainActivity$1;->val$resultText:Landroid/widget/TextView;
    invoke-virtual {v3, v4}, Landroid/widget/TextView;->setText(Ljava/lang/CharSequence;)V
.end method
```

**Algoritma:**
1. Kullanıcıdan PIN input alınıyor
2. Database'de kontrol ediliyor
3. Doğruysa flag gösteriliyor: **"Doğru pin, flag:SKYDAYS{logcatkullanin}"**

## Flag
```
SKYDAYS{logcatkullanin}
```

## Çözüm Akışı
```
📱 "Logcat" Challenge
            ↓
📥 Logcat.apk indirildi
            ↓
🔍 file Logcat.apk → Android APK
            ↓
📦 unzip → classes.dex bulundu
            ↓
🔤 strings → "skydays2025" görüldü
            ↓
🔧 apktool d Logcat.apk
            ↓
📂 Smali dosyaları decode edildi
            ↓
🔍 MainActivity$1.smali bulundu
            ↓
📝 grep -r "flag" . | grep -i sky
            ↓
🎯 const-string "Doğru pin, flag:SKYDAYS{logcatkullanin}"
            ↓
🚩 SKYDAYS{logcatkullanin}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file Logcat.apk

# APK extraction
unzip Logcat.apk -d Logcat

# String analizi
strings Logcat.apk | grep -i skydays

# Apktool decompile
apktool d Logcat.apk -o Logcat_decoded

# Smali dosyalarını bulma
find . -name "*.smali"

# Flag arama
grep -r "flag" . | grep -i sky
```

## Teknik Detaylar

### APK Dosya Yapısı
```
Logcat.apk (ZIP arşivi)
    ├── AndroidManifest.xml     # Uygulama yapılandırması
    ├── classes.dex             # Derlenmiş Dalvik bytecode
    ├── classes2.dex            # Ek sınıflar
    ├── classes3.dex            # Ek sınıflar
    ├── resources.arsc          # Derlenmiş kaynaklar
    ├── res/                    # Uygulama kaynakları
    │   ├── drawable/           # Görseller
    │   ├── layout/             # XML layout'lar
    │   └── values/             # String, color vb.
    └── META-INF/               # İmza bilgileri
        ├── MANIFEST.MF
        ├── CERT.SF
        └── CERT.RSA
```

### DEX vs Smali
- **DEX (Dalvik Executable)**: Android'in derlenmiş bytecode formatı
- **Smali**: DEX'in human-readable assembly formatı
- **Apktool**: DEX → Smali dönüşümü yapar

### Smali Syntax Örneği
```smali
# String tanımlama
const-string v4, "Doğru pin, flag:SKYDAYS{logcatkullanin}"

# Method çağrısı
invoke-virtual {v3, v4}, Landroid/widget/TextView;->setText(Ljava/lang/CharSequence;)V

# Object field erişimi
iget-object v0, p0, Lytu/skydays2025/mobile2/MainActivity$1;->val$pinInput:Landroid/widget/EditText;
```

### MainActivity$1 Nedir?
- `MainActivity$1`: Inner class (anonim sınıf)
- Genellikle event listener'lar için kullanılır
- Bu durumda: Button onClick listener

## Öğrenilenler
- Android APK reverse engineering
- Apktool kullanımı
- Smali bytecode okuma
- APK dosya yapısı
- DEX decompilation
- grep ile etkili arama
- Android app architecture

## Notlar
Bu challenge, Android mobile reverse engineering'e güzel bir giriş. Challenge adı "Logcat" olsa da, aslında flag logcat'te değil, direkt olarak uygulamanın smali kodunda hardcoded. 

Gerçek dünyada, hassas bilgiler (flag'ler, API key'ler, şifreler) asla bu şekilde hardcoded olarak saklanmamalı. Bu challenge, güvenli Android development'ın önemini gösteriyor.

## İpuçları
- APK her zaman ZIP arşivi olarak açılabilir
- strings komutu hızlı bir başlangıçtır ama her zaman yeterli olmaz
- Apktool, Android reverse engineering için temel araç
- MainActivity ve inner class'lar önemli flag lokasyonları
- grep ile recursive arama çok etkili
- Smali okumak zor görünse de temel syntax'ı öğrenmek kolay

## Apktool Kurulumu
```bash
# Linux (Debian/Ubuntu)
sudo apt install apktool

# macOS (Homebrew)
brew install apktool

# Manual
wget https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool
wget https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_2.7.0.jar
chmod +x apktool
sudo mv apktool apktool_2.7.0.jar /usr/local/bin/
```

## Alternatif Çözüm Yöntemleri
1. **JADX**: DEX → Java source code decompiler
   ```bash
   jadx Logcat.apk -d output
   # Java kodunu okumak daha kolay
   ```

2. **JEB Decompiler**: Professional Android reverse engineering tool

3. **Frida**: Dynamic instrumentation
   ```bash
   # Runtime'da değerleri okuyabilirsin
   frida -U -f ytu.skydays2025.mobile2
   ```

4. **Android Studio**: APK Analyzer
   ```
   Build → Analyze APK → Logcat.apk
   ```

5. **dex2jar + JD-GUI**: DEX → JAR → Java source
   ```bash
   d2j-dex2jar Logcat.apk
   jd-gui Logcat-dex2jar.jar
   ```

## JADX Kullanımı (Alternatif)
```bash
# JADX kurulumu
sudo apt install jadx

# Decompile
jadx Logcat.apk -d Logcat_jadx

# Java kodunu görüntüle
cd Logcat_jadx/sources/ytu/skydays2025/mobile2
cat MainActivity.java
```

JADX ile direkt Java kodu görebilirsin:
```java
public void onClick(View view) {
    String pin = pinInput.getText().toString();
    // Database query...
    if (pinCorrect) {
        resultText.setText("Doğru pin, flag:SKYDAYS{logcatkullanin}");
    }
}
```

## Logcat Nedir?
Challenge adı "Logcat" - Android'in logging sistemi:
```bash
# Gerçek cihazda/emülatörde logcat kullanımı
adb logcat

# Flag için filtreleme
adb logcat | grep -i flag
```

Not: Bu challenge'da flag logcat'te değil, ama real-world scenario'larda developer'lar bazen log'lara sensitive data yazar - bu bir security vulnerability'dir!

## Security Best Practices
Bu challenge'dan çıkarımlar:
1. ❌ Hardcoded secrets kullanma
2. ❌ Flag/password/API key kaynak kodda
3. ❌ Log'lara sensitive data yazma
4. ✅ ProGuard/R8 ile code obfuscation
5. ✅ Encryption kullan
6. ✅ Server-side validation
7. ✅ Certificate pinning
