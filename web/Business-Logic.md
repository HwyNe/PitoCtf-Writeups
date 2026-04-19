# Business Logic - Web Challenge

## Açıklama
Şifreye brute atmaya ve agresif saldırılara gerek olmayan, business logic hatası içeren bir Web challenge. Site GPT ile yazıldığı için bazı kontroller eksik bırakılmış.

## Çözüm

1. Challenge URL’si ziyaret edildi: http://64.226.74.243:5001
2. Site açıldığında giriş sayfası ve kayıt olma butonu görüldü.
3. Normal bir kullanıcı ile kayıt olundu:
   - Kullanıcı adı: yigit42
   - Şifre: 424242
4. Giriş yapıldıktan sonra dashboard sayfasına yönlendirildi.
5. Dashboard’da kullanıcının **Normal Kullanıcı** olduğu ve flag’e erişmek için **root veya robot** kullanıcısı olunması gerektiği mesajı görüldü.
6. Challenge açıklamasındaki ipuçlarına göre business logic zafiyeti olduğu anlaşıldı.
7. Kullanıcı adı doğrulamasının eksik olduğu ve case-insensitive kontrol yapıldığı tespit edildi.
8. Yeni bir hesap oluşturuldu:
   - Kullanıcı adı: RoBoT
   - Şifre: 123456
9. Bu kullanıcı ile giriş yapıldığında sistem tarafından **Admin** olarak tanındı.
10. Admin yetkisi ile dashboard üzerinden flag görüntülendi.

## Flag
`Flag{c4s3_1ns3ns1t1v3_us3rn4m3s_4r3_d4ng3r0us_goodby2025}`

## Öğrenilenler
- Business logic zafiyetleri
- Case-insensitive kullanıcı adı kontrolleri
- Reserved username doğrulamasının önemi
- Authentication bypass teknikleri

## Kullanılan Araçlar
- Web tarayıcısı
- Developer Tools
- Mantıksal analiz

## Zorluk
⭐ Kolay

## Notlar
Bu challenge özel olarak hazırlanmış bir görevdi ve herhangi bir CTF platformunda yayınlanmadı.
