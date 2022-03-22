## Medium’daki yazıları Dev.to ile senkronize etmek

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647980503849/wXIlYL8fu.jpeg)

Nedeni bir türlü anlayamadığım Medium’un yükselişinden sonra ben de dahil bir çok geliştirici, gerek insanlarla paylaşımda bulunmak gerek Türkçe kaynak oluşturmak için bildiklerini Medium üzerinden aktarmaya başladı.

Medium, düz yazılar paylaşmak güzel bir yer olabilir belki ama kod paylaşımı için hiç de kullanışlı olmayan, gist gibi ekstra zahmetlere katlanmamıza sebep olan bir yer.

Bu yazımda yaklaşık olarak iki yıldır takip ettiğim [**dev.to**](https://dev.to/)’dan ve Medium ile nasıl senkronize edebileceğimizden bahsedeceğim.

**Burada ki temel maksadım dev.to’nun da Türkçe kaynak olarak güçlenmesine katkıda bulunmak** ve ilk etapta Medium da ki trafikten vazgeçmek istemeyen kullanıcıların yazılarını dev.to’da zahmetsizce paylaşmasının yolunu açmak.

**Dev.to**, **ilan** yayınlamaktan tutunda **blog**, **podcast**, **video** gibi bir sürü paylaşımı içinde barındıran, geliştiriciler bir topluluk yaratmayı hedefleyen açık kaynak kodlu bir platformdur.

[forem/forem](https://github.com/forem/forem)

Dev.to kendini şu şekilde tanımlıyor,

> DEV, birbirlerine yardım etmek için bir araya gelen yazılım geliştiricilerinden oluşan bir topluluktur. Yazılım endüstrisi, işbirliğine ve ağa bağlı öğrenmeye dayanır. Bunun olması için bir yer sağlıyoruz.

[About DEV - DEV Community](https://dev.to/about)

### Gelelim senkronizasyon meselesine;

İnternette arattığımda nedendir bilmiyorum oldukça zahmetli yöntemler ile karşılaştım, biraz daha araştırınca bunu RSS akışı ile zahmetsizce yapılabileceğini gördüm. Sırasıyla aktarıyorum.

#### **Adım 1: Aktarımı sağlamak**

Profil resmimizin üzerine gelip menüden, **Settings’e** tıklıyoruz. Açılan sayfada sol menüden **Extensions** tıklıyoruz ve **Publishing to DEV from RSS** bölümüne geliyoruz.

Sonrasında **RSS Feed URL** başlığını göreceğiz, buraya Medium RSS akışımızı eklememiz gerekiyor.

> Medium RSS akışımız şu şekilde -> https://medium.com/feed/@**KullanıcıAdı**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647980504965/N9d_kE3xu.png)

Kaydetme işlemini tamamladıktan sonra **Fetch feed now** diyerek saniyeler içinde Medium akışımızın aktarımı tamamlıyoruz.

**Artık tek yapmamız gereken yazılarımızı yayına almak.**

#### Adım 2: Yayına almak

Taslak olarak eklenen yazılarımızı yayına almak için yine profil resmimizin üzerine gelip açılan menüden **Dasboard**’a tıklıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647980506203/heho88Hqs.png)

Burada başarıyla aktarımı gerçekleştirilmiş olan yazılarımızı görebiliyor olmamız gerekiyor.

**Edit** ile yazımızın içerisine girip **published: false** olan kısmı **true** olarak ayarlayıp kaydetmek. **Tüm işlem bu kadar.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647980507914/E1Hc_e413.png)

Bu yazı her ne kadar Medium özelinde görünse de aslında RSS akışı olan tüm bloglardan aktarım sağlayabiliriz.

Umarım Türkçe kaynağın desteklenmesine ve okuyan herkese ufak da olsa faydam dokunmuştur.

Bir sonraki yazımda görüşmek üzere.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647980509418/B7J3rEJhu.gif)