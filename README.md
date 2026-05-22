# MC34063A ile 5V Düşürücü Regülatör

Bu proje, **MC34063AP** anahtarlamalı regülatör entegresi ile hazırlanmış bir **5V DC buck regülatör** KiCad tasarımıdır. Kart; DC girişten sabit **5V DC çıkış** üretmek için tasarlanmıştır ve through-hole parçalarla kolay montaj hedeflenmiştir.

> Not: Kart üzerindeki eski notlarda giriş 6.5-9V gibi düşünülmüş olabilir. MC34063A datasheet'inde entegre giriş aralığı **3V-40V** olarak verilir ve 5V/500mA düşürücü örneği **15V-25V giriş** ile gösterilir. Bu yüzden tasarım 9V ile sınırlı değildir; ancak üst giriş sınırı kullanılan kondansatör, diyot, bobin ve ısınma koşullarına göre doğrulanmalıdır.

## Görseller

> Fotoğraf yeri: Kartın ön yüzü (`docs/images/on-yuz.jpg`)

<!-- ![Kartın ön yüzü](docs/images/on-yuz.jpg) -->

> Fotoğraf yeri: Kartın bakır/alt yüzü (`docs/images/bakir-yuz.jpg`)

<!-- ![Kartın bakır yüzü](docs/images/bakir-yuz.jpg) -->

> Fotoğraf yeri: Tamamlanmış kart (`docs/images/tamamlanmis-kart.jpg`)

<!-- ![Tamamlanmış kart](docs/images/tamamlanmis-kart.jpg) -->

## Teknik Özellikler

- **Giriş:** 6.5V DC ve üzeri, 9V ile sınırlı değil
- **Çıkış:** 5V DC
- **Hedef akım:** 300mA sürekli, 500mA maksimum
- **Regülatör entegresi:** MC34063AP, DIP-8
- **Topoloji:** Düşürücü / buck converter
- **PCB boyutu:** Yaklaşık 77mm x 30mm
- **Tasarım aracı:** KiCad 9.0

## Devre Notları

Datasheet'e göre MC34063A; yükseltici, düşürücü ve tersleyici regülatör uygulamaları için gerekli osilatör, referans, karşılaştırıcı, akım sınırlama ve anahtarlama transistörünü içinde barındırır.

Bu kartta çıkış gerilimi geri besleme dirençleriyle ayarlanır:

```text
Vout = 1.25V x (1 + R3 / R1)
R1 = 1k
R3 = 3k
Vout ~= 5V
```

Akım sınırlama direnci `R2 = 0.33R` seçilmiştir. Datasheet formülüne göre yaklaşık tepe anahtar akımı:

```text
Ipk ~= 0.3V / Rsc
Ipk ~= 0.3V / 0.33R ~= 0.9A
```

Bu değer çıkış akımıyla aynı şey değildir; bobin doyma akımı, diyot akımı, kart ısısı ve giriş gerilimi mutlaka dikkate alınmalıdır.

## Ana Parçalar

| Referans | Değer / Parça | Açıklama |
|---|---:|---|
| U1 | MC34063AP | Anahtarlamalı regülatör entegresi |
| D1 | 1N5819 | Schottky diyot |
| L1 | 220uH | Buck bobini |
| R1 | 1k | Geri besleme alt direnci |
| R3 | 3k | Geri besleme üst direnci |
| R2 | 0.33R | Akım sınırlama direnci |
| C1 | 470pF | Zamanlama kondansatörü |
| C2 | 100uF | Giriş kondansatörü |
| C3 | 470uF | Çıkış kondansatörü |
| C4 | 100nF | Bypass kondansatörü |
| J1 | DC Input | Giriş klemensi |
| J2 | DC Output | 5V çıkış klemensi |

## Dosyalar

- `5VBuckMC34063A.kicad_sch`: Devre şeması
- `5VBuckMC34063A.kicad_pcb`: PCB yerleşimi
- `5VBuckMC34063A.kicad_pro`: KiCad proje dosyası
- `5VBuckMC34063A-ÖnYüz.pdf`: Ön yüz baskı çıktısı
- `5VBuckMC34063A-Bakır.pdf`: Bakır yüz baskı çıktısı
- `5VBuckMC34063A-backups/`: KiCad otomatik yedekleri

## Kullanım

1. `J1` klemensinden DC giriş uygulayın.
2. Polariteye dikkat edin: `VIN+` ve `GND` ters bağlanmamalıdır.
3. `J2` klemensinden 5V çıkış alın.
4. İlk testte akım sınırlı güç kaynağı kullanın.
5. 9V üzeri girişlerde parça voltaj değerlerini ve regülatör sıcaklığını kontrol edin.

## Datasheet

- [TI MC34063A / MC33063A Datasheet]([https://www.onsemi.com/download/data-sheet/pdf/mc34063a-d.pdf](https://www.ti.com/lit/ds/symlink/mc34063a.pdf))

## Uyarı

Bu kart hobi ve prototip amaçlı bir tasarımdır. Yüksek giriş gerilimlerinde, yüksek yük akımlarında veya uzun süreli çalışmada sıcaklık, ripple ve parça limitleri ölçülerek doğrulanmalıdır.
