> Bu belge nv-tlabs/kimodo deposundaki `docs/source/getting_started/quick_start.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Hızlı Başlangıç

Bu sayfa Kimodo ile hareket üretimine kısa bir giriş sunar. Ayrıntılı açıklamalar için her bölümde bağlantısı verilen tam belge sayfalarını incelemenizi öneririz.

Bu komutları çalıştırmadan önce Kimodo'yu bir sanal ortama ya da Docker ile kurmak için [kurulum rehberini](KURULUM.md) izleyin.

## Genel bakış: Kimodo modelleri
Hareket üretimi, iskelete ve eğitim veri setine göre değişen birkaç eğitilmiş Kimodo modeliyle yapılabilir.

> Not: Modeller CLI'dan ya da Etkileşimli Demo'dan üretim yapmaya çalıştığınızda otomatik indirilir, bu yüzden onları elle indirmeniz gerekmez.

| Model | İskelet | Eğitim Verisi | Yayın Tarihi | Hugging Face | Lisans |
|-------|------|------|-------------|-------------|----|
| **Kimodo-SOMA-RP-v1.2** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 15 Ekim 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1.2) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-ArdyRep-v1.2** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 15 Ekim 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-ArdyRep-v1.2) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-v1.1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 10 Nisan 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1.1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-SEED-v1.1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 10 Nisan 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-SEED-v1.1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-RP-v1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-RP-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-G1-RP-v1** | [Unitree G1](https://github.com/unitreerobotics/unitree_mujoco/tree/main/unitree_robots/g1) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-G1-RP-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SOMA-SEED-v1** | [SOMA](https://github.com/NVlabs/SOMA-X) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SOMA-SEED-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-G1-SEED-v1** | [Unitree G1](https://github.com/unitreerobotics/unitree_mujoco/tree/main/unitree_robots/g1) | [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-G1-SEED-v1) | [NVIDIA Open Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/) |
| **Kimodo-SMPLX-RP-v1** | [SMPL-X](https://github.com/vchoutas/smplx) | [Bones Rigplay 1](https://bones.studio/datasets#rp01) | 16 Mart 2026 | [Bağlantı](https://huggingface.co/nvidia/Kimodo-SMPLX-RP-v1) | [NVIDIA R&D Model](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-internal-scientific-research-and-development-model-license/) |

Hareket üretimi için varsayılan olarak tam Bones Rigplay veri setiyle (700 saatlik hareket yakalama, mocap) eğitilen modelleri kullanmanızı öneririz.
BONES-SEED ile eğitilen modeller 288 saatlik [herkese açık mocap verisini](https://huggingface.co/datasets/bones-studio/seed) kullanır. Bu yüzden daha az yeteneklidir, ama kendi eğittiğiniz modelleri aynı veri seti üzerinde karşılaştırmak için işe yarar. BONES-SEED üzerinde standart bir değerlendirme takımı için [kıyaslama (benchmark)](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/introduction.md) belgesine bakın.

### Önerilen donanım
Kimodo, yerelde tamamen GPU üzerinde üretim yapmak için yaklaşık 17 GB VRAM ister. Bunun başlıca nedeni metin gömme (text embedding) modelinin boyutudur. Kartınız daha küçükse, metin kodlamayı CPU'ya almak için Kimodo komutlarını çalıştırırken `TEXT_ENCODER_DEVICE=cpu` ayarlayın. Bu biraz daha yavaştır ama VRAM kullanımını 3 GB'ın altına indirir.

Model en çok GeForce RTX 3090, GeForce RTX 4090 ve NVIDIA A100 GPU'larında test edildi. NVIDIA, yeterli VRAM'i olan öteki yeni kartlarda da çalışmasını bekliyor.

## Metin kodlayıcı hizmetini çalıştırın
Hareket üretimi, girdi metin isteminin (prompt) gömülmesine dayanır ve bu gömme Kimodo'nun girdisi olur. CLI komutlarını ve demoyu tek başlarına çalıştırmak da uygundur. Yine de tüm hareket üretim isteklerinin ortak kullandığı _metin kodlayıcı hizmetini_ arka planda başlatmak daha iyi bir seçenek olabilir. Art arda çok sayıda CLI çağrısı yaptığınızda bu çok daha verimlidir, çünkü büyük metin kodlayıcının her seferinde yeniden yüklenmesini önler.

Metin kodlayıcı hizmetini başlatmak için:
```bash
kimodo_textencoder
```

Hizmetin ilk çalıştırılması, gömme modelini indirdiği için bir süre alır. Bunu arka planda ya da açık kalacağı ve öteki betiklerin kullanabileceği ayrı bir terminalde çalıştırmanızı öneririz.

Docker kurulumunu kullanıyorsanız hizmeti kapsayıcı içinde şu komutla da başlatabilirsiniz:
```bash
docker compose up text-encoder
```

> Not: Metin kodlayıcı başlatılırken transformers kütüphanesi LLM2Vec için birkaç beklenmeyen ve eksik katman bildirir. Bunlar beklenen bildirimlerdir ve güvenle göz ardı edilebilir.

16 GB'tan az VRAM'i olan bir GPU kullanıyorsanız metin kodlayıcıyı CPU'ya alabilirsiniz, örneğin:
```bash
TEXT_ENCODER_DEVICE=cpu kimodo_textencoder
```

## Komut satırından metinden harekete üretim
**[CLI belgeleri](KOMUT-SATIRI-ARAYUZU.md)**

Hareketleri komut satırından üretim betiğiyle oluşturabilirsiniz:

```bash
kimodo_gen "A person walks forward." \
    --model Kimodo-SOMA-RP-v1 \
    --duration 5.0 \
    --output output
```

`--model` seçeneği yukarıdaki tablodaki model adına karşılık gelir. Çıktı hareketi, `--output` ile verilen kök ad (stem) kullanılarak Kimodo [çıktı biçiminde](CIKTI-BICIMLERI.md) kaydedilir. Kısıtlarla hareket üretme dahil tüm üretim argümanlarının ayrıntılı açıklaması için tam [CLI belgelerine](KOMUT-SATIRI-ARAYUZU.md) bakın.

Kimodo'yu Docker ile kurduysanız üretimi Docker kapsayıcısı içinde de çalıştırabilirsiniz. Bunun için `kimodo_gen XXX` yerine `docker compose run --rm demo kimodo_gen XXX` yazın. Üretimi birçok kez çalıştıracaksanız `demo` kapsayıcısını (ör. başka bir terminalde ya da arka planda) başlatıp komutları onun içinde `docker compose exec demo kimodo_gen XXX` ile çalıştırmanız daha iyidir.


## Etkileşimli hareket hazırlama demosu
**[Demo belgeleri](ETKILESIMLI-DEMO.md)**

Demo, metin istemi ve kısıtlar için sezgisel bir denetim arayüzüyle kolayca hareket üretmenizi sağlar.

Demoyu şu komutla başlatın:
```bash
kimodo_demo
```

Demo, [http://localhost:7860](http://localhost:7860) adresinde çalışan bir web uygulamasıdır. Arayüze erişmek için bu adresi tarayıcınızda açın.

Docker kullanıyorsanız demoyu şu komutla başlatabilirsiniz:
```bash
docker compose up demo
```
Demoyu ve metin kodlayıcı hizmetini (yukarıda açıklandı) aynı anda başlatmak istiyorsanız şunu kullanın:
```bash
docker compose up
```

<details>
<summary>Docker için ek ipuçları</summary>

Kimodo'yu Docker kapsayıcıları içinde çalıştırıyorsanız şu komutlar işinize yarar. Aşağıdaki örnek komutlarda `demo` yerine `text-encoder` da yazabilirsiniz:

**Günlükleri (logs) görün:**

```bash
docker compose logs demo
```

**Hizmeti durdurun:**

```bash
docker compose stop demo
```

**Hizmeti yeniden başlatın:**

```bash
docker compose restart demo
```

**Her şeyi durdurup kaldırın:**

```bash
docker compose down
```

</details>

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
