# MIPS 5 Aşamalı Pipeline — Windows Laboratuvar Kurulum ve Öğrenci Kullanım Kılavuzu

**Ders:** Bilgisayar Organizasyonu ve Mimarisi  
**Platform:** Windows 10/11 — Python 3.10+  
**Süre:** 90 dakika

## Amaç ve kapsam

Bu kılavuz, Windows 10/11 üzerinde MIPS 5 aşamalı (IF–ID–EX–MEM–WB) öğretim simülatörünün kurulmasını ve öğrencilerin üç deney yapmasını açıklar. Uygulama Python/Tkinter kullanır; gerçek bir MIPS işlemcisinin tüm ayrıntılarını modellemez.

## 1. Ön koşullar

Windows 10/11, Python 3.10 veya üzeri, ZIP dosyasını açma yetkisi. İnternet yalnızca Python kurulumu için gereklidir. Yönetici yetkisi çoğu kullanıcı kurulumunda gerekmez. Öğretim elemanı laboratuvar bilgisayarlarında Python kurulumunu önceden tamamlamalıdır.

## 2. Python kurulumu

Python resmi Windows indirme sayfasından kurulur: https://www.python.org/downloads/windows/ . Kurulumda PATH seçeneği varsa etkinleştirin. CMD veya PowerShell açıp `py --version` çalıştırın. Çıktı Python 3.10+ olmalıdır. `py` çalışmazsa `python --version` deneyin. Tkinter kontrolü: `py -m tkinter` (küçük pencere açılmalıdır).

## 3. Laboratuvar paketini yerleştirme

MIPS_Pipeline_Laboratuvari.zip dosyasını Dosya Gezgini ile açıp `C:\MIPS_Lab` içine çıkarın. Sonuçta `C:\MIPS_Lab\mips_pipeline_lab\app.py` dosyasının bulunduğunu doğrulayın. ZIP dosyası yalnızca bu kılavuz değildir; app.py ve simulator.py de gereklidir.

## 4. Çalıştırma ve test

PowerShell açın ve aşağıdaki komutları sırasıyla çalıştırın:
cd C:\MIPS_Lab\mips_pipeline_lab
py -m unittest -v test_simulator.py
py app.py
`py` komutu bulunmuyorsa komutlarda `python` kullanın. Testler başarısızsa deneylere başlamadan öğretim elemanına bildirin.

## 5. Arayüzü tanıma

MIPS Programı: komutların girildiği metin kutusu. Başlangıç register değerleri: her satıra `$t1=15` gibi bir değer. Bellek: her satıra `100=7` gibi adres=değer. Forwarding açık: veri yönlendirmesini etkinleştirir. Branch prediction: not_taken, taken, two_bit seçenekleri. Başlat / Sıfırla: mevcut girdilerle yeni simülasyon kurar. 1 çevrim ilerle: tek saat çevrimi çalıştırır. Tamamını çalıştır: simülasyonu tamamlar. Orta tablo: Cycle, IF, ID, EX, MEM, WB; alt alan: register ve bellek çıktıları.

## 6. Deney 1 — ALU ve veri bağımlılığı

Program:
add $t0,$t1,$t2
sub $t3,$t0,$t4
Register başlangıcı:
$t1=15
$t2=25
$t4=10
Bellek alanını boş bırakın. Forwarding açık seçin, Başlat / Sıfırla ve ardından 1 çevrim ilerle düğmesine art arda basın. Beklenen son değerler: $t0=40, $t3=30. Tablo üzerinden iki komutun EX aşamalarını ve ardışık bağımlılığı inceleyin.

## 7. Deney 2 — Load-use hazard ve forwarding

Program:
lw $t0,0($s1)
add $t2,$t0,$t3
sub $t4,$t2,$t5
Register başlangıcı:
$s1=100
$t3=5
$t5=12
Bellek:
100=7
Önce forwarding açıkken tamamını çalıştırın; stall, cycle ve CPI değerlerini not edin. Sonra Forwarding açık işaretini kaldırıp Başlat / Sıfırla ve Tamamını çalıştır yapın. Beklenen register sonuçları: $t0=7, $t2=12, $t4=0. Forwarding açıkken ardışık lw→add bağımlılığı için tipik olarak bir stall oluşur. Diğer sayılar simülatör modeline göre kaydedilmelidir.

## 8. Deney 3 — Branch prediction

Program:
addi $t0,$zero,1
beq $t0,$t0,TARGET
addi $t1,$zero,99
TARGET: addi $t2,$zero,7
Register ve bellek alanlarını boş bırakın. not_taken seçip Başlat / Sıfırla → Tamamını çalıştır yapın. Cycle, flush, misprediction ve CPI değerlerini kaydedin. Aynı deneyi taken ve two_bit ile tekrarlayın. Beklenen sonuç: $t1=0, $t2=7. Branch kararı EX aşamasında çözümlenir.

## 9. Öğrenci raporu

Öğrenci adı/numarası, bilgisayarın Python sürümü, her deney için MIPS kodu, başlangıç register/bellek değerleri, gözlenen son register değerleri, cycle/stall/flush/misprediction/CPI sonuçları, forwarding karşılaştırması ve branch tahmini yorumu teslim edilir. Tablo ekranının ve son durumun ekran görüntüsü eklenebilir.

## 10. Değerlendirme soruları

1) IF, ID, EX, MEM, WB aşamalarının görevi nedir?
2) `lw` sonrasındaki bağımlı `add` neden bekleyebilir?
3) Forwarding hangi bağımlılıkları azaltır?
4) Stall ile flush arasındaki fark nedir?
5) CPI nasıl hesaplanır ve kısa programlarda neden 1’den büyük olabilir?
6) Dallanma yanlış tahmin edildiğinde hangi komutlar iptal edilir?

## 11. Sık karşılaşılan sorunlar

`py` tanınmıyor: `python --version` deneyin veya Python kurulumunu düzeltin. `No module named tkinter`: Python kurulumundaki Tcl/Tk bileşenini etkinleştirin. `app.py` bulunamıyor: `dir` komutuyla klasörü kontrol edin. `No module named simulator`: app.py ile simulator.py aynı klasörde olmalıdır. Hatalı MIPS satırı: komut sözdizimini, register adlarını ve TARGET etiketini kontrol edin. Ayarları değiştirdikten sonra mutlaka Başlat / Sıfırla düğmesine basın.

## 12. Model sınırlamaları ve güvenlik

Bu uygulama eğitim amaçlı, tek komut genişlikli, sıralı beş aşamalı MIPS benzeri bir simülasyondur. Bellek sayısal adres=değer eşlemesiyle tutulur; cache, istisnalar, overflow, byte-adresleme ayrıntıları, branch delay slot ve gerçek donanım gecikmeleri modellenmez. Gerçek işlemci hız ölçümü değildir. Komutları yalnızca sağlanan uygulamada çalıştırın; yönetici yetkisi veya ağ erişimi gerekmez.


## 13. Öğrenci gözlem çizelgesi

| Deney | Forwarding | Tahmin | Cycle | Stall | Flush | Misprediction | CPI |
|---|---|---|---|---|---|---|---|
| 1 | Açık | — | | | | | |
| 2A | Açık | — | | | | | |
| 2B | Kapalı | — | | | | | |
| 3A | Açık | not_taken | | | | | |
| 3B | Açık | taken | | | | | |
| 3C | Açık | two_bit | | | | | |

**Teslim:** Her deneyin ekran görüntüsü, son register değerleri, tablodaki performans ölçümleri ve en az üç cümlelik teknik yorum. `CPI = toplam çevrim / tamamlanan komut sayısı` bağıntısını kullanın. Kısa programlarda pipeline dolma/boşalma maliyeti CPI'yi artırır.


**Teknik uyarı:** Bu simülatör gerçek MIPS donanımının tamamı değildir. Öğrencilerin gördüğü çevrim ve stall sayıları, programın bu eğitim modelindeki davranışıdır.
