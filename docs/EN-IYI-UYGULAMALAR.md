> Bu belge nv-tlabs/kimodo deposundaki `docs/source/key_concepts/limitations.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# En iyi uygulamalar

Bu sayfada, istem yazımı ve kısıtlar açısından Kimodo'dan en iyi sonucu almanın yollarını özetliyor, bilinen sınırlamaları ve hata durumlarını da topluyoruz. Ek bağlam için [teknik rapora](https://research.nvidia.com/labs/sil/projects/kimodo/assets/kimodo_tech_report.pdf) bakın.

## Metin istemi yazımı
- En iyi sonuç için her istemi "A person..." ile başlatın (ör. "A person walks forward" ya da "A person jumps and waves"). Bu kalıp hareketin öznesini ve amacını netleştirir ve eğitim verisindeki istemlerin üslubuna daha yakındır. Hareketi daha iyi tarif etmek için özneye "An old person..." ya da "A drunk person..." gibi bir stil de verebilirsiniz.
- Her istemi bir, en çok iki davranışa odaklayın. Uzun eylem dizileri için onları birden çok isteme bölün ve sırayla üretin.
- Bir hareketi orta düzeyde ayrıntıyla tarif etmek en iyisidir. "A person walks." gibi istemler fazla kısa ve belirsizdir. Her vücut parçasının hareketini ayrıntılı anlatan çok uzun istemler ise modelin kaldırabileceğinden fazladır. Eğitim verisinin çoğu bu ikisinin ortasındadır. İstem ayrıntı düzeyi hakkında fikir edinmek için [BONES-SEED veri setindeki](https://huggingface.co/datasets/bones-studio/seed) istemlere bakmanızı öneririz.
- Kimodo belirli bir insan davranışları kümesiyle eğitildi. Eğitim verisi genellikle yürüyüşü/hareketi (lokomosyon), jestleri, gündelik etkinlikleri, yaygın nesne etkileşimlerini, video oyunu dövüşünü, dansı ve yorgun, kızgın, mutlu, üzgün, korkmuş, sarhoş, yaralı, sinsi, yaşlı ve çocuksu gibi çeşitli stilleri kapsar. Bu kategorilerin dışındaki eylemler için yazılan istemler büyük olasılıkla kötü sonuç verir. Örneğin "A baseball player walks up to the plate and swings a bat" iyi bir istem sayılmaz, çünkü Kimodo beyzbol verisiyle eğitilmedi.
- Birden çok istem kullanırken (ör. zaman çizelgesi arayüzünde) her istemin tek başına yeterli bilgi taşıdığından emin olun. Örneğin 1. istem "A person is walking while carrying an object" ise 2. istem "A person walking carrying an object comes to a stop" olabilir. 2. istem bunun yerine "Then the person stops" olursa model daha önce ne olduğuna dair yeterli bağlama sahip olmaz ve düşük kaliteli hareketler üretebilir.

## Kısıtlar
- Verilen metin istemiyle ya da öteki kısıt türleriyle çelişen kısıtlardan kaçının. Kısıt doğruluğu ile metin doğruluğu arasında denge kurmakta zorlanıyorsanız [sınıflandırıcısız yönlendirme ağırlıklarını](URETIM-PARAMETRELERI.md) ayarlamayı deneyin.
- Yoğun 2B kök yolları dışında Kimodo ağırlıklı olarak seyrek zamansal kısıtlarla çalışmak üzere eğitildi. Kimodo, kısıt türü başına kısıt sayısı 20 anahtar karenin altında kaldığında en iyi sonucu verir.
- Ayak teması doğruluğu ve kısıtları tutturmak öncelikliyse [son işlemeyi](KISITLAR.md#son-işleme) açtığınızdan emin olun.

## Sınırlamalar
- **Hareket uzunluğu:** Üretilen hareketin süresi istem başına en çok 10 saniyedir
- **Kısıt sayısı:** Kısıt türü başına kısıtlı kare sayısı 20'nin altında olmalıdır (kök yolu kısıtı hariç)
- **Aşırı uzun ya da karmaşık istemler**, özellikle tek bir isteme birçok ayrı eylem sığdırıldığında hareketin amacını bulanıklaştırabilir.
- **Çelişen kısıtlar:** bozulmalara (artifact) ya da göz ardı edilen kısıtlara yol açabilir
- **Çok istemli diziler**: Bir istem dizisiyle hareket üretildiğinde her hareket tek tek üretilir. İkinci hareket ilkinin son karelerine koşullanır, bu yüzden istemler arasındaki geçiş aslında ikinci hareketin başında olur. Yani ikinci istem süresinin bir kısmını yumuşak bir geçişe ayırmak zorundadır. Bu da yeni istem içeriğini tam olarak yansıtmaya kalan süreyi kısaltabilir.
- **Son işleme**: Model tek başına ayak kayması üretebilir ve kısıtları tam olarak tutturmaz. Son işleme buna yardım eder, ancak G1 robot iskeletinde şu an sınırlı sonuç verir.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
