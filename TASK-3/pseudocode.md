
ALGORİTMA: HastaneRandevuTahlil

BAŞLA
    hasta_dogrulama ← FALSE
    devam_et ← TRUE

    // Hasta kimlik doğrulama
    WHILE hasta_dogrulama == FALSE DO
        tc_no ← KULLANICIDAN_GIRIS_AL("TC Kimlik No:")
        IF TC_DOGRULA(tc_no) THEN
            hasta_dogrulama ← TRUE
            YAZDIR("Kimlik doğrulama başarılı")
        ELSE
            YAZDIR("Hatalı TC No, tekrar deneyin")
        ENDIF
    ENDWHILE

    // Ana işlem döngüsü
    WHILE devam_et == TRUE DO
        YAZDIR("İşlem seçin: 1-Randevu Al, 2-Tahlil Sonucu Gör")
        secim ← KULLANICIDAN_GIRIS_AL()

        IF secim == "1" THEN
            // --- Randevu Modülü ---
            poliklinik_sec ← KULLANICIDAN_GIRIS_AL("Poliklinik seçin:")
            doktor_listesi ← DOKTOR_LISTESI(poliklinik_sec)
            YAZDIR("Doktorlar: ", doktor_listesi)
            doktor_sec ← KULLANICIDAN_GIRIS_AL("Doktor seçin:")

            uygun_saatler ← UYGUN_SAATLER(doktor_sec)
            YAZDIR("Uygun saatler: ", uygun_saatler)
            saat_sec ← KULLANICIDAN_GIRIS_AL("Saat seçin:")

            YAZDIR("Randevu onaylandı: ", poliklinik_sec, doktor_sec, saat_sec)
            SMS_GONDER(tc_no, poliklinik_sec, doktor_sec, saat_sec)

        ELSE IF secim == "2" THEN
            // --- Tahlil Modülü ---
            IF TAHLIL_VAR_MI(tc_no) == FALSE THEN
                YAZDIR("Tahlil bulunamadı")
            ELSE
                IF SONUC_HAZIR_MI(tc_no) THEN
                    YAZDIR("Tahlil sonucu: ", TAHLIL_SONUCU(tc_no))
                    YAZDIR("PDF indirmek ister misiniz? (E/H)")
                    cevap ← KULLANICIDAN_GIRIS_AL()
                    IF cevap == "E" OR cevap == "e" THEN
                        PDF_INDİR(tc_no)
                    ENDIF
                ELSE
                    YAZDIR("Sonuç henüz hazır değil. Lütfen daha sonra tekrar kontrol edin.")
                ENDIF
            ENDIF
        ELSE
            YAZDIR("Geçersiz seçim")
        ENDIF

        // Başka işlem yapmak ister mi
        YAZDIR("Başka işlem yapmak ister misiniz? (E/H)")
        cevap ← KULLANICIDAN_GIRIS_AL()
        IF cevap == "H" OR cevap == "h" THEN
            devam_et ← FALSE
        ENDIF
    ENDWHILE

BİTİR
