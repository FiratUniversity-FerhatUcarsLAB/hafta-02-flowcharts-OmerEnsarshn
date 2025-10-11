BAŞLA
Öğrenci girişi (öğrenci no + şifre)
DERS_LISTESI = tüm dersler

DERS_SECIM_DÖNGÜSÜ:
    Dersleri görüntüle
    Seçilen ders için:
        Eğer kontenjan doluysa:
            Uyarı: "Kontenjan dolu"
            tekrar ders seç
        Eğer ön koşul dersleri alınmamışsa:
            Uyarı: "Ön koşul dersleri eksik"
            tekrar ders seç
        Eğer zaman çakışması varsa:
            Uyarı: "Zaman çakışması"
            tekrar ders seç
        Eğer toplam kredi > 35:
            Uyarı: "Kredi limiti aşıldı"
            tekrar ders seç
        Eğer GPA < 2.5 ve danışman onayı gerekli:
            Danışman onayı iste
            Eğer onay verilmezse:
                Ders eklenemez
        Ders ekle/çıkar
    Başka ders eklemek ister misiniz? (EVET/HAYIR)
    EVET → ders seçimine dön
    HAYIR → kayıt özeti göster ve onayla

BİTİR
