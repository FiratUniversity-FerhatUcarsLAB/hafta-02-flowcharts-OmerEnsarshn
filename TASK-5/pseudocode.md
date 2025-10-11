BAŞLA
Sistem durumunu kontrol et
Eğer sistem aktif değilse:
    Bekle belirli süre
    Sistemi tekrar kontrol et

Sürekli sensör döngüsü başlat:
    Hareket sensörünü oku
    Kapı/Pencere sensörünü oku

    Eğer hareket veya kapı/pencere ihlali tespit edildiyse:
        Eğer ev sahibi evdeyse:
            Yanlış alarm mesajı göster
            Döngü başa dön
        DEĞİLSE:
            Alarm seviyesi belirle (1-düşük, 2-orta, 3-yüksek)
            Bildirim gönder (SMS, App, Email)
            Kullanıcı alarmı sıfırlamak ister mi?
                EVET → alarm sıfırla
                        Döngü sona erer (sistem beklemede)
                HAYIR → döngüye devam et
    DEĞİLSE:
        Döngüye devam et (sensörleri tekrar oku)

BİTİR
