# FakeSezar - Reverse Engineering Challenge

## Platform
CTF Yarışması

## Zorluk
⭐⭐⭐ Zor

## Kategori
Reverse Engineering

## Puan
500 (+50 Firstblood)

## Challenge Dosyası
**📥 Google Drive - FakeSezar**

## Kullanılan Araçlar
- file (Dosya analizi)
- strings (String extraction)
- Ghidra (Binary decompiler)
- Python 3 (Reverse script)
- Linux Terminal

## Çözüm

### 1. Dosya Analizi
Dosyayı indirip analiz ettim:

```bash
file FakeSezar
```

**Çıktı:**
```
FakeSezar: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), 
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
BuildID[sha1]=9298c7a7248b9a58de134714dd21f07a4002a6c5, 
for GNU/Linux 3.2.0, stripped
```

🔍 **Stripped binary** - Debug sembolleri yok, analiz zor olacak!

### 2. String Analizi
```bash
strings FakeSezar
```

**Önemli string'ler:**
```
Hmmm... Do you have something you want to tell me?
You win! The flag is: SKYDAYS{%s}
Welcome to the simple Caesar Cipher utility!
Enter the string to be processed: 
Enter the number of rotations (mod 26): 
Make sure you have entered a positive integer
The encrypted string is: '%s'
The decrypted string is: '%s'
Do you want to encrypt or decrypt?
 [1] Encrypt
 [2] Decrypt
BZZZT! You failed.
SKYDAYS25CTF
```

**Analiz:**
- ✅ Program Caesar cipher utility gibi görünüyor
- 🎯 "Hmmm..." → Gizli bir feature var!
- 🔑 "SKYDAYS25CTF" → Hedef string
- 🚩 "You win! The flag is: SKYDAYS{%s}" → Flag formatı

### 3. Program Testi
Programı çalıştırdım:

```bash
chmod +x FakeSezar
./FakeSezar
```

**Normal kullanım:**
```
Welcome to the simple Caesar Cipher utility!
Enter the string to be processed: test
Enter the number of rotations (mod 26): 13
Do you want to encrypt or decrypt?
 [1] Encrypt
 [2] Decrypt
1
The encrypted string is: 'grfg'
```

💡 Normal Caesar cipher çalışıyor, ama flag'i nasıl alacağız?

### 4. Gizli Feature Keşfi
Farklı rotation değerleri denedim:

```bash
./FakeSezar
```

**Input:**
```
String: test
Rotation: 99999999999999
```

**Çıktı:**
```
Hmmm... Do you have something you want to tell me?
```

🎯 **BULDUM!** Çok büyük rotation değerleri gizli feature'ı tetikliyor!

### 5. Ghidra ile Binary Analizi
Ghidra'yı açtım:

```bash
ghidra
```

**Adımlar:**
1. New Project → Non-Shared Project
2. File → Import File → FakeSezar
3. Analysis başlat
4. CodeBrowser aç
5. Functions listesinde `FUN_00101760` fonksiyonunu buldum

### 6. Decompiled Kod Analizi
**FUN_00101760 fonksiyonu (ana algoritma):**

```c
void FUN_00101760(char *param_1)
{
  // ... değişkenler ...
  
  sVar3 = strlen(param_1);
  if (9 < sVar3) {  // String 10+ karakter olmalı
    
    // Karakterler azalan sırada mı kontrol et
    do {
      if (*pcVar1 <= cVar6) {
        
        // GİZLİ FEATURE BAŞLIYOR!
        puts("Hmmm... Do you have something you want to tell me?");
        fgets(local_88,100,stdin);
        
        // Base64 decode
        FUN_001015b0(local_88,0x1040b0,0xc);
        
        // Shuffle işlemi
        lVar5 = 0;
        do {
          *(undefined1 *)((long)&local_94 + lVar5) = 
            (&DAT_001040b0)[(int)(&DAT_00104040)[lVar5]];
          lVar5 = lVar5 + 1;
        } while (lVar5 != 0xc);
        
        // XOR ve ADD işlemleri
        lVar5 = 0;
        do {
          (&DAT_001040b0)[lVar5] = 
            (&DAT_00104030)[lVar5] + (&DAT_001040b0)[lVar5] ^ 
            (&DAT_00104020)[lVar5];
          lVar5 = lVar5 + 1;
        } while (lVar5 != 0xc);
        
        // SKYDAYS25CTF ile karşılaştır
        iVar2 = strncmp(&DAT_001040b0,PTR_s_SKYDAYS25CTF_00104070,0xc);
        if (iVar2 == 0) {
          __printf_chk(1,"You win! The flag is: SKYDAYS{%s}\n",local_88);
        }
        else {
          puts("BZZZT! You failed.");
        }
        exit(0);
      }
    } while (...);
  }
}
```

**Algoritma:**
1. 📥 Input: Base64 encoded string (bizden isteniyor)
2. 🔓 Base64 decode: 12 byte'a decode ediliyor
3. 🔀 Shuffle: Index array ile karakterler karıştırılıyor
4. ➕ ADD: add_key ile toplanıyor
5. ⚡ XOR: xor_key ile XOR'lanıyor
6. ✅ Compare: Sonuç "SKYDAYS25CTF" olmalı

### 7. Data Array'leri Çıkarma
Ghidra'da adreslere gittim:

**DAT_00104040 (Index Array):**
```
00104040: 06 00 00 00 0a 00 00 00 04 00 00 00 07 00 00 00
00104050: 00 00 00 00 0b 00 00 00 02 00 00 00 09 00 00 00
00104060: 08 00 00 00 03 00 00 00 05 00 00 00 01 00 00 00
```
**Index array:** `[06, 0a, 04, 07, 00, 0b, 02, 09, 08, 03, 05, 01]`

**DAT_00104030 (ADD Key):**
```
00104030: 0f 02 07 0c 09 12 08 06 01 07 11 11
```
**ADD key:** `[0f, 02, 07, 0c, 09, 12, 08, 06, 01, 07, 11, 11]`

**DAT_00104020 (XOR Key):**
```
00104020: c1 b6 8d bd f9 e0 ac cd 58 f6 2c ca
```
**XOR key:** `[c1, b6, 8d, bd, f9, e0, ac, cd, 58, f6, 2c, ca]`

### 8. Python Reverse Script
**Algoritma reverse:**

```
Forward:
  result[i] = (add_key[i] + shuffled[i]) ^ xor_key[i]
  shuffled[i] = original[index_array[i]]

Reverse:
  shuffled[i] = (result[i] ^ xor_key[i]) - add_key[i]
  original[index_array[i]] = shuffled[i]
```

**solve.py:**
```python
import base64

target = "SKYDAYS25CTF"

# Arrays from Ghidra
index_array = [0x06, 0x0a, 0x04, 0x07, 0x00, 0x0b, 0x02, 0x09, 0x08, 0x03, 0x05, 0x01]
add_key = [0x0f, 0x02, 0x07, 0x0c, 0x09, 0x12, 0x08, 0x06, 0x01, 0x07, 0x11, 0x11]
xor_key = [0xc1, 0xb6, 0x8d, 0xbd, 0xf9, 0xe0, 0xac, 0xcd, 0x58, 0xf6, 0x2c, 0xca]

# Reverse: result[i] = (add_key[i] + shuffled[i]) ^ xor_key[i]
shuffled = []
for i in range(12):
    result_char = ord(target[i])
    # (add + shuffled) ^ xor = result
    # add + shuffled = result ^ xor
    # shuffled = (result ^ xor) - add
    temp = result_char ^ xor_key[i]
    shuffled_char = (temp - add_key[i]) % 256
    shuffled.append(shuffled_char)

# Reverse shuffle: shuffled[i] = original[index_array[i]]
original = [0] * 12
for i in range(12):
    original[index_array[i]] = shuffled[i]

# Base64 encode
original_bytes = bytes(original)
encoded = base64.b64encode(original_bytes).decode()

print(f"Base64 input: {encoded}")
```

### 9. Script Çalıştırma
```bash
python3 solve.py
```

**Çıktı:**
```
Base64 input: r3v3rs1ng+1s+fun
```

🎉 Mantıklı bir string! **"r3v3rs1ng+1s+fun"** = "reversing is fun"

### 10. Flag Alma
Programı çalıştırdım:

```bash
./FakeSezar
```

**Input:**
```
String: test
Rotation: 99999999999999
```

**Gizli feature tetiklendi:**
```
Hmmm... Do you have something you want to tell me?
r3v3rs1ng+1s+fun
```

**Çıktı:**
```
You win! The flag is: SKYDAYS{r3v3rs1ng+1s+fun}
```

🚩 **FLAG BULUNDU!**

## Flag
```
SKYDAYS{r3v3rs1ng+1s+fun}
```

## Çözüm Akışı
```
🔐 "FakeSezar" Challenge
            ↓
📥 ELF binary analiz
            ↓
🔤 strings → "SKYDAYS25CTF" bulundu
            ↓
🎮 Program test → Normal Caesar cipher
            ↓
🕵️ Büyük rotation → Gizli feature!
            ↓
🔧 Ghidra ile decompile
            ↓
📊 FUN_00101760 analizi:
   - Base64 decode
   - Shuffle (index array)
   - ADD operation
   - XOR operation
            ↓
🗂️ Data array'leri çıkar:
   - index_array [12 byte]
   - add_key [12 byte]
   - xor_key [12 byte]
            ↓
🐍 Python reverse script:
   result = (add + shuffled) ^ xor
   Reverse: shuffled = (result ^ xor) - add
            ↓
🔀 Shuffle reverse:
   original[index[i]] = shuffled[i]
            ↓
📝 Base64 encode → r3v3rs1ng+1s+fun
            ↓
🎯 Program'a input ver
            ↓
🚩 SKYDAYS{r3v3rs1ng+1s+fun}
```

## Kullanılan Komutlar
```bash
# Dosya analizi
file FakeSezar
strings FakeSezar

# Program testi
chmod +x FakeSezar
./FakeSezar

# Ghidra analizi
ghidra

# Python script
python3 solve.py
```

## Teknik Detaylar

### Algoritma Detayları
**Forward işlem (program içinde):**
```python
1. Base64 decode → 12 byte
2. Shuffle: shuffled[i] = original[index_array[i]]
3. Process: result[i] = (add_key[i] + shuffled[i]) ^ xor_key[i]
4. Compare: result == "SKYDAYS25CTF"
```

**Reverse işlem (bizim scriptimiz):**
```python
1. XOR reverse: temp = result[i] ^ xor_key[i]
2. ADD reverse: shuffled[i] = temp - add_key[i]
3. Shuffle reverse: original[index_array[i]] = shuffled[i]
4. Base64 encode → Input string
```

### Gizli Feature Tetikleme
- Normal Caesar cipher için: 0-25 arası rotation
- Gizli feature için: Çok büyük sayı (ör: 99999999999999)
- Bu büyük sayı, programın içindeki bir condition'ı tetikliyor

### Base64 Encoding
```python
original_bytes = bytes([r, 3, v, 3, r, s, 1, n, g, +, 1, s, +, f, u, n])
base64.b64encode(original_bytes) = "r3v3rs1ng+1s+fun"
```

## Öğrenilenler
- Ghidra ile stripped binary analizi
- Decompiled C kodu okuma ve anlama
- Multi-layer encoding (Base64 + XOR + ADD)
- Array manipulation ve shuffle algoritmaları
- Reverse engineering mantığı
- Hidden feature discovery teknikleri
- Ghidra'da data array'lerini bulma ve extract etme
- Python ile cryptographic operations reverse etme

## Notlar
Bu challenge, gerçek bir reverse engineering sorusunun güzel bir örneği. "Fake Caesar" adı çok yerinde - program Caesar cipher gibi görünüyor ama aslında Base64 + Shuffle + XOR + ADD kombinasyonu kullanıyor. 

Gizli feature'ı tetiklemek için büyük rotation değeri kullanma fikri yaratıcı. Bu, gerçek dünya malware analizinde de görülen bir teknik - programlar normal görünüp gizli fonksiyonlar içerebilir.

## İpuçları
- Stripped binary'lerde fonksiyon isimleri yok, mantığa odaklan
- Ghidra'nın decompiler'ı mükemmel değil, bazen kodu manuel yorumlamak gerek
- Data array'lerini bulmak için Ghidra'da adres referanslarını takip et
- Reverse engineering'de "forward" işlemi anla, sonra tersine çevir
- XOR ve ADD gibi işlemler matematiksel olarak tersine çevrilebilir
- Base64 her zaman kolayca encode/decode edilebilir
- Shuffle işlemlerinde index array'i kullan

## Alternatif Çözüm Yöntemleri
1. **Dynamic analysis**: GDB ile debug edip runtime'da değerleri görebilirsin
2. **ltrace/strace**: Library/system call'ları izleyerek algorithm flow'unu anlayabilirsin
3. **Angr**: Symbolic execution ile otomatik çözüm bulabilirsin
4. **Z3 solver**: Constraint'leri tanımlayıp otomatik çözdürebilirsin
