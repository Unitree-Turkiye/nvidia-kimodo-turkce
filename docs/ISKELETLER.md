> Bu belge nv-tlabs/kimodo deposundaki `docs/source/key_concepts/skeleton.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# İskeletler

Kimodo'nun farklı sürümleri farklı iskeletleri (karakterleri) destekler. Her iskelet için ayrı bir model eğitilir. Şu an
kullanılabilen seçenekler [SOMA](https://github.com/NVlabs/SOMA-X), [G1](https://github.com/unitreerobotics/unitree_mujoco/tree/main/unitree_robots/g1) ve [SMPL-X](https://github.com/vchoutas/smplx) iskeletleridir.

Bu sayfada anlatılan iskeletler `kimodo/skeleton/definitions.py` içinde tanımlanır.

![İskeletlere genel bakış](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/skeletons/skeletons.png)

## SOMA (varsayılan)

SOMA, Kimodo'nun varsayılan iskeletidir. [BONES-SEED veri setinde](https://huggingface.co/datasets/bones-studio/seed) de kullanılan [SOMA gövde modeline](https://github.com/NVlabs/SOMA-X) dayanır.
Kimodo birbirine çok yakın iki SOMA iskelet tanımı kullanır:

- **`somaskel30`**: modelin içte ve temel SOMA kısıt formülasyonunda kullandığı indirgenmiş 30 eklemli iskelet. Parmak ve el ayrıntısının çoğunu çıkarır.
- **`somaskel77`**: dışa dönük görselleştirmede ve SOMA hareket dışa aktarımlarında kullanılan tam 77 eklemli SOMA iskeleti.

Uygulamada Kimodo SOMA hareketlerini `somaskel30` üzerinde tahmin eder, sonuçları döndürürken ya da demoda görselleştirirken `somaskel77` iskeletine dönüştürür. Daha eski varlıklar ve örnekler `somaskel30` üzerinde saklanmış olabilir. Araçlar bu dosyalarla geriye dönük uyumluluğu korur.

Kimodo'nun tüm eğitim verisinin, SOMA gövde modeli için tek bir kimlik parametresi kümesine karşılık gelen tek tip bir iskelet oranında olduğunu unutmayın.

![SOMA iskeletleri](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/skeletons/soma_skels.png)

SOMA iskeletindeki çıktılar iki yolla görselleştirilebilir. Birincisi sabit bir SOMA donanımını (rig) eklemlendirip geleneksel deri bağlama (skinning) yapmaktır (kod tabanında `kimodo/viz/soma_skin.py` dosyasına karşılık gelir).
Bunun yerine üretilen eklem dönüşlerini, tek tip iskeletimizin gövde şekline karşılık gelen kimlik parametreleriyle SOMA katmanından da geçirebiliriz. Kod tabanındaki örneği `kimodo/viz/soma_layer_skin.py` dosyasındadır ve `kimodo/assets/skeletons/somaskel30/soma_base_fit_mhr_params.npz` içinde tanımlanan kimlik parametrelerini (BONES-SEED verisindekilerle aynı) kullanır.

Veri işlemedeki bazı özellikler yüzünden SOMA donanımı ile SOMA katmanı görselleştirmede çok az farklı sonuç verir. SOMA donanımı Kimodo'nun eğitildiği veriyi daha iyi yansıtır.

## Unitree G1

G1 iskeleti MuJoCo uyumlu dışa aktarımları ve robotik iş akışlarını hedefler.
Kimodo'nun kullandığı sürüm, öğrenmeyi kolaylaştırmak için ayak parmakları için ek eklemler eklenmiş 34 eklemli bir iskelettir. Üretilen hareketler MuJoCo `qpos` CSV biçimine dışa aktarıldığında bu eklemler, aşağı akış uygulamalarıyla uyumlu olmak için çıkarılır.

<img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/skeletons/g1.png" alt="Unitree G1 insansı robot iskeleti, Kimodo" width="60%">

## SMPL-X

Bu iskelet SMPL-X modeliyle hizalanır ve AMASS tarzı dışa aktarımları destekler. Yalnız gövde eklemlerine karşılık gelen 22 eklem kullanır. Bu seçenek SMPL-X akışlarıyla ya da AMASS parametreleri bekleyen aşağı akış araçlarıyla uyumluluk için işe yarar. Ancak üretilen hareketlerde özellikle belirgin yeniden hedefleme (retargeting) bozulmaları görülebildiği için önerilen Kimodo modeli SMPL-X **olmaz**.

<img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/skeletons/smplx.png" alt="SMPL-X iskeleti" width="60%">

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
