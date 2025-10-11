ALGORİTMA: OnlineAlisverisSepet

BAŞLA
    // --- Başlangıç ---
    kullanıcı_giris ← FALSE
    sepet ← BOS_LISTE()
    toplam_tutar ← 0
    indirim_kodu_uygulandi ← FALSE
    kargo_ucreti ← 0

    // Kullanıcı girişi
    WHILE kullanıcı_giris == FALSE DO
        kullanıcı_adı ← KULLANICIDAN_GIRIS_AL("Kullanıcı adı:")
        sifre ← KULLANICIDAN_GIRIS_AL("Şifre:")
        IF GIRIS_DOGRULA(kullanıcı_adı, sifre) THEN
            kullanıcı_giris ← TRUE
            YAZDIR("Hoşgeldiniz, ", kullanıcı_adı)
        ELSE
            YAZDIR("Hatalı giriş. Tekrar deneyin.")
        ENDIF
    ENDWHILE

    // Ürün gezintisi ve sepete ekleme
    urun_secim ← ""
    WHILE urun_secim != "Çıkış" DO
        YAZDIR("Kategori seçin veya 'Çıkış' yazın:")
        kategori ← KULLANICIDAN_GIRIS_AL()
        
        // Kategori yoksa tekrar sor
        IF KATEGORI_VAR_MI(kategori) == FALSE THEN
            YAZDIR("Geçersiz kategori. Tekrar deneyin.")
            CONTINUE
        ENDIF

        // Ürün listesi göster
        urun_listesi ← URUN_LISTESI(kategori)
        YAZDIR("Ürünler: ", urun_listesi)
        YAZDIR("Sepete eklemek için ürün adını girin veya 'Çıkış' yazın:")
        urun_secim ← KULLANICIDAN_GIRIS_AL()

        IF urun_secim == "Çıkış" THEN
            BREAK
        ENDIF

        // Stok kontrolü
        IF STOK_VAR_MI(urun_secim) THEN
            sepet.EKLE(urun_secim)
            toplam_tutar ← toplam_tutar + URUN_FIYATI(urun_secim)
            YAZDIR(urun_secim, " sepete eklendi.")
        ELSE
            YAZDIR("Üzgünüz, stokta yok.")
        ENDIF
    ENDWHILE

    // Sepeti görüntüleme ve düzenleme
    WHILE TRUE DO
        YAZDIR("Sepetiniz: ", sepet)
        YAZDIR("1: Ürün çıkar  2: Devam et")
        secim ← KULLANICIDAN_GIRIS_AL()
        IF secim == "1" THEN
            YAZDIR("Çıkarmak istediğiniz ürünü girin:")
            urun_cikar ← KULLANICIDAN_GIRIS_AL()
            sepet.SIL(urun_cikar)
            toplam_tutar ← toplam_tutar - URUN_FIYATI(urun_cikar)
        ELSE
            BREAK
        ENDIF
    ENDWHILE

    // İndirim kodu uygulama
    YAZDIR("İndirim kodunuz var mı? (E/H)")
    cevap ← KULLANICIDAN_GIRIS_AL()
    IF cevap == "E" OR cevap == "e" THEN
        kod ← KULLANICIDAN_GIRIS_AL("İndirim kodunu girin:")
        IF KOD_GEÇERLI_MI(kod) THEN
            toplam_tutar ← toplam_tutar * 0.9 // %10 indirim örnek
            indirim_kodu_uygulandi ← TRUE
            YAZDIR("İndirim kodu uygulandı. Yeni tutar: ", toplam_tutar)
        ELSE
            YAZDIR("Geçersiz kod.")
        ENDIF
    ENDIF

    // Minimum 50 TL kontrolü
    IF toplam_tutar < 50 THEN
        YAZDIR("Minimum sipariş tutarı 50 TL. Sepete ürün ekleyin.")
        DUR
    ENDIF

    // Kargo ücreti hesaplama
    IF toplam_tutar >= 200 THEN
        kargo_ucreti ← 0
    ELSE
        kargo_ucreti ← 20
    ENDIF
    YAZDIR("Kargo ücreti: ", kargo_ucreti, " TL")
    toplam_tutar ← toplam_tutar + kargo_ucreti

    // Ödeme yöntemi seçimi
    YAZDIR("Ödeme yöntemi seçin: 1:Kredi Kartı  2:Kapıda Ödeme  3:Mobil Ödeme")
    odeme_secim ← KULLANICIDAN_GIRIS_AL()
    IF odeme_secim == "1" THEN
        KREDI_KARTI_ODEME()
    ELSE IF odeme_secim == "2" THEN
        KAPIDA_ODEME()
    ELSE IF odeme_secim == "3" THEN
        MOBIL_ODEME()
    ELSE
        YAZDIR("Geçersiz seçim. Ödeme iptal edildi.")
        DUR
    ENDIF

    // Sipariş onayı
    YAZDIR("Siparişiniz onaylandı. Toplam tutar: ", toplam_tutar)
    YAZDIR("Teşekkürler, iyi alışverişler!")

BİTİR
