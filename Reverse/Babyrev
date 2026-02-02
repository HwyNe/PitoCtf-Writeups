# Babyrev - Reverse Engineering Challenge

## Platform
CTF Yarışması

## Zorluk
⭐ Kolay

## Kategori
Reverse Engineering

## Puan
200 (+50 Firstblood)

## Challenge Dosyası
**📥 Google Drive - Babyrev**

## Kullanılan Araçlar
- file (Dosya analizi)
- strings (String extraction)
- Linux Terminal
- chmod (İzin verme)

## Çözüm

### 1. Dosyayı İndirme ve Analiz
1. Dosyayı Google Drive'dan indirdim
2. Dosya tipini kontrol ettim:

```bash
file Babyrev
```

**Çıktı:**
```
Babyrev: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), 
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
BuildID[sha1]=0b4b6e2fc29b1a4d555524f0ee9a62cb5fdb3e60, 
for GNU/Linux 3.2.0, not stripped
```

**Analiz:**
- 🐧 ELF 64-bit: Linux executable
- 🔗 Dynamically linked: Harici kütüphaneler kullanıyor
- ✅ **Not stripped**: Debug sembolleri var → Kolay analiz!

### 2. String Analizi
Strings komutu ile binary içindeki tüm string'leri çıkardım:

```bash
strings Babyrev
```

**Önemli çıktılar:**
```
/lib64/ld-linux-x86-64.so.2
libc.so.6
puts
strcmp
Usage:
        ./easyrev <flag>
hctf{It_h4s_b33N_345Y}          # ← 🚩 FLAG!
You have found the flag!
Not the right one :(
```

🎯 **FLAG BULUNDU:** `hctf{It_h4s_b33N_345Y}`

### 3. Program Mantığı
Strings'ten program akışını anladım:

**Kullanım:**
```bash
./easyrev <flag>
```

**Program ne yapıyor:**
- 📥 Kullanıcıdan input alıyor
- 🔍 `strcmp()` ile kontrol ediyor
- ✅ Doğruysa: "You have found the flag!"
- ❌ Yanlışsa: "Not the right one :("

**Doğru flag:**
```
hctf{It_h4s_b33N_345Y}
```

### 4. Flag Doğrulama
Bulduğum flag'i test ettim:

```bash
chmod +x Babyrev
./Babyrev hctf{It_h4s_b33N_345Y}
```

**Çıktı:**
```
You have found the flag!
```

✅ **FLAG DOĞRULANDI!** Program doğru flag'i kabul etti!

## Flag
```
hctf{It_h4s_b33N_345Y}
```

## Çözüm Akışı
```
👶 "Babyrev" Challenge
            ↓
📥 Babyrev dosyası indirildi
            ↓
🔍 file Babyrev
   → ELF 64-bit, not stripped
            ↓
🔤 strings Babyrev
            ↓
📝 String'ler arasında tarama
            ↓
🎯 "hctf{It_h4s_b33N_345Y}" bulundu
            ↓
💡 strcmp() ile kontrol ediliyor
            ↓
🧪 Flag test edildi ve doğrulandı
            ↓
🚩 hctf{It_h4s_b33N_345Y}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file Babyrev

# String çıkarma
strings Babyrev

# (Opsiyonel) Çalıştırma ve test
chmod +x Babyrev
./Babyrev hctf{It_h4s_b33N_345Y}
```

## Teknik Detaylar

### "Not Stripped" Ne Demek?
- **Stripped**: Debug sembolleri kaldırılmış, analiz zor
- **Not Stripped**: Debug sembolleri var, fonksiyon isimleri okunabilir
- Bu challenge'da binary stripped değil → Kolay analiz!

### strcmp() Fonksiyonu
```c
int strcmp(const char *str1, const char *str2);
```
- İki string'i karşılaştırır
- Eşitse 0 döner
- Program bu fonksiyonla input'u kontrol ediyor

### String'ler Neden Binary'de?
- Programcı flag'i hardcoded olarak koymuş
- Compiler, string'leri binary'nin data section'ına koyar
- `strings` komutu bu section'ı okur

## Öğrenilenler
- Linux binary analizi temel komutları
- `file` komutu ile dosya tipi belirleme
- `strings` komutu ile string extraction
- ELF binary formatı temel yapısı
- "stripped" vs "not stripped" farkı
- strcmp() fonksiyonu çalışma mantığı
- Hardcoded string'lerin tehlikeleri

## Notlar
Bu "baby" seviyesinde bir reverse engineering challenge'ı. Flag direkt olarak binary içinde string olarak saklanmış, bu yüzden herhangi bir disassembler veya debugger kullanmaya gerek yok. Sadece `strings` komutu yeterli oluyor.

Gerçek dünya uygulamalarında, önemli veriler (şifreler, API key'ler, flag'ler) asla bu şekilde hardcoded olarak saklanmamalı. Bu challenge, güvenli kod yazmanın önemini de gösteriyor.

## İpuçları
- "Baby" veya "Easy" kategorisindeki challenge'larda önce basit yöntemleri dene
- Karmaşık araçlara geçmeden önce `strings` komutunu kullan
- "not stripped" binary'ler analiz için idealdir
- Flag formatına dikkat et (bu challenge'da `hctf{...}`)
- Test etmek her zaman iyi bir fikirdir (chmod +x ile çalıştırabilirsin)

## Alternatif Çözüm Yöntemleri
1. **Ghidra ile analiz**: main() fonksiyonunu decompile edip strcmp() çağrısını görebilirsin
2. **radare2**: `izz` komutu ile tüm string'leri listeleyebilirsin
3. **ltrace**: Program çalıştırılırken library call'ları izlenebilir
4. **hexdump**: Binary'yi hex formatında okuyup flag'i manuel arayabilirsin
