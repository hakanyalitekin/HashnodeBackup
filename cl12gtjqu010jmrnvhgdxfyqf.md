## Ngrok ile Localhost’u Dış Dünyaya Açmak

#### 💡 Ngrok Nedir?

Şöyle hayal edelim, kendi lokalimizde bir şeyler geliştiriyoruz ve bir arkadaşımızdan fikir almamız gerekti, müşteri ilerlemeyi görmek istedi ya da geliştirmek istediğimiz web uygulamasının gerçektende mobil bir telefonda nasıl göründüğünü merak ettik. Yayınlama süreçleriyle uğraşmak istemiyor olabiliriz ya da henüz altyapımız hazır olmayabilir.

Örnekleri arttırmak pek tabii mümkün.

🌍İşte böylesi senaryolarda hayatımızı kolaylaştıracak uygulamamızın adı **ngrok.** Saniyeler içerisinde kendi lokalimizdeki bir uygulamayı dünyanın herhangi bir yerinden erişilebilir hale getirebiliriz. Hatta şifre bile koyabiliriz. Tek yapmamız gereken hangi portun izleneceğini söylemek.

📢 Ngrok kendisini şöyle tanıtıyor, NAT’ların ve güvenlik duvarlarının arkasındaki yerel sunucuları güvenli tüneller üzerinden tüm dünyada erişilebilir hale getirir.

> NAT (Network Address Translation — Ağ Adresi Dönüştürme) Ağ / IP maskelemeyle birlikte, bir adres uzayını gizlemek için kullanılan teknik bir terimdir. — [wikipedia](https://www.wikiwand.com/tr/NAT)

#### 🧑‍💻Haydi Deneyelim

Öncelikle [**bu adres**](https://ngrok.com/) üzerinden resmi sitesine gidip bir hesap açmamız gerekiyor. Akabinde bizi bir dashboard ekranı karşılıyor. İşletim sistemimize uygun olan versiyonu indiriyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971938037/j_rMrsaqZ.png)

Zaten dashboard ekranını açılır açılmaz bizi sırasıyla izleyeceğim adımlar karşılıyor.

#### 1.Adım

Zip dosyasını istediğimiz herhangi bir konuma çıkarıyoruz. Uygulamamızı çalıştırıyoruz. Yayın süresince de açık bırakmalıyız aksi halde yayın sonlanır.

#### 2.Adım

Yine dashboard ekranında hali hazırda bulunan authtoken bilgisini alıp komut satırında çalıştırıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971939394/Du5jnZeLc.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971940917/5YFjfWaqh.png)

authtoke save to configuration file gibi bir mesaj alıyorsak buraya kadar her şey yolunda demektir.

#### 3.Adım

Oturumumuzu doğruladığımıza göre, şimdi tek yapmamız gereken ilgili portu dışarıya açmak kaldı.

Ben örnek bir .Net 5 API’ı oluşturdum ve default’ta swagger gelsin dedim. 5001 portunda ayağa kaldırdım.

```
ngrok http -host-header=localhost [https://localhost:5001](https://localhost:44313/)

##Eğer şifre koymak istersek auth tag'ini eklememiz gerekiyor##  
ngrok http -auth="deneme:1234" -host-header=localhost [https://localhost:5001](https://localhost:5001)
```

Yukarıdaki kodu çalıştırdığımız da aşağıdaki gibi bir görüntü elde ediyor olmamız gerekiyor. Bu artık yayında olduğumuz anlamına geliyor.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971942176/PMSI-AcTM.png)

Hem httphem de https olarak sunulan linklerde herhangi birine tıklıyoruz ve uygulamamızın çalışıp çalışmadığını teyit ediyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971943550/uZZNvm1T2.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971945023/u5k3P1_ju.png)

Her şey yolunda görünüyor; istek ayrıntılarını incelemek istersek ngrok’un sunduğu arayüze [**http://localhost:4040/**](http://localhost:4040/) üzerinden göz atabiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971946448/p036Eto1p.png)

**📝Özetleyecek olursak;**

1.  [ngrok](https://ngrok.com/)’a kaydol ve güncel sürümü indir
2.  Bir token al.
3.  ngrok’u başlat ve şu komutu çalıştır ```ngrok authtoken YOUR\_AUTHTOKEN```
4.  Bir tünel oluştur: ```ngrok http -host-header=localhost https://localhost:5001``` 

Bitirmeden önce şunu belirtmeliyim ki **ngrok ücretli bir uygulamadır**, ücretsiz özellikleri kısıtlıdır. Ücretli ve ücretsiz versiyonlar arasında farklılıkları pricing sekmesinden inceleyebilirsiniz.

Ayrıca; ngrok en kolay ve en iyi bilinen **localhost**tünel açma hizmetidir, ancak alternatif seçenekler mevcuttur:

*   [**LocalXpose**](https://localxpose.io/) : ücretsiz seçeneklere sahip ticari bir hizmet. Kayıt gereklidir, ancak terminal tabanlı ve grafik kullanıcı arayüzü mevcuttur.
*   [**localhost.run**](http://localhost.run/) : SSH üzerinden çalışan ücretsiz bir hizmettir, bu nedenle istemci veya kayıt gerekmez.
*   [**localtunnel**](https://localtunnel.github.io/www/) : açık kaynaklı bir Node.js istemcisi. Kayıt gerekli değildir.
*   [**JPRQ**](https://github.com/azimjohn/jprq) : açık kaynaklı bir Python istemcisi. Kayıt gerekli değildir.
*   [**sish**](https://github.com/antoniomika/sish) : açık kaynaklı, Docker tabanlı bir kapsayıcı istemcisi. Kayıt gerekli değildir.

Umarım ufakta olsa faydam dokunmuştur. Bir sonraki yazıda görüşmek üzere.

[ngrok Step-by-Step Guide: Easily Share Your Local Server - SitePoint](https://www.sitepoint.com/use-ngrok-test-local-site/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971948487/XuDoVtgQO.gif)