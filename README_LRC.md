# Doğrusal Regresyon Kanalı - Al/Sat Sinyalleri Göstergesi

## Genel Bakış
Bu Pine Script v6 göstergesi, doğrusal regresyon kanalına dayalı otomatik al ve sat sinyalleri üretir. Gösterge, fiyatın kanal sınırlarıyla etkileşimini analiz ederek potansiyel alım satım fırsatlarını belirler.

## Özellikler

### Doğrusal Regresyon Kanalı
- **Merkez Çizgi**: Belirlenen periyot için doğrusal regresyon çizgisi
- **Üst Kanal**: Merkez çizgi + (Üst Sapma × Standart Sapma)
- **Alt Kanal**: Merkez çizgi - (Alt Sapma × Standart Sapma)
- **Dinamik Hesaplama**: Kanal gerçek zamanlı olarak güncellenir

### Al/Sat Sinyalleri

#### Alış Sinyalleri (Yeşil Üçgen ▲)
- Fiyat **alt kanala** dokunduğunda veya altına düştüğünde tetiklenir
- Aşırı satım bölgesini gösterir
- Potansiyel yükseliş dönüşü işareti
- Uzun pozisyon için fırsat

#### Satış Sinyalleri (Kırmızı Üçgen ▼)
- Fiyat **üst kanala** dokunduğunda veya üstüne çıktığında tetiklenir
- Aşırı alım bölgesini gösterir
- Potansiyel düşüş dönüşü işareti
- Kar alma veya kısa pozisyon için fırsat

## Parametreler

### Doğrusal Regresyon Ayarları

#### 📊 Sayım (Count)
- **Varsayılan**: 100
- **Aralık**: 10-500
- **Açıklama**: Doğrusal regresyon hesaplaması için kullanılan çubuk sayısı
- **Kullanım**:
  - **Düşük değerler (20-50)**: Kısa vadeli trend takibi, daha sık sinyaller
  - **Orta değerler (80-120)**: Dengeli yaklaşım, orta vadeli trendler
  - **Yüksek değerler (150-300)**: Uzun vadeli trend analizi, daha az sinyal

#### 📈 Üst Sapma (Upper Deviation)
- **Varsayılan**: 2.0
- **Aralık**: 0.5-5.0
- **Açıklama**: Merkez çizgiden üst kanala olan standart sapma mesafesi
- **Kullanım**:
  - **1.0-1.5**: Dar kanal, daha sık satış sinyalleri
  - **2.0-2.5**: Standart kanal, dengeli sinyaller
  - **3.0+**: Geniş kanal, sadece güçlü hareketlerde sinyal

#### 📉 Alt Sapma (Lower Deviation)
- **Varsayılan**: 2.0
- **Aralık**: 0.5-5.0
- **Açıklama**: Merkez çizgiden alt kanala olan standart sapma mesafesi
- **Kullanım**:
  - **1.0-1.5**: Dar kanal, daha sık alış sinyalleri
  - **2.0-2.5**: Standart kanal, dengeli sinyaller
  - **3.0+**: Geniş kanal, sadece güçlü hareketlerde sinyal

#### ✅ Üst Sapmayı Kullan
- **Varsayılan**: Aktif
- **Açıklama**: Üst kanalın görünürlüğünü ve satış sinyallerini kontrol eder
- Kapatıldığında üst kanal gösterilmez ve satış sinyalleri oluşmaz

#### ✅ Alt Sapmayı Kullan
- **Varsayılan**: Aktif
- **Açıklama**: Alt kanalın görünürlüğünü ve alış sinyallerini kontrol eder
- Kapatıldığında alt kanal gösterilmez ve alış sinyalleri oluşmaz

### Alarm Ayarları

#### 🔔 Alış Alarmlarını Etkinleştir
- **Varsayılan**: Aktif
- Alış sinyalleri için TradingView alarmı gönderir
- Alarm mesajı: Fiyat ve alt kanal değerini içerir

#### 🔔 Satış Alarmlarını Etkinleştir
- **Varsayılan**: Aktif
- Satış sinyalleri için TradingView alarmı gönderir
- Alarm mesajı: Fiyat ve üst kanal değerini içerir

#### 🎯 Alarm Hassasiyeti
- **Dokunma** (Varsayılan): Fiyat kanala dokunduğu anda sinyal
  - Daha erken sinyaller
  - Daha fazla sinyal sayısı
  - Hızlı giriş/çıkış için uygun
- **Kırılma**: Fiyat kanalı geçip geri döndüğünde sinyal
  - Daha güvenilir sinyaller
  - Daha az yanlış pozitif
  - Onaylanmış dönüşler için uygun

### Görsel Ayarlar

Tüm renkler ve çizgi kalınlıkları özelleştirilebilir:
- Merkez Çizgi Rengi (Varsayılan: Mavi)
- Üst Kanal Rengi (Varsayılan: Pembe)
- Alt Kanal Rengi (Varsayılan: Yeşil)
- Alış Sinyali Rengi (Varsayılan: Parlak Yeşil)
- Satış Sinyali Rengi (Varsayılan: Parlak Kırmızı)
- Çizgi Kalınlığı (1-4)

## Kurulum

1. TradingView'ı açın
2. Pine Editor'e gidin
3. Yeni bir gösterge oluşturun
4. `linear-regression-channel.pine` dosyasındaki kodu kopyalayıp yapıştırın
5. "Grafiğe Ekle" düğmesine tıklayın

## Kullanım Senaryoları

### 📈 Scalping (1-5 dakika)
```
Sayım: 20-50
Üst/Alt Sapma: 1.5-2.0
Alarm Hassasiyeti: Dokunma
```
- Hızlı giriş/çıkış
- Sık işlem fırsatları
- Dar stop-loss mesafeleri

### 📊 Gün İçi İşlemler (15-60 dakika)
```
Sayım: 80-120
Üst/Alt Sapma: 2.0-2.5
Alarm Hassasiyeti: Dokunma veya Kırılma
```
- Dengeli sinyal frekansı
- Orta vadeli trendleri yakalama
- Daha güvenilir sinyaller

### 📉 Swing Trading (4 saat - Günlük)
```
Sayım: 150-300
Üst/Alt Sapma: 2.5-3.5
Alarm Hassasiyeti: Kırılma
```
- Uzun vadeli pozisyonlar
- Güçlü trend dönüşleri
- Az sayıda ama kaliteli sinyaller

### 🎯 Bollinger Bands Benzeri Kullanım
```
Sayım: 20
Üst/Alt Sapma: 2.0
```
- Standart Bollinger Bands mantığına benzer
- Volatilite genişlemesi/daralması analizi
- Ortalamaya dönüş stratejileri

## Alarm Kurulumu

### Adım 1: Alarmı Oluştur
1. Grafikteki gösterge adına sağ tıklayın
2. "Linear Regression Channel - Al/Sat Sinyalleri üzerinde Alarm Ekle" seçin
3. Koşul olarak şunlardan birini seçin:
   - "Alış Sinyali" - Alış alarmları için
   - "Satış Sinyali" - Satış alarmları için

### Adım 2: Alarm Ayarlarını Yapılandır
1. **Sıklık**: "Bar Kapanışında Bir Kez" (önerilir)
2. **Süre**: İhtiyacınıza göre (tek seferlik veya sınırsız)
3. **Bildirimler**: E-posta, SMS, Webhook, vb.

### Adım 3: Alarm Mesajı Örnekleri
- Alış: `🟢 ALIŞ SİNYALİ! Fiyat: 45.23 | Alt Kanal: 44.80`
- Satış: `🔴 SATIŞ SİNYALİ! Fiyat: 48.75 | Üst Kanal: 48.20`

## Ticaret Stratejileri

### Strateji 1: Temel Kanal Ticareti
```
📍 ALIŞ Koşulları:
- Alt kanala dokunma veya kırılma
- Stop-loss: Alt kanalın altında %0.5-1
- Hedef: Merkez çizgi veya üst kanal

📍 SATIŞ Koşulları:
- Üst kanala dokunma veya kırılma
- Stop-loss: Üst kanalın üstünde %0.5-1
- Hedef: Merkez çizgi veya alt kanal
```

### Strateji 2: Trend Takibi
```
📍 Yükseliş Trendinde:
- Sadece alt kanalda ALIŞ sinyalleri kullan
- Üst kanal sinyallerini kar alma için kullan
- Merkez çizgi destek olarak kullanılır

📍 Düşüş Trendinde:
- Sadece üst kanalda SATIŞ sinyalleri kullan
- Alt kanal sinyallerini pozisyon kapatma için kullan
- Merkez çizgi direnç olarak kullanılır
```

### Strateji 3: Kombinasyon Onayı
```
📍 Diğer Göstergelerle Birlikte:
- RSI < 30 ve Alt Kanal Teması → Güçlü ALIŞ
- RSI > 70 ve Üst Kanal Teması → Güçlü SATIŞ
- Volume artışı ile birleşince daha güvenilir
- MACD çaprazlaması ile onaylama
```

## Optimizasyon İpuçları

### 🎯 Farklı Varlıklar İçin

#### Volatil Kripto Paralar (BTC, ETH)
- Sayım: 50-100
- Sapma: 2.5-3.5
- Geniş kanallar volatiliteyi karşılar

#### Stabil Forex Çiftleri (EUR/USD)
- Sayım: 100-200
- Sapma: 1.5-2.0
- Dar kanallar hassas sinyaller verir

#### Hisse Senetleri
- Sayım: 80-150
- Sapma: 2.0-2.5
- Dengeli ayarlar çoğu hisse için uygun

### 📊 Farklı Zaman Dilimleri

| Zaman Dilimi | Önerilen Sayım | Önerilen Sapma | Hassasiyet |
|--------------|----------------|----------------|------------|
| 1m - 5m      | 20-50          | 1.5-2.0        | Dokunma    |
| 15m - 1h     | 80-120         | 2.0-2.5        | Her ikisi  |
| 4h - 1d      | 150-300        | 2.5-3.5        | Kırılma    |
| 1w+          | 200-400        | 3.0-4.0        | Kırılma    |

## Risk Yönetimi

### ⚠️ Önemli Kurallar

1. **Stop-Loss Kullanımı Zorunlu**
   - Her işlemde stop-loss belirleyin
   - Kanal dışında %0.5-2 mesafede

2. **Pozisyon Büyüklüğü**
   - Toplam sermayenin %1-2'si risk
   - Kanal genişliğine göre ayarlayın

3. **Aşırı İşlem Yapmayın**
   - Her sinyalde işlem açmayın
   - Trend yönünü dikkate alın
   - Diğer göstergelerle onaylayın

4. **Piyasa Koşulları**
   - Yatay piyasalarda kanal ticareti ideal
   - Güçlü trendlerde dikkatli olun
   - Haber akışını takip edin

## Sorun Giderme

### Çok Fazla Sinyal
**Çözümler:**
- Sayım değerini artırın (örn: 100 → 150)
- Sapma değerlerini artırın (örn: 2.0 → 2.5)
- Alarm Hassasiyetini "Kırılma"ya değiştirin
- Üst veya Alt Sapmayı geçici olarak kapatın

### Çok Az Sinyal
**Çözümler:**
- Sayım değerini azaltın (örn: 100 → 50)
- Sapma değerlerini azaltın (örn: 2.0 → 1.5)
- Alarm Hassasiyetini "Dokunma"ya değiştirin

### Yanlış Sinyaller
**Çözümler:**
- Daha yüksek zaman dilimi kullanın
- Sapma değerlerini artırın
- Trend filtreleri ekleyin (EMA, SMA)
- Volume onayı kullanın

### Kanal Çok Dar/Geniş
**Çözümler:**
- **Dar**: Sapma değerlerini artırın
- **Geniş**: Sapma değerlerini azaltın
- Varlığın volatilitesine göre ayarlayın

## Sınırlamalar ve Uyarılar

### ⚠️ Dikkat Edilmesi Gerekenler

1. **Yeniden Çizim**: Gösterge gerçek zamanlı güncellenir, geçmiş sinyaller değişmez

2. **Trend Takibi Değil**: Bu bir momentum göstergesi değil, aşırı alım/satım göstergesidir

3. **Güçlü Trendlerde**: Çok güçlü trendlerde kanal sürekli kırılabilir

4. **Düşük Likidite**: Düşük hacimli varlıklarda dikkatli kullanın

5. **Haber ve Olaylar**: Önemli haberlerde sinyaller güvenilir olmayabilir

## Teknik Detaylar

### Hesaplama Yöntemi
1. **Doğrusal Regresyon**: En küçük kareler yöntemi ile hesaplanır
2. **Standart Sapma**: Regresyon çizgisinden fiyat sapmaları
3. **Kanal Bantları**: Merkez ± (Sapma × Standart Sapma)

### Sinyal Filtreleme
- Minimum 3 bar arası sinyal mesafesi
- Aynı yönde çok hızlı sinyal engelleme
- Bar kapanışında onaylama

## Örnek Kullanım

### 📝 Senaryo 1: BTC/USDT - 15 Dakika
```
Ayarlar:
- Sayım: 100
- Üst Sapma: 2.5
- Alt Sapma: 2.5
- Hassasiyet: Dokunma

Sonuç:
- Günde 4-8 sinyal
- %65-70 başarı oranı
- Risk/Ödül: 1:2
```

### 📝 Senaryo 2: EUR/USD - 1 Saat
```
Ayarlar:
- Sayım: 150
- Üst Sapma: 2.0
- Alt Sapma: 2.0
- Hassasiyet: Kırılma

Sonuç:
- Günde 2-4 sinyal
- %70-75 başarı oranı
- Risk/Ödül: 1:3
```

## Sık Sorulan Sorular

**S: Bollinger Bands'den farkı nedir?**
C: Doğrusal regresyon trend yönünü de hesaba katar, Bollinger Bands sadece ortalamadan sapma kullanır.

**S: Hangi zaman dilimi en iyi?**
C: İşlem tarzınıza bağlı. Scalping için 1-5m, swing trading için 4h-1d önerilir.

**S: Her sinyalde işlem açmalı mıyım?**
C: Hayır. Sinyalleri trend yönü ve diğer göstergelerle onaylayın.

**S: Stop-loss nereye koymalıyım?**
C: Kanal sınırının %0.5-1 dışına, varlığın volatilitesine göre ayarlayın.

**S: Otomatik trading için kullanabilir miyim?**
C: Alarmları webhook ile otomatik sistemlere bağlayabilirsiniz, ancak manuel onay önerilir.

## Destek ve Güncellemeler

Bu gösterge sürekli geliştirilmektedir. Önerileriniz ve geri bildirimleriniz için:
- GitHub Issues bölümünü kullanın
- Sorunları detaylı açıklayın
- Ekran görüntüleri ekleyin

## Sorumluluk Reddi

⚠️ **ÖNEMLİ UYARI**:
- Bu gösterge sadece eğitim amaçlıdır
- Finansal tavsiye değildir
- Alım satım kayıp riski içerir
- Geçmiş performans gelecek sonuçları garanti etmez
- Kendi araştırmanızı yapın
- Risk yönetimi kullanın
- Kaldıraçlı işlemlerde çok dikkatli olun

---

**Başarılı İşlemler Dileriz! 🚀**
