# Cipher - Crypto Challenge

## Açıklama
Çok katmanlı şifreleme içeren bir Crypto challenge. Amaç, şifrelenmiş metni adım adım çözerek flag’i elde etmek.

## Çözüm

1. Google Drive üzerinden **Cipher.txt** dosyası indirildi.
2. Dosya içeriğinde uzun bir Base64 string olduğu görüldü.
3. Birinci katmanda Base64 decode işlemi yapıldı.
4. Elde edilen çıktı tekrar Base64 formatında olduğu için ikinci kez Base64 decode edildi.
5. İkinci decode sonrası hex formatında bir çıktı elde edildi.
6. Hex veri `xxd -r -p` ile çözüldü.
7. Ortaya çıkan veri tekrar Base64 formatında olduğu için üçüncü kez Base64 decode edildi.
8. Final çıktıda **key**, **iv** ve **cipher** bilgileri elde edildi.
9. IV kullanıldığı için block cipher algoritmaları denendi.
10. CyberChef üzerinde önce AES-CBC denendi ancak başarısız oldu.
11. DES-CBC ile decrypt işlemi yapıldığında şifre çözülerek anlamlı metin elde edildi.
12. Çözülen metin flag formatına dönüştürüldü.

## Flag
`Flag{dessifrelemeguvensizderlerdidogruymus}`

## Öğrenilenler
- Multi-layer encoding yapıları  
- Base64 ve hex çözümleme  
- Key, IV ve cipher ayrıştırma  
- Block cipher algoritmalarını ayırt etme  
- DES-CBC decryption  

## Kullanılan Araçlar
- Terminal (Base64, xxd)  
- CyberChef  
- Google Drive  

## Zorluk
⭐ Kolay

## Notlar
Bu challenge çok katmanlı şifreleme mantığını öğretmeye yönelik hazırlanmış bir Crypto göreviydi.
