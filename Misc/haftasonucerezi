# Haftasonu Çerezi - Misc Challenge

## Açıklama
Bayrak sembolleri, hash kırma ve steganografi içeren çok adımlı bir Misc (Crypto + Stego) challenge. Amaç, görsel içindeki gizli flag’i ortaya çıkarmak.

## Çözüm

1. Challenge dosyası **Flags.jpg** Google Drive üzerinden indirildi.
2. Görsel incelendiğinde bayraklarla şifrelenmiş bir içerik olduğu fark edildi.
3. Yapılan araştırma sonucu kullanılan yöntemin **Maritime Signals Code (Deniz Sinyalleri)** olduğu anlaşıldı.
4. Bayraklar dCode.fr üzerinden tek tek decode edildi.
5. Decode işlemi sonucunda şu değer elde edildi:  
   **DD1951B5F76789461994B7AAF628452A**
6. Elde edilen değerin bir hash olduğu düşünüldü.
7. Hash, CrackStation üzerinden kırıldı.
8. Hash tipi **NTLM**, plain text değeri **p4ssw0rd123** olarak bulundu.
9. Bu şifrenin steghide için kullanıldığı tahmin edildi.
10. Steghide ile Flags.jpg dosyasından gizli içerik çıkarıldı.
11. Çıkarılan dosya okunarak flag elde edildi.

## Flag
`FLAG{haftasonucereziAfiyetOlsun}`

## Öğrenilenler
- Maritime Signals (Deniz Sinyalleri) şifreleme yöntemi  
- Hash türlerini ayırt etme  
- NTLM hash kırma  
- Steghide ile steganografi analizi  
- Misc challenge’larda zincirleme çözüm mantığı  

## Kullanılan Araçlar
- Google Drive  
- dCode.fr (Maritime Signals Decoder)  
- CrackStation (Hash Cracker)  
- Steghide  
- Linux terminal  

## Zorluk
⭐ Orta seviye

## Notlar
Challenge, Crypto + Stego tekniklerinin birlikte kullanıldığı öğretici bir Misc göreviydi. Doğru analiz sırası izlenmeden flag’e ulaşmak mümkün değil.
