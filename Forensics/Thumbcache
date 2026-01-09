# Thumbcache - Forensics Challenge

## Açıklama
Windows işletim sistemine ait thumbcache veritabanı içinden gömülü resimleri çıkararak flag’i bulmayı amaçlayan bir Forensics challenge.

## Çözüm

1. Google Drive üzerinden **thumbcache.db** dosyası indirildi.
2. Dosya tipi `file thumbcache.db` komutu ile kontrol edildi.
3. Hex dump analizi yapıldığında dosya içinde **CMMM** imzası görüldü ve bunun Windows Thumbcache dosyası olduğu anlaşıldı.
4. `strings` komutu ile ASCII ve Unicode string’ler çıkarıldı, doğrudan flag bulunamadı.
5. ASCII string’lerde **IHDR**, **sRGB**, **gAMA** ifadeleri görülerek gömülü PNG/JPEG dosyaları olduğu tespit edildi.
6. `binwalk thumbcache.db` komutu ile gömülü dosyalar tarandı.
7. 4 adet PNG ve 1 adet JPEG dosyası bulunduğu görüldü.
8. `foremost -i thumbcache.db -o extracted/` komutu ile gömülü dosyalar çıkarıldı.
9. PNG dosyalarının Windows klasör ikonları olduğu görüldü.
10. JPEG dosyası açıldığında flag görüntülendi.

## Flag
`flag{human_after_all}`

## Öğrenilenler
- Windows Thumbcache dosya yapısı  
- Dosya imzası (signature) analizi  
- Hex ve string analizi  
- Binwalk ve foremost ile dosya çıkarma  
- Forensics’te thumbnail önbelleklerinin önemi  

## Kullanılan Araçlar
- file  
- strings  
- xxd / hexdump  
- binwalk  
- foremost  
- eog / xdg-open  

## Zorluk
⭐ Kolay

## Notlar
Flag, thumbcache veritabanı içinde gömülü bir JPEG dosyasında yer almaktaydı.
