> Bu belge nv-tlabs/kimodo deposundaki `docs/source/key_concepts/model.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Modele genel bakış

Bir bakışta:
- Girdi: metin istemi + isteğe bağlı kısıtlar.
- Çıktı: tam gövde hareket dizisi
- Temel fikir: hareket özniteliklerinin gürültüsünü her adımda iki aşamalı bir dönüştürücüyle (transformer) gidermek.

Kimodo, iskelet pozlarından oluşan bir dizinin gürültüsünü gidererek 3B insan hareketi üreten açık (explicit) bir hareket difüzyon modelidir. Model, özenle tasarlanmış bir hareket temsili üzerinde çalışır. Bu temsil, üretilen hareket üzerinde hassas denetim sağlarken havada süzülme (floating) ve ayak kayması (foot skating) gibi yaygın bozulmaları en aza indirir. Temsilde, pratik animasyon araçlarında çizilen yolları taklit eden yumuşatılmış bir kök ile seyrek anahtar kare kısıtlarına uygun küresel eklem dönüşleri ve konumları bulunur.

Tüm ayrıntılar için [teknik rapora](https://research.nvidia.com/labs/sil/projects/kimodo/assets/kimodo_tech_report.pdf) bakın.

![Kimodo model mimarisi](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/arch.png)

## Difüzyon süreci

Gürültü giderme sürecinin her adımında model metin isteminin gömmesini, bir kinematik kısıt kümesini ve o anki gürültülü hareketi alır. Kısıtlar, girdi hareketiyle aynı hareket temsiliyle belirtilir ve gürültülü hareketteki karşılık gelen değerlerin üzerine yazılır. Ayrıca hangi öğelerin kısıtlı olduğunu gösteren bir maske girdi hareketine eklenir (concatenate). Amaç, girdi hareketinin temiz bir sürümünü tahmin etmektir.

## İki aşamalı dönüştürücü gürültü giderici

Bu girdilerle iki aşamalı dönüştürücü gürültü giderici, metne ve kısıtlara uyan temiz bir hareket tahmin eder. İki aşamalı gürültü giderici kök ve gövde hareketi tahminini ayırır: kök gürültü gidericisi önce küresel kök hareketini tahmin eder, bu hareket yerel bir temsile çevrilerek gövde gürültü gidericisine girdi olur. Son çıktı, iki aşamanın birleştirilmesidir.

## Eğitim veri seti

Kimodo'yu etkili biçimde eğitmenin temel bileşeni [Bones Rigplay](https://bones.studio/ai-datasets/) veri setidir. Bu büyük stüdyo mocap veri seti, metin tanımlarıyla birlikte 700 saati aşkın yapım kalitesinde insan hareketi içerir. Veri yürüyüşü/hareketi (lokomosyon), jestleri, gündelik etkinlikleri, yaygın nesne etkileşimlerini, video oyunu dövüşünü, dansı ve yorgun, kızgın, mutlu, üzgün, korkmuş, sarhoş, yaralı, sinsi, yaşlı ve çocuksu gibi çeşitli stilleri kapsar.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
