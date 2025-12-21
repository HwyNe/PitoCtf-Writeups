# MyCrackme - 6941aea10992a052ab22239d

## Platform
crackmes.one

## Zorluk
⭐⭐ Medium

## Açıklama
Kullanıcıdan şifre isteyen basit bir crackme. Doğru şifre girildiğinde "Access Granted" mesajı veriyor.

## Kullanılan Araçlar
- Ghidra
- ChatGPT (Kod analizi için)

## Çözüm

### 1. Dosyayı İndirme
- crackmes.one'dan dosyayı indirdim
- Zip şifresi: `crackmes.one`

### 2. Ghidra ile Analiz
1. Ghidra'yı açtım
2. `File > New Project > Non Shared Project` ile yeni proje oluşturdum
3. `File > Import File` ile crackme dosyasını import ettim
4. "Would you like to analyze this file now?" sorusuna **Yes** dedim

### 3. Fonksiyonları Bulma
1. Arama çubuğuna `main` yazarak ana fonksiyonu buldum
2. Filter'a `x` yazarak x fonksiyonunu buldum
3. Filter'a `y` yazarak y fonksiyonunu buldum
4. Her iki fonksiyonun kodunu kopyaladım

### 4. Kod Analizi
Kopyaladığım fonksiyonları ChatGPT'ye gönderdim ve analiz ettirdim. Yapay zeka, fonksiyonların şifre kontrol mekanizmasını çözerek şifrenin **1337** olduğunu buldu.

### 5. Doğrulama
```bash
$ ./MyCrackme
Enter the password: 1337
Access Granted.
```

## Şifre/Key
`1337`

## Öğrenilenler
- Ghidra ile temel reverse engineering
- Fonksiyon analizi
- Decompiler çıktılarını okuma
- Şifre algoritması analizi

## Notlar
Bu challenge'da yapay zeka kullanarak kod analizi yapıldı. Manuel olarak da çözülebilir ancak AI desteği süreci hızlandırdı.
