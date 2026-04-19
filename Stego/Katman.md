# Katman - Stego Challenge

## Açıklama
GIMP XCF formatındaki bir görsel dosyasında gizlenmiş flag’i, katmanları inceleyerek bulmayı amaçlayan bir Stego challenge.

## Çözüm

1. Google Drive üzerinden **KatmanKatman** dosyası indirildi.
2. Dosya tipini belirlemek için `file KatmanKatman` komutu çalıştırıldı.
3. Çıktıdan dosyanın **GIMP XCF image data** olduğu anlaşıldı.
4. GIMP kuruldu ve başlatıldı.
5. Dosya GIMP ile açıldı.
6. Sağ taraftaki **Layers** panelinde birden fazla katman olduğu görüldü.
7. Katmanların göz ikonları kullanılarak katmanlar tek tek kapatıldı.
8. 3. katmanda flag görüntülendi.

## Flag
`Flag{9a64bc4a390cb0ce31452820ee562c3F}`

## Öğrenilenler
- XCF dosya formatı  
- Dosya tipi tespiti  
- GIMP katman yapısı  
- Steganografi temel mantığı  

## Kullanılan Araçlar
- file  
- GIMP  

## Zorluk
⭐ Kolay

## Notlar
Flag, üst katmanların altında gizlenmişti ve yalnızca katmanlar ayrı ayrı incelendiğinde görülebiliyordu.
