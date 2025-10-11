İsim Soy isim: Ömer Ensar Şahin
Öğrenci No: 250542022

ALGORİTMA: ATM_ParaCekme

BAŞLA
    // --- Başlangıç / Ön tanımlar ---
    max_pin_hata ← 3
    pin_hata_sayacı ← 0
    oturum_acık ← FALSE
    günlük_limit ← 5000         // örnek günlük limit (TL)
    kullanici_gunluk_cekim ← 0  // aynı gün içinde çekilen toplam
    hesap_bakiye ← HESAP_BAKIYE_OKU()  // bankadan/alttaki sistemden çek
    kart_bloklu ← FALSE

    // Kart takıldığında
    KART_TAKILDI_MI ← KONTROL_ET()
    IF KART_TAKILDI_MI == FALSE THEN
        YAZDIR("Lütfen kartınızı takınız.")
        DUR
    ENDIF

    // PIN doğrulama döngüsü
    WHILE pin_hata_sayacı < max_pin_hata AND kart_bloklu == FALSE DO
        PIN ← KULLANICIDAN_PIN_AL()
        IF PIN_DOĞRU_MU(PIN) THEN
            oturum_acık ← TRUE
            YAZDIR("PIN doğrulandı. Hoşgeldiniz.")
            BREAK
        ELSE
            pin_hata_sayacı ← pin_hata_sayacı + 1
            kalan_hak ← max_pin_hata - pin_hata_sayacı
            IF kalan_hak > 0 THEN
                YAZDIR("Hatalı PIN. Kalan hak: ", kalan_hak)
            ELSE
                YAZDIR("Kart bloke edildi. Banka ile iletişime geçin.")
                kart_bloklu ← TRUE
                LOCK_CARD()   // kartı bloke etme işlemi
                EJECT_CARD()
                LOG("Kart bloke: kullanıcıID, zaman")
                DUR
            ENDIF
        ENDIF
    ENDWHILE

    // Eğer oturum açıldıysa işlem menüsüne geç
    WHILE oturum_acık == TRUE DO
        YAZDIR("1: Bakiye sorgula  2: Para çek  3: İşlemi sonlandır")
        secim ← KULLANICIDAN_SECIM_AL()

        IF secim == 1 THEN
            YAZDIR("Mevcut bakiye: ", hesap_bakiye, " TL")
            CONTINUE  // menüye geri dön

        ELSE IF secim == 2 THEN
            // Para çekme işlemi
            YAZDIR("Çekmek istediğiniz tutarı girin (TL):")
            tutar ← KULLANICIDAN_TUTAR_AL()

            // Geçerlilik kontrolleri
            IF tutar <= 0 THEN
                YAZDIR("Geçersiz tutar. Pozitif bir değer girin.")
                CONTINUE
            ENDIF

            IF tutar % 20 != 0 THEN
                YAZDIR("Tutar 20 TL'nin katı olmalı. Lütfen 20, 40, 60 ... girin.")
                CONTINUE
            ENDIF

            // Günlük limit kontrolü
            IF (kullanici_gunluk_cekim + tutar) > günlük_limit THEN
                YAZDIR("Günlük limit aşılıyor. Kalan limit: ", (günlük_limit - kullanici_gunluk_cekim), " TL")
                CONTINUE
            ENDIF

            // Bakiye kontrolü
            IF tutar > hesap_bakiye THEN
                YAZDIR("Yetersiz bakiye. Mevcut bakiye: ", hesap_bakiye, " TL")
                CONTINUE
            ENDIF

            // ATM nakit stoğu kontrolü (opsiyonel)
            IF ATM_NAKIT_TIYETLI_MI(tutar) == FALSE THEN
                YAZDIR("ATM'de yeterli nakit yok. Lütfen başka tutar deneyin.")
                CONTINUE
            ENDIF

            // Tüm kontroller geçti — para verme adımları
            hesap_bakiye ← hesap_bakiye - tutar
            kullanici_gunluk_cekim ← kullanici_gunluk_cekim + tutar
            DISPENSE_CASH(tutar)        // fiziksel para basma
            YAZDIR(tutar, " TL verildi.")
            PRINT_RECEIPT(opcionel: TRUE, bakiye: hesap_bakiye, tutar: tutar)
            LOG("Para çekme: kullanıcıID, tutar, kalan bakiye, zaman")

            // İşlem sonrası sor
            YAZDIR("Başka işlem yapmak ister misiniz? (E/H)")
            cevap ← KULLANICIDAN_GIRIS_AL()
            IF cevap == "E" OR cevap == "e" THEN
                CONTINUE
            ELSE
                YAZDIR("Oturum sonlandırılıyor. Kartınızı alınız.")
                EJECT_CARD()
                oturum_acık ← FALSE
            ENDIF

        ELSE IF secim == 3 THEN
            YAZDIR("İşlem sonlandırılıyor. Kartınızı alınız.")
            EJECT_CARD()
            oturum_acık ← FALSE

        ELSE
            YAZDIR("Geçersiz seçim. Tekrar deneyin.")
        ENDIF
    ENDWHILE

    // Oturum kapandıktan sonra temizlik / log
    CLEAR_SESSION()
    BİTİR

