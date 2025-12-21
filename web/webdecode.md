# WebDecode

## Açıklama
Web sayfasının HTML kodunu inceleyerek Base64 ile encode edilmiş flag'i bulma ve decode etme challenge'ı.

## Çözüm

1. Challenge linkine gittim: https://play.picoctf.org/practice/challenge/427
2. Sayfada **About** butonuna tıkladım
3. "Try inspecting the page!! You might find it there" ipucunu gördüm
4. **Ctrl + U** ile sayfanın kaynak kodunu açtım
5. HTML kodları arasında şu satırı buldum:
```html
<section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9">
```
6. `notify_true` attribute'undaki değeri kopyaladım
7. https://www.base64decode.org/ sitesine giderek decode ettim
8. Flag ortaya çıktı

## Flag
`picoCTF{web_succ3ssfully_d3c0ded_f6f6b78a}`

## Öğrenilenler
- HTML attribute'larında gizli veri bulma
- Base64 encoding/decoding
- Kaynak kod inceleme
- Web forensics temelleri

## Kullanılan Araçlar
- Chrome
- Base64 Decode (Online)

## Zorluk
⭐ Easy - Beginner seviye
