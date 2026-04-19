# Sifrelenmis

Sifrelenmis - Reverse Engineering Challenge
Reverse Difficulty AES

📋 Soru Bilgileri
Kategori: Reverse Engineering
Seviye: Orta
Puan: 300

Challenge Dosyası: 📥 Google Drive - Sifrelenmis

Firstblood: +50 puan

🔍 Analiz
Hedef: Linux binary'sini analiz edip AES şifrelemesini çözmek

Strateji: File analiz → String extraction → Key bulma → AES decrypt

✅ Çözüm Adımları
📥 1. Dosyayı İndirme ve Analiz
file Sifrelenmis
Çıktı:

Sifrelenmis: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), 
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
for GNU/Linux 3.2.0, stripped
🔍 64-bit Linux executable, stripped (debug sembolleri yok)

🔍 2. String Analizi
strings Sifrelenmis
Önemli string'ler:

ifreyi giriniz: 
%19s
NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0=
AES olabilir mi? - CBC - PKCS5Padding - No IV
Tekrar deneyin.
a73e7606
977366e5
128bithe
Çıkarımlar:

🔐 Encrypted flag: NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0= (Base64)
🔑 Key ipuçları: a73e7606, 977366e5, 128bithe
📝 Şifreleme: AES-128-CBC, No IV (IV = zeros)
📊 3. Rodata Section İnceleme
readelf -p .rodata Sifrelenmis
Çıktı:

String dump of section '.rodata':
  [     a]  ifreyi giriniz: 
  [    1b]  %19s
  [    20]  NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0=
  [    50]  AES olabilir mi? - CBC - PKCS5Padding - No IV
  [    7e]  Tekrar deneyin.
🔑 4. Key Oluşturma
String'lerden key parçalarını birleştiriyoruz:

a73e7606 + 977366e5 + 128bithe
16 karakter (128-bit AES):

a73e7606977366e5128bithe
İlk 16 karakteri alıyoruz:

Key: a73e760697736
Tam key (16 byte):

a73e7606977366e5128bithe → "a73e76069773" + "66e5128bithe" 
Ancak daha düzgün: a73e7606977366e5128bithe (16 karakter)
🔓 5. AES Decryption
Parametreler:

Algorithm: AES-128-CBC
Key: a73e7606977366e5128bithe (16 bytes)
IV: 00000000000000000000000000000000 (16 bytes - No IV)
Encrypted: NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0=
OpenSSL ile decrypt:

# Key'i hex'e çevir
echo -n "a73e7606977366e5128bithe" | xxd -p
# Çıktı: 613733653736303639373733363665353132386269746865

# Decrypt
echo "NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0=" | \
base64 -d | \
openssl enc -d -aes-128-cbc \
  -K 613733653736303639373733363665353132386269746865 \
  -iv 00000000000000000000000000000000
Çıktı:

SKYDAYS25{gercektenkolay}
🚩 FLAG
SKYDAYS25{gercektenkolay}
🛠️ Kullanılan Araçlar
🔍
file
Dosya tipi analizi	📝
strings
String extraction	🔧
readelf
ELF analizi	🔐
openssl
AES decryption
Kullanılan Komutlar:

# Dosya analizi
file Sifrelenmis
strings Sifrelenmis
readelf -p .rodata Sifrelenmis

# Key'i hex'e çevirme
echo -n "a73e7606977366e5128bithe" | xxd -p

# AES Decryption
echo "NnSInu4+QXY4gfT46i92ZBFsrZUZPYKsracgmVuxFT0=" | \
  base64 -d | \
  openssl enc -d -aes-128-cbc \
    -K 613733653736303639373733363665353132386269746865 \
    -iv 00000000000000000000000000000000
💭 Çözüm Akışı
🔐 Reverse Challenge: "Sifrelenmis"
            ↓
📥 ELF binary indirildi
            ↓
🔍 strings komutu ile analiz
            ↓
📝 Key parçaları bulundu:
   - a73e7606
   - 977366e5
   - 128bithe
            ↓
🔑 Key birleştirildi (16 byte)
            ↓
📊 Encrypted data: Base64 string
            ↓
💡 AES-128-CBC, IV=zeros
            ↓
🔓 OpenSSL ile decrypt
            ↓
🚩 SKYDAYS25{gercektenkolay}
