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
- **DEPTH_ENGINE** (varsayılan: 30): Fiyat analizi derinliğini kontrol eder
  - Minimum: 1
  - Daha yüksek değerler = daha az duyarlı, daha az sinyal
  - Daha düşük değerler = daha duyarlı, daha fazla sinyal

- **DEVIATION_ENGINE** (varsayılan: 5): Minimum fiyat sapma eşiğini belirler
  - Minimum: 1
  - Sinyal filtreleme duyarlılığını kontrol eder

- **BACKSTEP_ENGINE** (varsayılan: 5): Erken sinyal üretimini önler
  - Minimum: 2
  - Değişken piyasalardaki gürültüyü filtreler

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
   - **Gün İçi İşlemler**: Daha fazla sinyal için daha düşük DEPTH_ENGINE (15-25)
   - **Salınım İşlemleri**: Daha az ama daha güçlü sinyaller için daha yüksek DEPTH_ENGINE (35-50)

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

1. **signalLib Yapılandırması**: Temel sinyal üretim parametreleri
2. **Alarmlar**: Alarm türlerini etkinleştir/devre dışı bırak
3. **Etiketler**: Etiketlerin görsel görünümü
4. **Renkler**: Özelleştirilebilir renk şeması

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
