# Cookie Monster Secret Recipe

## Açıklama
Kullanıcı girişi sonrası cookie'de saklanan bilgileri decode ederek flag'i bulma challenge'ı.

## Çözüm

1. Challenge linkine gittim
2. Username ve password girişi yaptım
3. **F12** ile Developer Tools'u açtım
4. **Application** sekmesine gittim
5. Sol menüden **Cookies** kısmını açtım
6. Cookie değerini kopyaladım
7. https://www.base64decode.org/ sitesine gittim
8. Cookie değerini yapıştırıp decode ettim
9. Flag ortaya çıktı

## Flag
`picoCTF{c00k1e_m0nster_l0ves_c00kies_731...}`

## Öğrenilenler
- Cookie'lerin tarayıcıda nasıl saklandığı
- Base64 encoding/decoding
- Developer Tools'da Application sekmesini kullanma
- Cookie manipulation temel mantığı

## Kullanılan Araçlar
- Chrome Developer Tools
- Base64 Decode (Online)

## Zorluk
⭐ Easy - Beginner seviye
