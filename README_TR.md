# TradingView Al/Sat Sinyali Göstergesi

## Genel Bakış
Bu, TradingView grafiklerinde otomatik al ve sat sinyalleri üreten bir Pine Script v6 göstergesidir. Gösterge, fiyat hareketleri analizine dayalı potansiyel alım satım fırsatlarını belirlemek için özel bir sinyal kütüphanesi kullanır.

**Yazar:** yashgode9 (YASH NANDKUMAR GODE)

## Özellikler

### Sinyal Üretimi
- **Otomatik Al/Sat Algılama**: Piyasa dönüşlerini belirlemek için zigzag tabanlı örüntü tanıma kullanır
- **Görsel Göstergeler**: Grafik üzerinde üçgen işaretçiler görüntüler
  - ALIŞ sinyalleri için yeşil yukarı üçgen (▲) (çubuğun altında)
  - SATIŞ sinyalleri için kırmızı aşağı üçgen (▼) (çubuğun üstünde)
- **Dinamik Etiketler**: Özelleştirilebilir görünümlü "Al-noktası" ve "Sat-noktası" etiketleri gösterir
- **Yeniden Çizim Modu**: Gerçek zamanlı sinyal güncellemeleri için şu anda `true` olarak ayarlanmıştır

### Özelleştirilebilir Parametreler

#### Sinyal Motoru Yapılandırması
- **DEPTH_ENGINE** (varsayılan: 8, 1s grafikler için optimize): Fiyat analizi derinliğini kontrol eder
  - Minimum: 1
  - Daha yüksek değerler = daha az duyarlı, daha az sinyal
  - Daha düşük değerler = daha duyarlı, daha fazla sinyal
  - Önerilen: 1s grafikler için 5-10, gün içi işlemler için 15-25, salınım işlemleri için 35-50

- **DEVIATION_ENGINE** (varsayılan: 2, 1s grafikler için optimize): Minimum fiyat sapma eşiğini belirler
  - Minimum: 1
  - Sinyal filtreleme duyarlılığını kontrol eder
  - Önerilen: 1s grafikler için 2-3, yüksek zaman dilimleri için 4-5

- **BACKSTEP_ENGINE** (varsayılan: 2, 1s grafikler için optimize): Erken sinyal üretimini önler
  - Minimum: 2
  - Değişken piyasalardaki gürültüyü filtreler
  - Önerilen: 1s grafikler için 2-3, yüksek zaman dilimleri için 4-5

#### Görsel Özelleştirme
- **Etiket Şeffaflığı** (varsayılan: 0): Etiket opaklığını kontrol eder (0-100)
- **Alış Rengi** (varsayılan: #03ff85 - parlak yeşil): Özelleştirilebilir alış sinyali rengi
- **Satış Rengi** (varsayılan: #fc0808 - parlak kırmızı): Özelleştirilebilir satış sinyali rengi
- **Etiket Boyutu** (varsayılan: 3 - normal): 5 boyut arasından seçim yapın
  1. Çok küçük
  2. Küçük
  3. Normal
  4. Büyük
  5. Çok büyük

### Alarm Sistemi
Gösterge kapsamlı bir alarm sistemi içerir:

1. **Yerleşik Alarmlar**: Her sinyal değişikliğinde tetiklenir
   - Alış sinyalleri için "Alış sinyali oluşturuldu!!!"
   - Satış sinyalleri için "Satış sinyali oluşturuldu!!!"

2. **Koşullu Alarmlar**: Kullanıcı tarafından kontrol edilebilir alarmlar
   - **Alış Alarmlarını Etkinleştir** (varsayılan: true)
   - **Satış Alarmlarını Etkinleştir** (varsayılan: true)
   - Alarm mesajında sembol adı ve mevcut fiyat bilgisi içerir

### Teknik Detaylar

#### Sinyal Mantığı
Gösterge, aşağıdakileri döndüren `signalLib_yashgode9/2` kütüphanesini kullanır:
- `direction`: Mevcut piyasa yönü (negatif = alış, pozitif = satış)
- `zee1`: Birincil zigzag nokta verisi
- `zee2`: İkincil zigzag nokta verisi

**Alış Sinyali Koşulları:**
- Yön değişir (`ta.change(direction) != 0`)
- VE yön negatiftir (`direction < 0`)

**Satış Sinyali Koşulları:**
- Yön değişir (`ta.change(direction) != 0`)
- VE yön pozitiftir (`direction > 0`)

#### Kaynak Limitleri
- Maksimum Etiket: 200
- Maksimum Çizgi: 50

## Kurulum

1. TradingView'ı açın
2. Pine Editör'e gidin
3. Yeni bir gösterge oluşturun
4. `buysellsignal-yashgode9.pine` dosyasındaki kodu kopyalayıp yapıştırın
5. "Grafiğe Ekle" düğmesine tıklayın

## Kullanım

### Temel Kurulum
1. Göstergeyi grafiğinize ekleyin
2. Varsayılan ayarlar çoğu zaman dilimi için iyi çalışır
3. Parametreleri işlem tarzınıza göre ayarlayın:
   - **1 Saniyelik Grafikler (Scalping)**: DEPTH_ENGINE (5-10), DEVIATION_ENGINE (2-3), BACKSTEP_ENGINE (2-3)
   - **Gün İçi İşlemler**: Daha fazla sinyal için daha düşük DEPTH_ENGINE (15-25)
   - **Salınım İşlemleri**: Daha az ama daha güçlü sinyaller için daha yüksek DEPTH_ENGINE (35-50)

### 1 Saniyelik Grafik Optimizasyonu (YENİ!)
Gösterge artık ultra hızlı 1 saniyelik grafikler için özel optimizasyonlar içermektedir:

#### Otomatik Parametre Tespiti
- 1s, 5s veya dakika altı zaman dilimlerinde çalıştığını otomatik olarak algılar
- Her zaman dilimi türü için optimize edilmiş varsayılan değerler sağlar
- Önerilen ayarlar parametre ipuçlarında görünür

#### Gürültü Filtreleme (1s Grafikler için Şiddetle Önerilir)
- **ATR Tabanlı Filtre**: Piyasa gürültüsünden kaynaklanan yanlış sinyalleri ortadan kaldırır
  - `ATR Çarpanı`: 1s grafikler için 0.2-0.5 (varsayılan: 0.3)
  - Düşük değerler = daha fazla sinyal, Yüksek değerler = daha az ama daha kaliteli sinyal

- **Volume Filtresi** (Opsiyonel): Sinyalleri hacim gücü ile doğrular
  - Zayıf hareketleri filtrelemeye yardımcı olur
  - Piyasa koşullarına göre etkinleştirilebilir/devre dışı bırakılabilir

- **Ardışık Sinyal Önleme**: Hızlı ateşli sinyalleri önler
  - Varsayılan: Aynı yönde sinyaller arası 2 bar
  - 1s grafiklerde aşırı işlem yapmayı azaltır

#### Debug Özellikleri
- **Debug Bilgi Paneli**: Gerçek zamanlı gösterge durumunu gösterir
  - Mevcut zaman dilimi
  - Aktif parametre değerleri
  - ATR ve minimum hareket eşikleri
  - Filtre durumu (aktif/inaktif)

- **Filtrelenmiş Sinyal Görselleştirmesi**: Hangi sinyallerin filtreler tarafından engellendiğini görün
  - Gri X işaretleri filtrelenen sinyalleri gösterir
  - Filtre parametrelerini ayarlamanıza yardımcı olur

#### 1s Grafikler için Performans İpuçları
1. **Muhafazakar Başlayın**: Varsayılan 1s ayarlarını kullanın (Depth=8, Deviation=2, Backstep=2)
2. **Gürültü Filtresini Etkinleştirin**: Yanlış sinyallerden kaçınmak için 1s zaman dilimlerinde kritik
3. **ATR'yi İzleyin**: Piyasa volatilitesine göre ATR çarpanını ayarlayın
4. **Debug Modunu Kullanın**: Sinyal davranışını anlamak için geçici olarak etkinleştirin
5. **Volume Filtresini Devre Dışı Bırakın**: Yüksek likiditeye sahip varlıklar ticareti yapmıyorsanız
6. **Replay Modunda Test Edin**: Canlı işlem yapmadan önce ayarlarınızı geri testte deneyin

### Alarm Kurulumu
1. Grafikteki gösterge adına sağ tıklayın
2. "buysellsignal-yashgode9 üzerinde Alarm Ekle" seçeneğini seçin
3. Şunlardan birini seçin:
   - Alış sinyali bildirimleri için "Alış Alarmı"
   - Satış sinyali bildirimleri için "Satış Alarmı"
4. Bildirim tercihlerinizi yapılandırın (e-posta, SMS, webhook, vb.)

### En İyi Uygulamalar
- **Sinyalleri Doğrulayın**: Sadece otomatik sinyallere dayalı işlem yapmayın
- **Stop Loss Kullanın**: Her zaman uygun risk yönetimi uygulayın
- **Önce Geçmiş Verilerde Test Edin**: Göstergeyi geçmiş verilerde test edin
- **Parametreleri Ayarlayın**: Ayarları spesifik piyasanız ve zaman diliminiz için optimize edin
- **Diğer Göstergelerle Birleştirin**: Hacim, RSI, MACD vb. ile birlikte kullanın

## Sinyal Yorumlama

### Alış Sinyalleri (Yeşil Üçgen ▲)
- Potansiyel yükseliş dönüşünü gösterir
- Yön negatife değişmiştir
- Fiyat yerel bir dipte olabilir
- Uzun pozisyonlar almayı düşünün

### Satış Sinyalleri (Kırmızı Üçgen ▼)
- Potansiyel düşüş dönüşünü gösterir
- Yön pozitife değişmiştir
- Fiyat yerel bir tepede olabilir
- Kar almayı veya kısa pozisyonlar açmayı düşünün

## Yapılandırma Grupları

Gösterge ayarları mantıksal gruplara ayrılmıştır:

1. **signalLib Yapılandırması**: Temel sinyal üretim parametreleri (1s grafikler için otomatik optimize edilmiş)
2. **Filtreler**: Gürültü azaltma ve sinyal kalitesi kontrolleri (YENİ!)
   - ATR tabanlı algılama ile gürültü filtresi
   - Hacim onay filtresi
   - Ardışık sinyal önleme
3. **Alarmlar**: Alarm türlerini etkinleştir/devre dışı bırak
4. **Etiketler**: Etiketlerin görsel görünümü
5. **Renkler**: Özelleştirilebilir renk şeması
6. **Debug**: Performans izleme ve sinyal analiz araçları (YENİ!)

## Önemli Notlar

### Yeniden Çizim
- Gösterge şu anda `repaint = true` olarak ayarlanmıştır
- Bu, sinyallerin kapanana kadar mevcut çubukta değişebileceği anlamına gelir
- Yeniden çizim yapmayan sinyaller için bu ayarın değiştirilmesi gerekir

### Bağımlılıklar
- `yashgode9/signalLib_yashgode9/2` kütüphanesini gerektirir
- Göstergenin çalışması için düzgün bir şekilde içe aktarılmalıdır

## Sorun Giderme

### Sinyal Görünmüyor
- DEPTH_ENGINE değerini artırın
- signalLib kütüphanesinin düzgün bir şekilde içe aktarıldığını kontrol edin
- Göstergenin doğru fiyat verisine uygulandığını doğrulayın

### Çok Fazla Sinyal
- DEPTH_ENGINE değerini artırın (40-50 deneyin)
- DEVIATION_ENGINE değerini artırın
- BACKSTEP_ENGINE değerini artırın
- **1s grafikler için**: Gürültü Filtresini etkinleştirin ve ATR Çarpanını artırın (0.4-0.6)
- Ek onay için Volume Filtresini etkinleştirin

### Çok Az Sinyal (1s Grafikler)
- DEPTH_ENGINE değerini azaltın (5-7 deneyin)
- ATR Çarpanını azaltın (0.2-0.25)
- Volume Filtresini devre dışı bırakın
- Filtre durumunu doğrulamak için Debug Bilgilerini kontrol edin

### 1s Grafiklerde Yanlış Sinyaller
- ATR Çarpanını artırın (0.4-0.6)
- Volume Filtresini etkinleştirin
- Ardışık Sinyal Barlarını artırın (3-5)
- Daha yüksek DEVIATION_ENGINE kullanın (3-4)

### Alarmlar Çalışmıyor
- "Alış Alarmlarını Etkinleştir" veya "Satış Alarmlarını Etkinleştir" seçeneğinin işaretli olduğunu doğrulayın
- TradingView'da alarmı oluşturduğunuzdan emin olun
- Alarm sıklığı ayarlarını kontrol edin (once_per_bar_close olarak ayarlanmış)

## Kod Yapısı

```
buysellsignal-yashgode9.pine
├── signalLib kütüphanesini içe aktar
├── Girdi parametreleri yapılandırması
├── signalLib kullanarak sinyal hesaplama
├── Etiket ve çizgi çizim mantığı
├── Al/Sat sinyali algılama
├── Alarm oluşturma
└── Görsel göstergeler (plotshape)
```

## Sürüm Bilgisi
- **Pine Script Sürümü**: v6
- **Kütüphane Sürümü**: signalLib_yashgode9/2
- **Maksimum Etiket**: 200
- **Maksimum Çizgi**: 50

## Lisans
yashgode9 (YASH NANDKUMAR GODE) tarafından oluşturulmuştur

## Sorumluluk Reddi
Bu gösterge yalnızca eğitim amaçlıdır. Alım satım önemli kayıp riski içerir. Geçmiş performans gelecekteki sonuçların göstergesi değildir. Alım satım kararları vermeden önce her zaman kendi araştırmanızı yapın ve finansal danışmanlara danışın.
