# Rsa - Crypto Challenge

## Açıklama
RSA algoritması ile şifrelenmiş bir mesajın çözülmesini amaçlayan bir Crypto challenge. Amaç, verilen parametreleri kullanarak ciphertext’i decrypt edip flag’i elde etmek.

## Çözüm

1. Google Drive üzerinden **Rsa.txt** dosyası indirildi.
2. Dosya içinden RSA parametreleri alındı:
   - n (Modulus)
   - e (Exponent)
   - c (Ciphertext)
3. dCode üzerindeki RSA Cipher aracı açıldı.
4. “Decrypt RSA” bölümü seçildi.
5. n, e ve c değerleri ilgili alanlara girildi.
6. Decrypt işlemi çalıştırıldı.
7. Plaintext çıktısı elde edilerek flag okundu.

## Flag
`SiberVatan{hackerlar_verinin_arkeologlaridir}`

## Öğrenilenler
- RSA temel yapısı (n, e, c)  
- Online RSA çözüm araçlarının kullanımı  
- Public key ile şifrelenmiş verinin çözülmesi  

## Kullanılan Araçlar
- dCode RSA Cipher  
- FactorDB  
- RsaCtfTool  

## Zorluk
⭐ Kolay

## Notlar
Bu challenge, temel RSA çözüm mantığını kavramaya yönelik hazırlanmış basit bir Crypto göreviydi.
