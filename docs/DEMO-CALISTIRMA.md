> Bu belge nv-tlabs/kimodo deposundaki `docs/source/interactive_demo/launching.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Demoyu çalıştırma

Kurulum [yönergelerini](KURULUM.md) izledikten sonra demoyu aşağıdaki komutlarla başlatabilirsiniz. Demo web tarayıcısında [http://localhost:7860](http://localhost:7860) adresinde çalışır.

<details>
<summary>Demoyu bir sunucuda çalıştırıyorsanız ona erişmek için bağlantı noktası yönlendirmesi kullanabilirsiniz.</summary>

Uzak bir sunucuda çalışan demonun web arayüzüne erişmek için SSH bağlantı noktası yönlendirmesi kurun. Böylece web tarayıcınız `http://localhost:7860` adresine yereldeymiş gibi ulaşır.

**Seçenek 1: SSH yapılandırmanıza LocalForward ekleyin**

SSH yapılandırma dosyanızı düzenleyin (ya da oluşturun) (genellikle `~/.ssh/config`):

```
Host your-server-name
    HostName your.server.address
    User username
    LocalForward 7860 localhost:7860
```
Sonra şu komutla bağlanın:
```
ssh your-server-name
```

**Seçenek 2: SSH komut satırını doğrudan kullanın**

Yerel makinenizde şunu çalıştırın:
```
ssh -N -L 7860:localhost:7860 username@your.server.address
```
Bu komut yerel 7860 bağlantı noktanızı sunucunun 7860 bağlantı noktasına yönlendirir.
Bağlandıktan sonra web tarayıcınızda [`http://localhost:7860`](http://localhost:7860) adresini açın.

`username` ve `your.server.address` değerlerini kendi kullanıcı ve sunucu bilgilerinizle değiştirin.

</details>
<br>

Demoyu sık sık yeniden başlatacaksanız önce [hızlı başlangıç rehberinde](HIZLI-BASLANGIC.md#metin-kodlayıcı-hizmetini-çalıştırın) anlatıldığı gibi metin kodlayıcı hizmetini arka planda başlatmanızı öneririz. Metin kodlayıcı hizmeti çalışmıyorsa demo metin kodlayıcı modelini kendisi otomatik yükler.

Demo, açılışta ve arayüzde model tercihi değiştirildiğinde gerektikçe Kimodo model kontrol noktasını da otomatik indirir.

## Komut satırından başlatma
Kimodo'yu paket olarak ya da kaynaktan kurduysanız demoyu şu komutla başlatın:
```bash
kimodo_demo
```

## Docker ile başlatma
Docker ile kurduysanız demoyu şu komutla başlatın:
```bash
docker compose up demo
```

<details>
<summary>Docker için ek ipuçları</summary>

Kimodo'yu Docker kapsayıcıları içinde çalıştırıyorsanız şu komutlar işinize yarar. Aşağıdaki örnek komutlarda `demo` yerine `text-encoder` da yazabilirsiniz:

**Günlükleri görün:**

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
