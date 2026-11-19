> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/cli.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Komut satırı arayüzü (CLI)

Ana CLI giriş noktası `kimodo_gen` komutudur. Bu komut `kimodo/scripts/generate.py` konumundaki betiğe karşılık gelir, bu yüzden aynı işi `python -m kimodo.scripts.generate` ile de yapabilirsiniz.

**Docker ile kullanım**: Kimodo'yu Docker ile kurduysanız üretimi Docker kapsayıcısı içinde de çalıştırabilirsiniz. Bunun için `kimodo_gen XXX` yerine `docker compose run --rm demo kimodo_gen XXX` yazın. Üretimi birçok kez çalıştıracaksanız `demo` kapsayıcısını (ör. başka bir terminalde ya da arka planda) başlatıp komutları onun içinde `docker compose exec demo kimodo_gen XXX` ile çalıştırmanız daha iyidir.

**Tek istemle üretim:**

```bash
kimodo_gen "A person walks forward." \
    --model Kimodo-SOMA-RP-v1 \
    --duration 5.0 \
    --output output
```

`--model` seçeneği, üretimde kullanılacak Kimodo model kontrol noktasına (checkpoint) karşılık gelir. Verilmezse varsayılan olarak en son Kimodo-SOMA-RP modeli (şu an `Kimodo-SOMA-RP-v1.2`) kullanılır. Çıktı hareketi, `--output` ile verilen kök ad kullanılarak Kimodo [çıktı biçiminde](CIKTI-BICIMLERI.md) kaydedilir. G1 ya da SMPL-X modeliyle üretim yapıyorsanız MuJoCo qpos CSV dosyası ve AMASS NPZ biçimi gibi başka çıktı biçimlerine de kaydedebilirsiniz.

Üretimden sonra Kimodo NPZ, AMASS NPZ, SOMA BVH ve G1 MuJoCo CSV arasında **çevrim dışı dönüştürme** için `kimodo_convert` komutunu kullanın ([Hareket biçimi dönüştürme](HAREKET-BICIMI-DONUSTURME.md) belgesine bakın).

**Çok istemle üretim:**

Bir dizi metin isteminden üretim, noktayla ayrılmış birden çok cümle ve bunlara karşılık gelen sürelerle yapılır:

```bash
kimodo_gen "A person walks forward. A person is walking backwards." \
    --duration "5.0 4.0" \
```

Bu komut Kimodo ile her istemi sırayla üretir. İlk üretilen hareketle sürekliliği sağlamak için ikinci istemin başına kısıtlar eklenir. Art arda gelen hareketleri harmanlamak için kullanılan kare sayısını `--num_transition_frames` argümanıyla denetlersiniz.

**Kısıtlı tek istem:**

Üretimi bir kısıt JSON dosyası vererek kısıtlayabilirsiniz ([Kısıt biçimi tanımına](KISIT-JSON-BICIMI.md) bakın).

```bash
kimodo_gen "A person walks forward and picks something up from the ground." \
    --model Kimodo-SOMA-RP-v1 \
    --duration 5.0 \
    --constraints kimodo/assets/demo/examples/kimodo-soma-rp/03_full_body_keyframes/constraints.json
```

Kısıt dosyalarını etkileşimli demoda oluşturup kaydedebilir ya da [kısıt biçimi rehberine](KISIT-JSON-BICIMI.md) göre elle tanımlayabilirsiniz.

## Çıktı biçimleri

Çıktı biçimlerinin tüm ayrıntıları için [bu sayfaya](CIKTI-BICIMLERI.md) bakın.

Bu biçimler arasında çevrim dışı dönüştürme için [Hareket biçimi dönüştürme](HAREKET-BICIMI-DONUSTURME.md) (`kimodo_convert`) belgesine bakın.

CLI üretimi tüm biçimler (NPZ, AMASS NPZ, CSV ve BVH) için tek bir **çıktı kök adı** (`--output`) kullanır. Örnek sayısına göre **tek dosya** ya da **dosyalarla dolu bir klasör** yazar:

- **Tek örnek** (`--num_samples 1`): her biçim için kök adda tek bir dosya yazar (ör. `--output test` → `test.npz`, `test.csv`). Klasör oluşturulmaz. SMPLX için AMASS, `test_amass.npz` dosyasına yazılır.
- **Birden çok örnek**: o kök adla bir klasör oluşturur ve her örnek için `_00`, `_01` gibi son eklerle birer dosya yazar (ör. `--output test` → `test/test_00.npz`, ...).

Aynı kök adla BVH de (yalnız SOMA) dışa aktarmak için `--bvh` bayrağını kullanın.

### Çıktı dinlenme pozu

SOMA tabanlı Kimodo modellerinde hareketler iki farklı dinlenme pozuna (rest pose) göre dışa aktarılabilir. `NPZ` biçiminin her zaman kullandığı varsayılan dinlenme pozu, SOMA modelinin kanonik T-pozuyla tutarlı standart bir T-pozdur. `BVH` çıktılarında varsayılan dinlenme pozu standart olmayan bir pozdur, ama [BONES-SEED veri setinin](https://huggingface.co/datasets/bones-studio/seed) BVH biçimiyle tutarlıdır. Dinlenme pozu standart T-poz olan bir `BVH` dosyası çıkarmak için `--bvh_standard_tpose` seçeneğini kullanın.

Kimodo'nun kullandığı standart T-poz, [depo varlıklarında](https://github.com/nv-tlabs/kimodo/tree/main/kimodo/assets/skeletons/somaskel77) bir BVH dosyası olarak bulunur.

## Üretilen hareketleri görselleştirme

CLI ile üretilen hareketleri demo arayüzünde görselleştirebilirsiniz. Bunun için "Load/Save" > "Motion" altında üretilen npz çıktı dosyasının yolunu yazın, sonra görüntüleyiciye yüklemek için "Load Motion" düğmesine tıklayın. Üretirken kısıt kullandıysanız onları da benzer şekilde yükleyebilirsiniz.

## Argümanlar

Tüm bayrakları görmek için `kimodo_gen --help` çalıştırın. Bayraklar şunlardır:

- `prompt`: İstenen hareketin metin tanımı (zorunlu)
- `--model`: Kullanılacak model adı (varsayılan: `Kimodo-SOMA-RP-v1`; seçenekler [bu tablodaki](HIZLI-BASLANGIC.md#genel-bakış-kimodo-modelleri) modellerdir)
- `--duration`: Saniye cinsinden hareket süresi (varsayılan: `5.0`). Birden çok istem için
  boşlukla ayrılmış süreleri tek bir dize olarak verin.
- `--diffusion_steps`: Gürültü giderme (denoising) adımı sayısı (varsayılan: `100`)
- `--num_samples`: Üretilecek hareket çeşidi sayısı (varsayılan: `1`)
- `--num_transition_frames`: İstemler arasında harmanlama için kullanılan kare sayısı (varsayılan: `5`)
- `--constraints`: Kısıtları içeren JSON dosyasının yolu
- `--output`: Çıktı kök adı (varsayılan: `output`). Tüm biçimler (NPZ, AMASS NPZ, CSV, BVH) için kullanılır. Tek örnekte her biçim için tek bir dosya yazar (ör. `test.npz`, `test.csv`). Birden çok örnekte bir klasör oluşturur ve içine `test_00.npz`, `test_01.npz`, … yazar. Tek örnekli SMPLX'te AMASS, ana NPZ'nin üzerine yazmamak için `stem_amass.npz` dosyasına yazılır.
- `--save_example_dir`: Verilirse çıktıları Kimodo demosunda yüklenebilen bir "example" dizin yapısına kaydeder.
- `--bvh`: İsteğe bağlı bayrak. Verildiğinde `--output` ile aynı kök adı kullanarak BVH de dışa aktarır (yalnız SOMA modelleri).
- `--bvh_standard_tpose`: BVH dışa aktarılıyorsa, dinlenme pozu BONES-SEED veri setiyle tutarlı poz yerine standart T-poz olacak şekilde dışa aktarır.
- `--seed`: Yeniden üretilebilir sonuçlar için tohum değeri
- `--no-postprocess`: Son işlemeyi (ayak kayması temizliği ve kısıt eniyilemesi dahil) kapatır
- `--input_folder`: meta.json ve isteğe bağlı constraints.json içeren klasör. Verilirse üretim ayarları meta.json'dan yüklenir. Bu dosyalar demo örnek klasörlerinde bulunur.
- `--cfg_type`: Sınıflandırıcısız yönlendirme (classifier-free guidance) kipi: `nocfg`, `regular` ya da `separated` (metin ve kısıt ölçekleri bağımsız olan özel kip). Aşağıdaki [Sınıflandırıcısız yönlendirme (ayrıntılar)](#sınıflandırıcısız-yönlendirme-cfg) bölümüne bakın.
- `--cfg_weight`: `regular` CFG için tek bir ondalık sayı, `separated` CFG için iki ondalık sayı `[text_weight, constraint_weight]`. Yalnız ağırlık verirseniz (`--cfg_type` olmadan) tek değer `regular`, iki değer `separated` anlamına gelir. `nocfg` ile kullanılmaz.

### Sınıflandırıcısız yönlendirme (CFG)

CLI, [Üretim parametreleri](URETIM-PARAMETRELERI.md) belgesindeki Python API'sini yansıtır: Kimodo standart CFG'yi (`regular`) ve metin ile kısıtlar için iki ölçekli **ayrık** (separated) bir çeşidi destekler. Bu projede olağan ayar ayrık çeşittir.

**Kurallar:**

- `nocfg`: ağırlık yok, `--cfg_weight` vermeyin.
- `regular`: `--cfg_weight` ardından tam olarak bir değer verin.
- `separated`: `--cfg_weight` ardından tam olarak iki değer verin.

**Komut satırında `--cfg_type` ya da `--cfg_weight`** verirseniz, `--input_folder` kullanırken bu değerler `meta.json` içindeki her `cfg` bloğunun yerini alır. İki bayrağı da vermezseniz `meta.json` CFG'yi yine `cfg.enabled`, `cfg.text_weight` ve `cfg.constraint_weight` ile sağlayabilir (etkileşimli demo örnekleriyle aynı yapı). CLI'da CFG yoksa ve meta içinde `cfg` yoksa model kendi yerleşik varsayılanlarını kullanır.

Örnekler:

```bash
# No classifier-free guidance
kimodo_gen "A person walks." --cfg_type nocfg

# Standard CFG (single scale)
kimodo_gen "A person walks." --cfg_type regular --cfg_weight 2.5

# Separated CFG (text scale, then constraint scale)
kimodo_gen "A person walks." --cfg_type separated --cfg_weight 2.0 1.5

# Infer mode from arity: one float -> regular; two floats -> separated
kimodo_gen "A person walks." --cfg_weight 2.0 2.0
```

## Python API
Kimodo'nun Python API'sini doğrudan kullanmak isterseniz onu tanımak için `kimodo/scripts/generate.py` betiği iyi bir başlangıç noktasıdır. Model API'sinin tamamı [API belgelerinde](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/api_reference/index.rst) anlatılır.

Kimodo'yu başka bir projede kullanmak isterseniz onunla şöyle çalışabilirsiniz:

```python
from kimodo import load_model

model = load_model("kimodo-soma-rp", device="cuda")
output = model(
    prompt="A person jumps",
    num_frames=150,
    num_denoising_steps=100,
)
```

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
