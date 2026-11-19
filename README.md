<div align="center">

# NVIDIA Kimodo Türkçe: Unitree G1 İnsansı Robot ve İnsan Karakterleri için Metinden Denetlenebilir Hareket Üretimi

**Kimodo: Scaling Controllable Human Motion Generation**

**Kinematik hareket difüzyon modeli · Metin istemi ve kinematik kısıtlar · SOMA, Unitree G1 ve SMPL-X modelleri**

**Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org · [www.robotlar.org](https://www.robotlar.org)**

👉 **Unitree G1 ürün sayfası: [robotlar.org/tr/insansi-robotlar/unitree-g1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · G1 EDU için teklif: [robotlar.org/tr/fiyat/unitree-g1-edu-fiyat](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat)**

📖 **Türkçe rehber: [NVIDIA Kimodo hareket üretimi rehberi](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi)**

[Genel Bakış](#-genel-bakış) · [Modeller](#-kimodo-modelleri) · [Başlarken](#-başlarken) · [Demo](#-etkileşimli-hareket-hazırlama-demosu) · [CLI](#-komut-satırı-arayüzü-cli) · [Robotik](#-kimodonun-robotikteki-aşağı-akış-uygulamaları) · [Belgeler](#-belgeler) · [G1 ile Başlangıç](#-unitree-g1-ile-kimodo-türkiyede-nereden-başlanır) · [Unitree Ürünleri](#-unitree-ürün-ailesi-türkiye)

</div>

<p align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/banner.png" alt="NVIDIA Kimodo hareket üretim modeli afişi" width="100%">
  <a href="https://github.com/nv-tlabs/kimodo/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-76B900.svg" alt="Lisans"></a>
  <a href="https://research.nvidia.com/labs/sil/projects/kimodo/"><img src="https://img.shields.io/badge/Project-Page-blue" alt="Proje sayfası"></a>
  <a href="https://research.nvidia.com/labs/sil/projects/kimodo/docs/index.html"><img src="https://img.shields.io/badge/docs-online-green.svg" alt="Belgeler"></a>
</p>

> Bu depo, NVIDIA'nın (nv-tlabs) resmi [nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo) projesinin README ve belgelerinin Türkçe çevirisidir. Kaynak kod, model ağırlıkları, örnek veriler ve görseller orijinal depoda ve Hugging Face'te kalır. Burada yalnız Türkçe anlatım bulunur ve görseller orijinal depodan gösterilir.
>
> **Çevrilen sürüm:** `main` dalı, commit `e1c219c` (15 Ekim 2026).
>
> **Lisans dağılımı:** kod ve belgeler Apache-2.0 (bu çeviri aynı lisansla, değişiklik notuyla yayımlanır). Model kontrol noktaları ve veriler, Hugging Face indirme sayfalarında gösterilen ayrı lisanslarla gelir. Ayrıntı [Lisans](#-lisans) bölümünde.
>
> **Kodu çalıştırmak için orijinal depoyu kullanın:** [github.com/nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo)
>
> Kimodo, NVIDIA'nın projesidir. Çeviri ve Türkçe teknik destek: Unitree Türkiye resmi distribütörü [Robotlar.org](https://www.robotlar.org).

## 🔎 Genel Bakış

Kimodo, büyük ölçekli (700 saat) ve ticari kullanıma uygun bir optik hareket yakalama veri setiyle eğitilmiş bir **ki**nematik **mo**tion **d**iffusi**o**n, yani kinematik hareket difüzyon modelidir. Model yüksek kaliteli 3B insan ve robot hareketleri üretir. Onu metin istemleriyle (prompt) ve geniş bir kısıt kümesiyle denetlersiniz: tam gövde poz anahtar kareleri, uç işlevci (end-effector) konumları/dönüşleri, 2B yollar ve 2B ara noktalar (waypoint). Model mimarisinin ve eğitiminin tüm ayrıntıları [teknik raporda](https://research.nvidia.com/labs/sil/projects/kimodo/assets/kimodo_tech_report.pdf) yer alır.

Orijinal depo şunları sunar:
- **Çıkarım (inference):** insan ve robot iskeletlerinde hareket üretmek için kod ve CLI
- **Etkileşimli demo:** metin istemleri ve kinematik denetimlerden oluşan bir zaman çizelgesi arayüzüyle kolay hareket hazırlama
- **Kıyaslama (benchmark):** hareket üretim modellerini metne ve kısıtlara uyma yeteneklerine göre değerlendirmek için [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) veri seti üzerine kurulu [test durumları](https://huggingface.co/datasets/nvidia/Kimodo-Motion-Gen-Benchmark) ve değerlendirme kodu
- **Açıklamalar (annotations):** Kimodo projesi için hazırlanan ince taneli zamansal metin tanımları [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) veri setine dahildir. Bu etiketler hakkında daha fazla bilgi için ayrı [Hugging Face depomuza](https://huggingface.co/datasets/nvidia/SEED-Timeline-Annotations) bakın.

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/teaser.gif" width="1280" alt="Kimodo ile metin istemlerinden ve kısıtlardan üretilen insan ve robot hareketleri">
</div>

## 📰 Haberler

Tüm değişikliklerin ayrıntılı listesi için [tam değişiklik günlüğüne](https://github.com/nv-tlabs/kimodo/blob/main/CHANGELOG.md) bakın.

- **[2026-10-15]** Kıyaslama, [ARDY](https://research.nvidia.com/labs/sil/projects/ardy/) hareket temsilini (motion rep) kullanmayı destekleyecek şekilde güncellendi. Kıyaslama uyumluluğu düzeltmeleri içeren SOMA-RP-v1.2 modelleri yayımlandı.
- **[2026-07-10]** [ARDY projesi](https://research.nvidia.com/labs/sil/projects/ardy/) yayımlandı: Kimodo'nun tüm denetlenebilirliğine sahip, _gerçek zamanlı_ bir hareket üretim modeli!
- **[2026-05-03]** _DÜZELTME_: kıyaslamada kısıt test durumları için ortalama metriklerin yanlış hesaplanmasına yol açan bir hata düzeltildi
- **[2026-04-24]** _YENİ_: çok istemli üretim iyileştirildi, `TEXT_ENCODER_DEVICE=cpu` ortam değişkeniyle düşük VRAM'li GPU'lara daha iyi destek geldi
- **[2026-04-10]** Yeni v1.1 Kimodo-SOMA modelleriyle birlikte [Kimodo Hareket Üretimi Kıyaslaması](#-kimodo-hareket-üretimi-kıyaslaması-benchmark) yayımlandı
- **[2026-03-19]** **Geriye dönük uyumsuz değişiklik:** Model girdileri/çıktıları artık SOMA 77 eklemli iskeletini (`somaskel77`) kullanır.
- **[2026-03-16]** Kimodo'nun ilk açık kaynak sürümü beş model çeşidi (SOMA, G1, SMPL-X), CLI, etkileşimli demo ve BONES-SEED için zaman çizelgesi açıklamalarıyla yayımlandı.


## 🧠 Kimodo Modelleri

Kimodo'nun çeşitli iskeletler ve veri setleriyle eğitilmiş birkaç çeşidi bulunur. Tüm modeller metinden harekete (text-to-motion) üretimi ve kinematik denetimleri destekler.

> Not: Modeller CLI'dan ya da Etkileşimli Demo'dan üretim yapmaya çalıştığınızda otomatik indirilir, bu yüzden onları elle indirmeniz gerekmez.

| Model | İskelet | Eğitim Verisi | Yayın Tarihi | Hugging Face | Lisans |
|:-------|:-------------|:------:|:------:|:-------------:|:-------------:|
| **Kimodo-SOMA-RP-v1.2** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 15 Ekim 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1.2) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-ArdyRep-v1.2** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 15 Ekim 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-ArdyRep-v1.2) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-v1.1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 10 Nisan 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1.1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-SEED-v1.1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 10 Nisan 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-SEED-v1.1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-v1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-G1-RP-v1** | [Unitree G1](https://github.com/unitreerobotics/unitree_mujoco/tree/main/unitree_robots/g1) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-G1-RP-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-SEED-v1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-SEED-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-G1-SEED-v1** | [Unitree G1](https://github.com/unitreerobotics/unitree_mujoco/tree/main/unitree_robots/g1) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-G1-SEED-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SMPLX-RP-v1** | [SMPL-X](https://github.com/vchoutas/smplx) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SMPLX-RP-v1) | [NVIDIA R&D Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-internal-scientific-research-and-development-model-license/) |

Hareket üretimi için varsayılan olarak tam Bones Rigplay 1 veri setiyle (700 saatlik mocap) eğitilen modelleri kullanmanızı öneririz.
BONES-SEED ile eğitilen modeller 288 saatlik [herkese açık mocap verisini](https://huggingface.co/datasets/bones-studio/seed) kullanır. Bu yüzden daha az yeteneklidir, ama BONES-SEED ile eğitilen öteki modellerle karşılaştırma için işe yarar. Hareket üretim modellerini Kimodo ile kolayca karşılaştırmak için [Hareket Üretimi Kıyaslamamıza](#-kimodo-hareket-üretimi-kıyaslaması-benchmark) bakın.

### Model güncelleme günlüğü
* _v1.2_: Bu modeller düzeltilmiş veri bölmeleriyle (split) eğitildi ve kıyaslama için bunlar kullanılmalıdır (bazı BONES-SEED test dizileri v1.1'in eğitim kümesine sızmıştı). **Kimodo-SOMA-RP-v1.2** yumuşatılmış kök (smooth-root) hareket temsilini korur. **Kimodo-SOMA-RP-ArdyRep-v1.2** ise ARDY modelleriyle doğrudan karşılaştırılabilmek için [ARDY](https://github.com/nv-tlabs/ardy) hareket temsilini (kök olarak ham kalça) kullanır. İkisinin kıyaslama sonuçları [kıyaslama sonuçlarında](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/results.md) yer alır.
* _v1.1_: Bu Kimodo-SOMA modelleri öncelikle [Hareket Üretimi Kıyaslaması](#-kimodo-hareket-üretimi-kıyaslaması-benchmark) ile uyumluluk için yayımlandı, ama v1'e göre küçük kalite iyileştirmeleri de içerir. Bu iyileştirmelerin ayrıntıları için [Kimodo-SOMA-RP-v1.1](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1.1#changes-in-v11) ve [Kimodo-SOMA-SEED-v1.1](https://huggingface.co/nvidia/Kimodo-SOMA-SEED-v1.1#changes-in-v11) Hugging Face sayfalarına bakın.

## 🚀 Başlarken

Ayrıntılı kurulum yönergeleri, CLI ile Etkileşimli Demo'nun kullanımı ve Kimodo ile hareket üretmeye dair öteki pratik ipuçları için tam belgelere bakın:

**[Tam belgeler (özgün, İngilizce)](https://research.nvidia.com/labs/sil/projects/kimodo/docs)**
- [Hızlı başlangıç rehberi](docs/HIZLI-BASLANGIC.md)
- [Kurulum yönergeleri](docs/KURULUM.md)
- [Etkileşimli hareket hazırlama demosu](docs/ETKILESIMLI-DEMO.md)
- [Komut satırı arayüzü](docs/KOMUT-SATIRI-ARAYUZU.md)
- [Kıyaslama yönergeleri](https://research.nvidia.com/labs/sil/projects/kimodo/docs/benchmark/introduction.html)
- [API başvurusu](https://research.nvidia.com/labs/sil/projects/kimodo/docs/api_reference/index.html)

Hareket üretimine **başlamadan önce** [en iyi uygulamaları](docs/EN-IYI-UYGULAMALAR.md) gözden geçirin ve [model sınırlamalarını](docs/EN-IYI-UYGULAMALAR.md#sınırlamalar) bilin.


Kurulum ortamına dair bazı notlar:
- Kimodo, yerelde tamamen GPU üzerinde üretim yapmak için yaklaşık 17 GB VRAM ister. Bunun başlıca nedeni metin gömme modelidir. Kartınız daha küçükse, metin kodlamayı CPU'ya almak için Kimodo komutlarını çalıştırırken `TEXT_ENCODER_DEVICE=cpu` ayarlayın. Bu biraz daha yavaştır ama VRAM kullanımını 3 GB'ın altına indirir.
- Model en çok GeForce RTX 3090, GeForce RTX 4090 ve NVIDIA A100 GPU'larında test edildi. NVIDIA, yeterli VRAM'i olan öteki yeni kartlarda da çalışmasını bekliyor.
- Bu depo Linux üzerinde geliştirildi. NVIDIA, Windows'ta da özellikle Docker ile çalışacağını belirtir.

## 🎬 Etkileşimli Hareket Hazırlama Demosu

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/demo_screenshot.png" width="1000" alt="Kimodo etkileşimli hareket hazırlama demosunun ekran görüntüsü, zaman çizelgesi ve 3B görüntüleyici">
</div>

<br>

**[Demo belgeleri ve eğitimi](docs/ETKILESIMLI-DEMO.md)**

Web tabanlı etkileşimli demo, Kimodo model çeşitlerinin herhangi biriyle hareket üretmek için sezgisel bir arayüz sunar. Kurulumdan sonra demoyu `kimodo_demo` komutuyla başlatırsınız. Demo yerelde http://127.0.0.1:7860 adresinde çalışır. Arayüze erişmek için bu adresi tarayıcınızda açın (ya da bir sunucuda kurduysanız bağlantı noktası yönlendirmesi kullanın, bkz. [Demoyu çalıştırma](docs/DEMO-CALISTIRMA.md)).

### Demo özellikleri
- **Birden çok karakter**: Kimodo'nun SOMA, G1 ve SMPL-X sürümleriyle üretimi destekler
- **Metin istemleri**: İstenen hareketlerin bir ya da daha çok doğal dil tanımını zaman çizelgesine girin
- **Zaman çizelgesi düzenleyicisi**: Birden çok kısıt izinde anahtar kareler ve kısıtlı aralıklar ekleyip düzenleyin
- **Kısıt türleri**:
  - Tam gövde: Belirli karelerde tüm eklem konumu kısıtları
  - 2B kök: Zemin düzleminde izlenecek ara noktaları ya da tam yolları tanımlayın
  - Uç işlevciler: El ve ayak konumlarını/dönüşlerini denetleyin
- **Kısıt düzenleme**: Düzenleme kipi kısıtların yeniden pozlanmasını ya da ara noktaların ayarlanmasını sağlar
- **3B görselleştirme**: Üretilen hareketlerin iskelet ve deri bağlanmış ağ (mesh) seçenekleriyle gerçek zamanlı çizimi
- **Oynatma denetimleri**: Üretilen hareketleri ayarlanabilir oynatma hızıyla önizleyin
- **Birden çok örnek**: Birden çok hareket çeşidi üretip karşılaştırın
- **Örnekler**: Kimodo'nun yeteneklerini daha iyi anlamak için hazır örnekleri yükleyin
- **Dışa aktarma**: Kısıtları ve üretilen hareketleri sonra kullanmak üzere kaydedin

## 💻 Komut Satırı Arayüzü (CLI)

**[CLI belgeleri ve örnekleri](docs/KOMUT-SATIRI-ARAYUZU.md)**

Hareketleri doğrudan komut satırından `kimodo_gen` komutuyla ya da `python -m kimodo.scripts.generate` betiğini çalıştırarak da üretebilirsiniz.

**Temel argümanlar:**
- `prompt`: İstenen hareket için tek bir metin tanımı ya da metinler dizisi (zorunlu)
- `--model`: Üretimde hangi Kimodo modelinin kullanılacağı
- `--duration`: Saniye cinsinden hareket süresi
- `--num_samples`: Üretilecek hareket çeşidi sayısı
- `--constraints`: Üretilen hareketi denetleyen kısıt dosyası (ör. web demosundan kaydedilmiş)
- `--diffusion_steps`: Gürültü giderme adımı sayısı
- `--cfg_type` / `--cfg_weight`: Sınıflandırıcısız yönlendirme (`nocfg`, tek ağırlıklı `regular` ya da metin ve kısıtlar için iki ağırlıklı `separated`); bkz. [CLI belgeleri](docs/KOMUT-SATIRI-ARAYUZU.md#sınıflandırıcısız-yönlendirme-cfg)
- `--no-postprocess`: Ayak kayması ve kısıt temizliği son işlemesini kapatan bayrak
- `--seed`: Yeniden üretilebilir sonuçlar için rastgele tohum değeri

Betik, kullanılan iskelete göre farklı çıktı biçimlerini destekler. Varsayılan olarak web demosuyla uyumlu özel bir NPZ biçimi kaydedilir.
Kimodo-G1 modellerinde hareket standart MuJoCo qpos CSV biçiminde kaydedilebilir.
Kimodo-SMPLX'te hareket, mevcut akışlarla uyumluluk için standart AMASS npz biçiminde kaydedilebilir.

### Varsayılan NPZ çıktı biçimi
Üretilen hareketler şunları içeren NPZ dosyaları olarak kaydedilir:
- `posed_joints`: Küresel eklem konumları `[T, J, 3]`
- `global_rot_mats`: Küresel eklem dönüş matrisleri `[T, J, 3, 3]`
- `local_rot_mats`: Yerel (üst ekleme göre) eklem dönüş matrisleri `[T, J, 3, 3]`
- `foot_contacts`: Ayak teması etiketleri [sol topuk, sol parmak ucu, sağ topuk, sağ parmak uçları] `[T, 4]`
- `smooth_root_pos`: Modelin çıkardığı yumuşatılmış kök temsilleri `[T, 3]`
- `root_positions`: Gerçek kök ekleminin (ör. leğen kemiği) yumuşatılmamış yörüngesi `[T, 3]`
- `global_root_heading`: Modelin çıkardığı yönelim yönü `[T, 2]`

`T` kare sayısı, `J` eklem sayısıdır. Tüm biçimlerin ayrıntısı [Çıktı biçimleri](docs/CIKTI-BICIMLERI.md) belgesindedir.

## 🐍 Düşük Düzeyli Python API

**[Model API belgeleri](https://research.nvidia.com/labs/sil/projects/kimodo/docs/api_reference/model.html#kimodo.model.kimodo_model.Kimodo.__call__)**

En geniş esneklik için düşük düzeyli model çıkarım API'sini üst düzey CLI yerine doğrudan çağırabilirsiniz.
Bu yol, sınıflandırıcısız yönlendirme ağırlıkları ve çok istemli dizilerdeki geçişlerle ilgili parametreler dahil gelişmiş model yapılandırmasına izin verir ([Üretim parametreleri](docs/URETIM-PARAMETRELERI.md)).

## 🦾 Kimodo'nun Robotikteki Aşağı Akış Uygulamaları

### G1 hareketlerini MuJoCo ile görselleştirme

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/mujoco_result.gif" width="800" alt="Kimodo ile üretilen Unitree G1 insansı robot hareketinin MuJoCo'da görselleştirilmesi">
</div>

G1 robot iskeletinde üretilen ve MuJoCo qpos CSV dosya biçiminde kaydedilen hareketler MuJoCo içinde kolayca kullanılır ve görselleştirilir.
Küçük bir görselleştirme betiği şu komutla çalışır:
```
python -m kimodo.scripts.mujoco_load
```
Çalıştırmadan önce betiği CSV dosyanızı doğru gösterecek şekilde düzenleyin ve Mujoco'yu kurun.

### Üretilen hareketleri ProtoMotions ile izleme

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/protomotions_results.gif" width="1280" alt="Kimodo hareketlerini ProtoMotions ile fizik tabanlı simülasyonda izleyen karakterler ve insansı robotlar">
</div>

[ProtoMotions](https://github.com/NVlabs/ProtoMotions), fiziksel olarak benzetilen dijital insanları ve insansı robotları eğitmek için GPU hızlandırmalı bir simülasyon ve öğrenme çatısıdır. Kimodo NPZ ve CSV çıktı biçimlerinin ikisi de ProtoMotions ile uyumludur. Bu sayede Kimodo'nun ürettiği hareketlerle fizik tabanlı politikalar kolayca eğitilir. ProtoMotions hem SOMA iskeletindeki hem Unitree G1'deki çıktıları destekler.

Kimodo ile hareket ürettikten sonra onları nasıl içe aktaracağınızı görmek için [ProtoMotions belgelerine](https://github.com/NVlabs/ProtoMotions?tab=readme-ov-file#-motion-authoring-with-kimodo) geçin.

### Hareketleri GMR ile başka robotlara yeniden hedefleme

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/gmr_results.gif" width="1280" alt="Kimodo-SMPLX hareketlerinin GMR ile farklı robotlara yeniden hedeflenmesi">
</div>

Kimodo-SMPLX ile üretilen hareketler [General Motion Retargeting (GMR)](https://github.com/YanjieZe/GMR) ile başka robotlara yeniden hedeflenebilir (retargeting).
GMR, AMASS NPZ biçimini doğrudan destekler. Bu yüzden Kimodo ile hareket üretip kaydetmek için `--output` kullanmanız yeterlidir. AMASS NPZ, `stem_amass.npz` dosyasına (tek örnek) ya da çıktı klasörüne (birden çok örnek) yazılır. Sonra GMR'deki [SMPL-X'ten robota betiğiyle](https://github.com/YanjieZe/GMR?tab=readme-ov-file#retargeting-from-smpl-x-amass-omomo-to-robot) desteklenen herhangi bir robota yeniden hedefleyin. Örneğin:
```
# run within GMR codebase
python scripts/smplx_to_robot.py --smplx_file /path/to/saved/amass_format.npz --robot booster_t1
```

### Kimodo'yu GEAR-SONIC ile birleştirme

<div align="center">
  <img src="https://raw.githubusercontent.com/nv-tlabs/kimodo/main/assets/sonic_kimodo_demo.gif" width="800" alt="Kimodo ile üretilen G1 hareketini simülasyonda izleyen GEAR-SONIC demosu">
</div>

Bir kavram kanıtı olarak Kimodo'yu [etkileşimli GEAR-SONIC demosuna](https://nvlabs.github.io/GEAR-SONIC/demo.html) da ekledik. Demoda Kimodo, G1 robot iskeletinde kinematik bir hareket üretir, ardından GEAR-SONIC bu hareketi simülasyonda izler.

## 📊 Kimodo Hareket Üretimi Kıyaslaması (Benchmark)

[**[Kıyaslama belgeleri](https://research.nvidia.com/labs/sil/projects/kimodo/docs/benchmark/introduction.html)**]
[**[Hugging Face'teki test takımı](https://huggingface.co/datasets/nvidia/Kimodo-Motion-Gen-Benchmark)**]

Kimodo modellerinin yanında, hareket üretim modellerinin değerlendirmesini geniş bir test durumu kümesiyle standartlaştırmak için tasarlanmış bir kıyaslama sunuyoruz. Kıyaslama şunları içerir:

* **Değerlendirme verisi**: [Hugging Face'te bulunan](https://huggingface.co/datasets/nvidia/Kimodo-Motion-Gen-Benchmark) bir test durumu takımı, tam kıyaslamayı kurmak için [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) veri setiyle birlikte kullanılır.
* **Çeşitli test durumları**: Test durumları metne ve kısıtlara koşullu hareket üretiminin geniş bir yelpazesini kapsar.
* **Değerlendirme akışı**: Kıyaslama kurulumu, hareket üretimi ve değerlendirme dahil tüm değerlendirme akışının kodu.
* **Metrikler**: Üretilen hareketleri hareket kalitesi, kısıtlara uyma ve metinle hizalanma açısından değerlendiren birkaç metrik. Bones Rigplay veri setinin 700 saatinin tamamıyla eğitilen [TMR-SOMA-RP-v1](https://huggingface.co/nvidia/TMR-SOMA-RP-v1) modelimiz, R-precision ve FID gibi yaygın metrikleri hesaplamak için güçlü bir gömme modelidir.

Gelecekteki araştırmaları kolaylaştırmak için Kimodo-SOMA-v1.1 modellerinin [kıyaslama sonuçlarını raporluyoruz](https://research.nvidia.com/labs/sil/projects/kimodo/docs/benchmark/results.html). Bu sonuçlar yeniden üretilebilir ve BONES-SEED verisiyle eğitilen öteki yöntemlerle kolayca karşılaştırılabilir.

## 📑 BONES-SEED için Zaman Çizelgesi Açıklamaları

[Teknik raporda](https://research.nvidia.com/labs/sil/projects/kimodo/assets/kimodo_tech_report.pdf) ayrıntılı anlatıldığı gibi Kimodo, mocap kliplerinin ince taneli zamansal metin açıklamalarıyla eğitilir.
Tam [Rigplay 1](https://bones.studio/datasets#rp01) veri seti tescillidir (proprietary). Herkese açık [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) alt kümesi için ise zamansal bölütlemeleri yayımladık.
Bu açıklamalar BONES-SEED veri setine zaten dahildir. Bağımsız etiketler ve onlarla ilgili ek bilgiler [Hugging Face'te](https://huggingface.co/datasets/nvidia/SEED-Timeline-Annotations) bulunur. SEED BVH dosyalarını Kimodo temsiline çevirmek için [BONES-SEED BVH verisini yükleme](docs/BONES-SEED-BVH-YUKLEME.md) belgesine bakın.


## 🤝 NVIDIA'daki İlgili İnsansı Çalışmaları
Kimodo, robotik, fiziksel yapay zeka ve öteki uygulamalar için insansı hareket verisini mümkün kılmaya yönelik daha geniş bir çalışmanın parçasıdır.

İlgili çalışmalar:
* [ARDY](https://github.com/nv-tlabs/ardy): etkileşimli uygulamalar için gerçek zamanlı, denetlenebilir hareket üretimini sağlamak üzere Kimodo'nun üzerine kurulur
* [MotionBricks](https://nvlabs.github.io/motionbricks/): hızlı ve dayanıklı hareket ara kare üretiminde (inbetweening) uzmanlaşmış gerçek zamanlı hareket üretim çatısı
* [SOMA Body Model](https://github.com/NVlabs/SOMA-X): birleşik, parametrik bir insan gövde modeli
* [BONES-SEED Dataset](https://huggingface.co/datasets/bones-studio/seed): SOMA ve G1 biçiminde büyük ölçekli bir insan(sı) hareket yakalama veri seti
* [ProtoMotions](https://github.com/NVlabs/ProtoMotions): fiziksel olarak benzetilen insan(sı)ları eğitmek için simülasyon ve öğrenme çatısı
* [SOMA Retargeter](https://github.com/NVIDIA/soma-retargeter): SOMA'dan G1'e yeniden hedefleme aracı
* [GEM](https://github.com/NVlabs/GEM-X): videodan insan hareketi yeniden kurma
* [GEAR SONIC](https://github.com/NVlabs/GR00T-WholeBodyControl): fiziksel robotlar için insansı davranış temel modeli

## 📚 Belgeler

Kimodo'ya yeni başlıyorsanız sırasıyla **[Kurulum](docs/KURULUM.md)**, **[Hızlı Başlangıç](docs/HIZLI-BASLANGIC.md)** ve **[En iyi uygulamalar](docs/EN-IYI-UYGULAMALAR.md)** belgelerini okuyun. G1 ile çalışacaksanız **[İskeletler](docs/ISKELETLER.md)** ve **[Çıktı biçimleri](docs/CIKTI-BICIMLERI.md)** belgeleri G1 iskeletini ve MuJoCo CSV düzenini anlatır.

**Başlarken**

- **[Kurulum](docs/KURULUM.md):** Hugging Face belirteci, paket kurulumu ve kaynak koddan kurulum.
- **[Sanal ortamla kurulum](docs/KURULUM-SANAL-ORTAM.md):** venv ya da conda, PyTorch, Viser çatalı ve düzenlenebilir kurulum.
- **[Docker ile kurulum](docs/KURULUM-DOCKER.md):** `text-encoder` ve `demo` kapsayıcıları.
- **[Kimodo-SMPLX modelini kullanma](docs/KIMODO-SMPLX-KURULUMU.md):** model erişimi ve SMPL-X gövde modeli.
- **[Hızlı Başlangıç](docs/HIZLI-BASLANGIC.md):** model tablosu, önerilen donanım, metin kodlayıcı hizmeti, CLI ve demo.

**Kullanıcı rehberi**

- **[Etkileşimli demo](docs/ETKILESIMLI-DEMO.md)** ve **[Demoyu çalıştırma](docs/DEMO-CALISTIRMA.md):** özellikler, SSH bağlantı noktası yönlendirmesi, Docker ipuçları.
- **[Komut satırı arayüzü](docs/KOMUT-SATIRI-ARAYUZU.md):** `kimodo_gen` argümanları, çok istemli üretim, kısıtlı üretim, CFG ve Python API.
- **[Kısıt JSON biçimi](docs/KISIT-JSON-BICIMI.md):** kısıt türleri, koordinat uzayı ve örnekler.
- **[Çıktı biçimleri](docs/CIKTI-BICIMLERI.md):** Kimodo NPZ, SOMA BVH, G1 MuJoCo CSV ve AMASS NPZ.
- **[Hareket biçimi dönüştürme](docs/HAREKET-BICIMI-DONUSTURME.md):** `kimodo_convert` ve 30 Hz yeniden örnekleme.
- **[BONES-SEED BVH verisini yükleme](docs/BONES-SEED-BVH-YUKLEME.md):** SEED BVH dosyasından Kimodo hareket temsiline.
- **[Üretim parametreleri](docs/URETIM-PARAMETRELERI.md):** sınıflandırıcısız yönlendirme ve gürültü giderme adımları.

**Temel kavramlar**

- **[Modele genel bakış](docs/MODEL-GENEL-BAKIS.md):** difüzyon süreci, iki aşamalı dönüştürücü gürültü giderici ve eğitim verisi.
- **[En iyi uygulamalar](docs/EN-IYI-UYGULAMALAR.md):** istem yazımı, kısıt kullanımı ve sınırlamalar.
- **[Hareket temsili](docs/HAREKET-TEMSILI.md):** yumuşatılmış kök ve poz öznitelikleri.
- **[Kısıtlar](docs/KISITLAR.md):** kısıt türleri, zaman kapsamı ve son işleme.
- **[İskeletler](docs/ISKELETLER.md):** SOMA, Unitree G1 ve SMPL-X.

| Türkçe belge | Kaynak dosya |
| --- | --- |
| [docs/KURULUM.md](docs/KURULUM.md) | `docs/source/getting_started/installation.md` |
| [docs/KURULUM-SANAL-ORTAM.md](docs/KURULUM-SANAL-ORTAM.md) | `docs/source/getting_started/installation_virtual_env.md` |
| [docs/KURULUM-DOCKER.md](docs/KURULUM-DOCKER.md) | `docs/source/getting_started/installation_docker.md` |
| [docs/KIMODO-SMPLX-KURULUMU.md](docs/KIMODO-SMPLX-KURULUMU.md) | `docs/source/getting_started/installation_smpl.md` |
| [docs/HIZLI-BASLANGIC.md](docs/HIZLI-BASLANGIC.md) | `docs/source/getting_started/quick_start.md` |
| [docs/ETKILESIMLI-DEMO.md](docs/ETKILESIMLI-DEMO.md) | `docs/source/interactive_demo/index.md` |
| [docs/DEMO-CALISTIRMA.md](docs/DEMO-CALISTIRMA.md) | `docs/source/interactive_demo/launching.md` |
| [docs/KOMUT-SATIRI-ARAYUZU.md](docs/KOMUT-SATIRI-ARAYUZU.md) | `docs/source/user_guide/cli.md` |
| [docs/KISIT-JSON-BICIMI.md](docs/KISIT-JSON-BICIMI.md) | `docs/source/user_guide/constraints.md` |
| [docs/CIKTI-BICIMLERI.md](docs/CIKTI-BICIMLERI.md) | `docs/source/user_guide/output_formats.md` |
| [docs/HAREKET-BICIMI-DONUSTURME.md](docs/HAREKET-BICIMI-DONUSTURME.md) | `docs/source/user_guide/motion_convert.md` |
| [docs/BONES-SEED-BVH-YUKLEME.md](docs/BONES-SEED-BVH-YUKLEME.md) | `docs/source/user_guide/seed_dataset.md` |
| [docs/URETIM-PARAMETRELERI.md](docs/URETIM-PARAMETRELERI.md) | `docs/source/user_guide/configuration.md` |
| [docs/MODEL-GENEL-BAKIS.md](docs/MODEL-GENEL-BAKIS.md) | `docs/source/key_concepts/model.md` |
| [docs/EN-IYI-UYGULAMALAR.md](docs/EN-IYI-UYGULAMALAR.md) | `docs/source/key_concepts/limitations.md` |
| [docs/HAREKET-TEMSILI.md](docs/HAREKET-TEMSILI.md) | `docs/source/key_concepts/motion_representation.md` |
| [docs/KISITLAR.md](docs/KISITLAR.md) | `docs/source/key_concepts/constraints.md` |
| [docs/ISKELETLER.md](docs/ISKELETLER.md) | `docs/source/key_concepts/skeleton.md` |

**Özgün dilde okunacak belgeler** (orijinal depoda):

- Demo sayfaları: [arayüze genel bakış](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/ui_overview.md) · [model seçimi](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/model_selection.md) · [örnekler](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/examples.md) · [üretim](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/generation.md) · [kısıtlar](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/constraints.md) · [sonuçları dışa aktarma](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/export_results.md)
- Kıyaslama: [giriş](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/introduction.md) · [akış](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/pipeline.md) · [metrikler](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/metrics.md) · [sonuçlar](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/results.md)
- Başvuru: [proje yapısı](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/project_structure.md) · [proje bilgisi](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/project_info.md) · [API başvurusu](https://github.com/nv-tlabs/kimodo/tree/main/docs/source/api_reference) · [belgeleri derleme](https://github.com/nv-tlabs/kimodo/blob/main/docs/README.md)

---

## 🧭 Unitree G1 ile Kimodo: Türkiye'de nereden başlanır

Kimodo'nun iki G1 modeli var: `Kimodo-G1-RP-v1` (Bones Rigplay 1, 700 saat mocap) ve `Kimodo-G1-SEED-v1` (BONES-SEED, 288 saat). İkisi de metin istemleri ve kinematik kısıtlarla Unitree G1 iskeletinde hareket üretir. Kimodo'nun kullandığı G1 iskeleti 34 eklemlidir. Öğrenmeyi kolaylaştırmak için ayak parmağı eklemleri eklendi, MuJoCo `qpos` CSV dışa aktarımında bu eklemler çıkarılır. CSV'nin her satırı 36 değer taşır: kök ötelemesi `[x, y, z]`, kök dördeyi (quaternion) `[w, x, y, z]` ve G1 eklem sırasında 29 eklem değeri. Koordinatlar MuJoCo düzenindedir (z yukarı, +x ileri).

Kimodo kinematik hareket üretir. NVIDIA'nın README'de gösterdiği iki robot yolu var: üretilen NPZ ve CSV hareketleriyle [ProtoMotions](https://github.com/NVlabs/ProtoMotions) içinde fizik tabanlı politika eğitmek ve GEAR-SONIC demosunda olduğu gibi G1 hareketini simülasyonda bir izleme denetleyicisine vermek. Fiziksel robota geçişte izleme denetleyicisi, limit uygulaması ve güvenlik doğrulaması sizin iş akışınızın parçasıdır. Belgelere göre ayak kayması ve kısıt son işlemesi G1 iskeletinde şu an sınırlı sonuç verir. Kimodo'yu insan hareketini G1'e aktaran [SOMA Retargeter](https://github.com/NVIDIA/soma-retargeter) ve gerçek zamanlı üretim yapan [ARDY](https://github.com/nv-tlabs/ardy) ile birlikte düşünün.

**Platform seçimi**

| Ürün | Kimodo iş akışındaki yeri |
| --- | --- |
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | Kimodo-G1 modellerinin hedeflediği insansı robot, 132 cm, 35 kg, 23-43 DoF |
| [Unitree G1 EDU teklif](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) | Geliştirici ve araştırma sürümü için teklif formu |
| [Unitree G1 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-plus) | 2 DoF boyun, yüksek tork, dokunsal kafa ve 6 mikrofonlu G1 |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [G1 Ar-Ge platformu](https://www.robotlar.org/tr/g1-arge-platformu) | Üniversite ve Ar-Ge laboratuvarları için G1 çözüm sayfası |

**Türkçe rehberler (iş akışına göre)**

- NVIDIA insansı hareket akışının bütünü: [NVIDIA insansı hareket ailesi ve Unitree G1](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi)
- Bu deponun özeti: [NVIDIA Kimodo hareket üretimi rehberi](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi)
- Kimodo üzerine gerçek zamanlı üretim: [NVIDIA ARDY rehberi](https://www.robotlar.org/tr/rehber/nvidia-ardy-gercek-zamanli-hareket-rehberi)
- Gerçek zamanlı ara kare üretimi ve G1 modeli: [Unitree G1 MotionBricks rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi)
- Üretilen hareketle fizik tabanlı eğitim: [NVIDIA ProtoMotions rehberi](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi)
- Gerçek robotta tüm gövde kontrolü: [GEAR SONIC ve Unitree G1 rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)
- Tüm vücut kontrolü ve GR00T: [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi)
- Referans hareketi izleyen politikaları eğitmek için pekiştirmeli öğrenme: [Unitree RL Gym](https://www.robotlar.org/unitree/rl-gym)
- Gerçek hareket verisi toplamak için XR teleoperasyon: [Unitree G1 teleoperasyon](https://www.robotlar.org/unitree/g1-teleoperasyon)
- Robotla haberleşme ve düşük seviye kontrol: [Unitree SDK rehberi](https://www.robotlar.org/unitree/sdk-rehberi)

**Türkçe GitHub depoları:** [G1](https://github.com/Unitree-Turkiye/unitree-g1-turkce) · [SDK2](https://github.com/Unitree-Turkiye/unitree-sdk2-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce) · [ARDY](https://github.com/Unitree-Turkiye/nvidia-ardy-turkce) · [MotionBricks](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce)

Üniversite ve kurumlar için platform, eğitim ve Türkçe geliştirici desteği konusunda [kurumsal teklif formundan](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) bize ulaşabilirsiniz.

## 🧬 NVIDIA İnsansı Hareket Ailesi (Türkçe)

NVIDIA'nın animasyon, robotik ve fiziksel yapay zeka için yayımladığı insansı hareket projeleri birbirini tamamlar: SOMA ortak gövde modelini, BONES-SEED veriyi, GEM videodan hareketi, Kimodo ve ARDY hareket üretimini, MotionBricks gerçek zamanlı hareket planlamayı, SOMA Retargeter insan hareketini Unitree G1'e aktarmayı, ProtoMotions fizik tabanlı eğitimi, GEAR SONIC de gerçek robotta tüm gövde kontrolünü üstlenir. Akışın bütününü [merkez rehberde](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi) anlatıyoruz.

| Proje | Ne yapar | Türkçe repo | Türkçe rehber | Resmi depo |
|---|---|---|---|---|
| MotionBricks | Bağlam ve hedef pozlar arasında gerçek zamanlı hareket üretir, Unitree G1 modeliyle gelir | [unitree-g1-motionbricks-turkce](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi) | [NVlabs/motionbricks](https://github.com/NVlabs/motionbricks) |
| **Kimodo** | **Denetlenebilir çevrim dışı insan hareketi üretimi** | **[nvidia-kimodo-turkce](https://github.com/Unitree-Turkiye/nvidia-kimodo-turkce)** | **[Rehber](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi)** | **[nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo)** |
| ARDY | Kimodo üzerine gerçek zamanlı, etkileşimli hareket üretimi | [nvidia-ardy-turkce](https://github.com/Unitree-Turkiye/nvidia-ardy-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-ardy-gercek-zamanli-hareket-rehberi) | [nv-tlabs/ardy](https://github.com/nv-tlabs/ardy) |
| SOMA Body Model | Birleşik, parametrik insan gövde modeli | [nvidia-soma-govde-modeli-turkce](https://github.com/Unitree-Turkiye/nvidia-soma-govde-modeli-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-soma-govde-modeli-rehberi) | [NVlabs/SOMA-X](https://github.com/NVlabs/SOMA-X) |
| BONES-SEED | SOMA ve G1 biçiminde büyük hareket yakalama veri seti | [bones-seed-hareket-veri-seti-turkce](https://github.com/Unitree-Turkiye/bones-seed-hareket-veri-seti-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi) | [bones-studio/seed](https://huggingface.co/datasets/bones-studio/seed) |
| ProtoMotions | Benzetilen insan ve insansıları eğitmek için GPU hızlandırmalı simülasyon çatısı | [nvidia-protomotions-turkce](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi) | [NVlabs/ProtoMotions](https://github.com/NVlabs/ProtoMotions) |
| SOMA Retargeter | SOMA hareketini Unitree G1 gibi insansı robotlara aktarır | [unitree-g1-soma-retargeter-turkce](https://github.com/Unitree-Turkiye/unitree-g1-soma-retargeter-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/soma-retargeter-unitree-g1-hareket-aktarimi-rehberi) | [NVIDIA/soma-retargeter](https://github.com/NVIDIA/soma-retargeter) |
| GEM | Görüntü ve videodan tüm gövde ve el hareketini kestirir | [nvidia-gem-x-turkce](https://github.com/Unitree-Turkiye/nvidia-gem-x-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-gem-x-videodan-hareket-rehberi) | [NVlabs/GEM-X](https://github.com/NVlabs/GEM-X) |
| GEAR SONIC | Fiziksel insansı robotlar için davranış temel modeli ve tüm gövde kontrolü | [unitree-g1-gear-sonic-turkce](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi) | [NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl) |

İlgili Türkçe kaynaklar: [Unitree G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [XR teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [RL Gym](https://github.com/Unitree-Turkiye/unitree-rl-gym-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce)

## 🤖 Unitree Ürün Ailesi (Türkiye)

Robotlar.org, Unitree Robotics'in Türkiye resmi distribütörüdür. Aşağıdaki ürünlerin Türkçe tanıtımı, teknik özellikleri ve kurumsal teklif formu sitede yer alır. Fiyatlar konfigürasyona göre değiştiği için [teklif üzerinden](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) paylaşılır.

### İnsansı robotlar

| Ürün | Ne işe yarar |
|---|---|
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | 132 cm, 35 kg, 23-43 DoF insansı robot |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [Unitree G1 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-plus) | 2 DoF boyun, yüksek tork, dokunsal kafa ve 6 mikrofonlu G1 |
| [Unitree H2](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2) | 182 cm, 70 kg, 31 DoF tam boy insansı robot |
| [Unitree H2-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-d) | 7 DoF kollu, tekerlekli çift kollu insansı robot |
| [Unitree H2 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-plus) | NVIDIA Isaac GR00T referans insansı robotu, 75 DoF, Jetson Thor |
| [Unitree H2-X](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-x) | Hizmet ve koruma görevleri, 4 m platformdan iniş |
| [Unitree R1](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1) | 123 cm, 29 kg, 26 DoF insansı robot (AIR, R1, EDU sürümleri) |
| [Unitree R1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1-d) | R1-A5 ya da R1-A7 (7-DOF) kollu çift kollu insansı platform |
| [Tüm insansı robotlar →](https://www.robotlar.org/tr/insansi-robotlar) | |

### Robot elleri

| Ürün | Ne işe yarar |
|---|---|
| [Unitree Dex3-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex3-1) | G1 için 7 serbestlik dereceli el, opsiyonel 33 dokunsal sensör |
| [Unitree Dex5-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-1) | 20 serbestlik dereceli (16 aktif) el, Dex5-1P sürümünde 94 dokunsal sensör |
| [Unitree Dex5-S](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-s) | 22 aktif serbestlik, 620 g, çift T1s Ethernet, 1000 Hz |
| [Unitree Dex1-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex1-1) | İki çeneli tutucu: 90 mm strok, 5-120 N ayarlı kavrama, RS-485 |
| [Tüm robot elleri →](https://www.robotlar.org/tr/robot-elleri) | |

### Dört ayaklı robotlar ve bileşenler

[Go2](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2) · [Go2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2-w) · [B2](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2) · [B2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2-w) · [A2](https://www.robotlar.org/tr/kopek-robotlar/unitree-a2) · [AS2](https://www.robotlar.org/tr/kopek-robotlar/unitree-as2) · [Tüm dört ayaklı robotlar](https://www.robotlar.org/tr/kopek-robotlar) · [4D LiDAR L2](https://www.robotlar.org/tr/unitree-bilesenler/4d-lidar-l2) · [Tüm Unitree bileşenleri](https://www.robotlar.org/tr/unitree-bilesenler)

## 📖 Türkçe Rehberler

- [NVIDIA insansı hareket ailesi ve Unitree G1 (merkez rehber)](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi)
- [NVIDIA Kimodo hareket üretimi rehberi](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi)
- [NVIDIA ARDY gerçek zamanlı hareket rehberi](https://www.robotlar.org/tr/rehber/nvidia-ardy-gercek-zamanli-hareket-rehberi)
- [Unitree G1 MotionBricks rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi)
- [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi)
- [Unitree LeRobot: veri toplama ve taklit öğrenme](https://www.robotlar.org/tr/rehber/unitree-lerobot-rehberi)
- [Unitree G1 XR teleoperasyon](https://www.robotlar.org/unitree/g1-teleoperasyon)
- [Unitree RL Gym](https://www.robotlar.org/unitree/rl-gym)
- [Unitree SDK rehberi](https://www.robotlar.org/unitree/sdk-rehberi)
- [Unitree eğitim çözümleri (üniversite ve laboratuvar)](https://www.robotlar.org/tr/unitree-egitim-cozumleri)
- [Tüm açık kaynak Unitree projeleri (Türkçe)](https://www.robotlar.org/tr/acik-kaynak)

## 🔗 Unitree Türkiye GitHub Ağı

**Modeller:** [Go2](https://github.com/Unitree-Turkiye/unitree-go2-turkce) · [Go2-W](https://github.com/Unitree-Turkiye/unitree-go2-w-turkce) · [B2](https://github.com/Unitree-Turkiye/unitree-b2-turkce) · [A2](https://github.com/Unitree-Turkiye/unitree-a2-turkce) · [G1](https://github.com/Unitree-Turkiye/unitree-g1-turkce) · [H2](https://github.com/Unitree-Turkiye/unitree-h2)
**Geliştirici:** [SDK2](https://github.com/Unitree-Turkiye/unitree-sdk2-turkce) · [XR Teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [Becerikli El](https://github.com/Unitree-Turkiye/unitree-hunerli-el-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce) · [ROS2](https://github.com/Unitree-Turkiye/unitree-ros2-turkce) · [VLA](https://github.com/Unitree-Turkiye/unitree-vla-turkce)
**G1 araştırma:** [G1 MotionBricks](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) · [G1 dokunsal algılama giysisi](https://github.com/Unitree-Turkiye/unitree-g1-dokunsal-giysi-turkce) · [G1 Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce)
**NVIDIA hareket ailesi:** [Kimodo](https://github.com/Unitree-Turkiye/nvidia-kimodo-turkce) · [ARDY](https://github.com/Unitree-Turkiye/nvidia-ardy-turkce) · [SOMA](https://github.com/Unitree-Turkiye/nvidia-soma-govde-modeli-turkce) · [BONES-SEED](https://github.com/Unitree-Turkiye/bones-seed-hareket-veri-seti-turkce) · [ProtoMotions](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) · [SOMA Retargeter](https://github.com/Unitree-Turkiye/unitree-g1-soma-retargeter-turkce) · [GEM](https://github.com/Unitree-Turkiye/nvidia-gem-x-turkce) · [GEAR SONIC](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce) · [🏠 Unitree Türkiye](https://github.com/Unitree-Turkiye)

## 🙏 Teşekkür

Orijinal proje şu değerli açık kaynak projelerin üzerine kuruludur (orijinal depodan):
- 3B hareket hazırlama demosu için [Viser](https://github.com/nerfstudio-project/viser)
- Metin kodlama için [LLM2Vec](https://github.com/McGill-NLP/llm2vec)

## 📄 Lisans

Orijinal kod tabanı [Apache-2.0](https://github.com/nv-tlabs/kimodo/blob/main/LICENSE) ile lisanslıdır. Model kontrol noktaları ve veriler, Hugging Face indirme sayfalarında gösterildiği gibi ayrı lisanslanır. [Model tablosunda](#-kimodo-modelleri) SOMA ve G1 modelleri NVIDIA Open Model lisansıyla, Kimodo-SMPLX-RP-v1 ise NVIDIA R&D Model lisansıyla görünür.

Proje ek üçüncü taraf açık kaynak yazılım projelerini indirir ve kurar. Kullanmadan önce bu açık kaynak projelerin lisans koşullarını inceleyin. Birlikte gelen üçüncü taraf bileşenlerin bildirimleri orijinal depodaki [ATTRIBUTIONS.MD](https://github.com/nv-tlabs/kimodo/blob/main/ATTRIBUTIONS.MD) dosyasındadır.

**Bu çevirinin lisansı:** Bu depodaki metinler, Apache-2.0 lisanslı Kimodo README ve belgelerinin Türkçe çevirisidir ve aynı [Apache-2.0](LICENSE) lisansıyla yayımlanır. Özgün eser: NVIDIA, [nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo). Değişiklik: Türkçeye çeviri ve Türkiye bağlantıları. Bu depo kaynak kod, model, veri ya da medya dosyası barındırmaz. Görseller orijinal depodan gösterilir.

## 📝 Atıf

Bu kodu araştırmanızda kullanırsanız lütfen şu atfı verin:

```bibtex
@article{Kimodo2026,
  title={Kimodo: Scaling Controllable Human Motion Generation},
  author={Rempe, Davis and Petrovich, Mathis and Yuan, Ye and Zhang, Haotian and Peng, Xue Bin and Jiang, Yifeng and Wang, Tingwu and Iqbal, Umar and Minor, David and de Ruyter, Michael and Li, Jiefeng and Tessler, Chen and Lim, Edy and Jeong, Eugene and Wu, Sam and Hassani, Ehsan and Huang, Michael and Yu, Jin-Bey and Chung, Chaeyeon and Song, Lina and Dionne, Olivier and Kautz, Jan and Yuen, Simon and Fidler, Sanja},
  journal={arXiv:2603.15546},
  year={2026}
}
```

## ✉️ İletişim

Sorular ve sorunlar için [orijinal depoda](https://github.com/nv-tlabs/kimodo/issues) bir issue açın ya da doğrudan yazarlara ulaşın. Bu Türkçe çeviriyle ilgili düzeltme önerilerinizi bu depoda issue olarak iletebilirsiniz.

---

> ℹ️ **Unitree Türkiye:** Unitree Robotics'in Türkiye resmi satış ve teknik distribütörü. Unitree G1 ailesi için teklif, kurulum, eğitim ve Türkçe geliştirici desteği: **[robotlar.org](https://www.robotlar.org/tr/rehber/unitree-turkiye-distributoru)** · 📧 info@robotlar.org

© 2026 **[www.robotlar.org](https://www.robotlar.org)**
