> Bu belge nv-tlabs/kimodo deposundaki `docs/source/key_concepts/constraints.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Kısıtlar

Kısıtlar, hareketin geri kalanını modelin serbestçe çözmesine bırakırken üretilen hareketi
belirli uzamsal hedeflere yönlendiren, zamanda yerelleşmiş sinyallerdir. Yörüngeyi,
pozu ve uç işlevcileri (end-effector) denetlemek için kısıtları metin istemleriyle birleştirebilirsiniz. Kısıtları en kolay [etkileşimli demoda](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/constraints.md) tanımlar, [JSON biçiminde](KISIT-JSON-BICIMI.md) kaydedersiniz.

![Zaman çizelgesi üzerinde kısıt türlerinin genel diyagramı](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/constraints.png)

## Neden kısıtlar?

Kısıtlar şunları yapmanızı sağlar:

- karakteri bir hedef poza ya da anahtar kareye sabitlemek
- doğal hareketi korurken zeminde bir yolu yönlendirmek
- elleri ya da ayakları belirli anlarda sabitlemek (örneğin dokunma ya da temas olayları)

## Kısıt türleri

Kimodo belirli kısıt türlerinde başarılı olacak şekilde eğitildi.

**Seyrek 2B kök ara noktası**: karakterin küresel ötelemesini yönlendiren, zemin düzlemindeki 2B ara noktalar. Bu kısıt, Kimodo'nun ürettiği yumuşatılmış kök temsilinin 2B bileşenlerini kısıtlar.

**Yoğun 2B kök yolu**: sürekli bir yörüngeyi yönlendiren yoğun 2B yol kısıtları. Bu kısıt, Kimodo'nun ürettiği yumuşatılmış kök temsilinin 2B bileşenlerini kısıtlar.

**Seyrek tam gövde anahtar karesi**: belirli karelerde tam gövde poz hedefleri. Kimodo hareket temsilinde bu kısıt, belirli bir karedeki yumuşatılmış kök konumunu ve tüm gövde eklem konumlarını kısıtlar.

**Seyrek uç işlevci kısıtı**: gövdenin geri kalanını esnek bırakırken el ya da ayak hedefleri. Bu kısıt, yumuşatılmış kök konumunu belirtilen uç işlevcilerle birlikte kısıtlar. Ellerde bilek konumunu ve dönüşünü el ucu konumuyla birlikte kısıtlar. Ayaklarda topuk konumunu ve dönüşünü parmak ucu konumuyla birlikte kısıtlar. Kimodo, uç işlevcilerin keyfi alt kümelerini destekleyecek şekilde eğitildi.

**Ayak temasları**: parmak ucu/topuk temas desenleri. Model bunu destekleyecek şekilde eğitildi. Demo arayüzü ve Python API şu an bu kısıtı sunmaz.


> **Not:** SOMA modellerinde kısıtlar tam `somaskel77` iskeleti üzerinde hazırlanıp gösterilebilir, ama Kimodo onları modele vermeden önce indirgenmiş `somaskel30` temsiline dönüştürür. Ayrıntı için [iskelet](ISKELETLER.md) bölümüne bakın.

## Koordinat uzayı

Tüm kısıt değerleri, birimleri **metre** olan **Y yukarı** bir koordinat sistemindedir. Model, kısıtları kökün 0. karede XZ = (0, 0) noktasından başladığı kanonik bir başlangıç noktasına göre bekler. Başlangıç yönelimi `first_heading_angle` üretim parametresiyle ayarlanır (varsayılan 0, +Z'ye bakar). Her alanın tüm ayrıntıları için [kısıt JSON biçimi](KISIT-JSON-BICIMI.md#koordinat-uzayı-ve-birimler) belgesine bakın.

## Zaman ve kapsam

CLI'mızda ve demomuzda kısıtlar şu düzeylerde tanımlanabilir:

- **Tek kareler**: anahtar kare tarzı kısıtlar
- **Aralıklar**: bir kare aralığı boyunca yönlendirme

Ancak yukarıda anlatıldığı gibi model daha çok seyrek anahtar karelerde başarılı olacak şekilde eğitildi. Yoğun anahtar kareler genellikle yalnız kök yollarında görülür. Ayrıntı için [en iyi uygulamalar](EN-IYI-UYGULAMALAR.md) belgesine bakın.

## Son işleme

Bir sinir ağının kısıtlara kesin biçimde uyması çok zor olduğu için demo ve CLI, hareketin kısıtları _tam olarak_ tutturmasını sağlayan hareket son işlemesini destekler. Bu iş, ivme ve hızdaki değişimleri en aza indirirken eklemleri yumuşakça ayarlayan hafif bir eniyileme (optimization) ile yapılır.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
