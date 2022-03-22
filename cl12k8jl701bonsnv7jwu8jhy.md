## Local NuGet Server Kurulumu ve Kullanımı

Merhabalar,

Uzun bir aradan sonra, yakın zamanda öğrendiğim, benim için de öğretici ve eğlenceli bir konu olan NuGet Server kurulumu ve kullanımından bahsetmek istedim.

Çoğumuzun bildiği üzere NuGet, .NET için bir paket yönetim sistemidir. [**nuget.org**](https://www.nuget.org/) üzerinden ister var olan paketleri kullanabilir, istersek de kendi paketlerimizi herkese açık bir şekilde yükleyebiliriz.

Peki ya herkese açık bir şekilde değil de kurum içinde kalmasını istediğimiz paketleri ne yapacağız, nasıl yöneteceğiz? İşte tam da bu ihtiyaç doğrultusunda NuGet Server imdadımıza yetişiyor.

> **Dikkat**: Başlamadan şunu not düşmeliyim ki; bu anlatım **.Net Framework 4.6 ile çalışır.** Eğer NuGet Server’ı .Net Core üzerinde ayaklandırmak isterseniz Google da BaGet’i aratırsanız ihtiyacınız görülecektir.

Haydi başlayalım ve toplamda 5 aşamadan oluşan adımlarımızı tek tek uygulayalım.

[**1-** NuGet Server uygulamasının oluşturulması  
](#dc2b)[**2-** IIS’te NuGet Server’ın yayınlanması](#57f1)  
[**3-** nuget.exe’nın Ortam Değişkenlerine eklenmesi](#4f77)[**4-** Kendi NuGet’imize paket yükleme](#52f3)  
[**4.1-** Publish yöntemi](#37dd)  
[**4.2**\- DLL yöntemi](#e3bd)  
[**5-** Gerekli ön ayarların yapılması ve paketimizin kullanımı](#a97f)

### **Adım 1: NuGet Server Uygulamasının Oluşturulması**

Yeni bir uygulama oluşturuyoruz ve **ASP.NET Web Application(.Net Framework)**’u seçiyoruz, özellikle .Net Framework 4.6 seçilmesi gerektiğini tekrar hatırlatmak isterim. Bir sonraki ekranda **Empty’i** seçerek uygulamamızı oluşturma aşamasını tamamlıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972051041/5oXFwJsPp.jpeg)

Uygulamamız oluştuktan sonra Solution’a sağ tıklayıp **NuGet Package Manager**’a tıklıyoruz ve arama alanına **NuGet.Server** yazıyoruz. Akabinde ilk çıkan paketi yüklüyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972052580/PIkcA8O4C.jpeg)

Paket yüklemesi tamamlandıktan sonra uygulamamızı çalıştırıyoruz. Eğer aşağıdaki görüntüyü görebiliyorsanız her şey yolunda demektir. Paket yöneticimizi neredeyse publish etmeye hazırız.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972054237/JTZtaUr9y.png)

Publish etmeden önce **web.config** içerisinde küçük bir ayar yapmamız gerekiyor. NuGet server üzerinde **nuget.exe** (birazdan değiniyor olacağım) ile push ve delete komutlarını çalıştırmak istendiğinde bir **Api Key** zorunlu olması için **requireApiKey** parametresi halihazırda **true** olarak geliyor, tek yapmamız gereken **apiKey** parametresini bulup parolamızı belirlemek.

> **Unutulmamalıdır ki bu public bir key, edinen herkes her işlemi yapabilir.**

<add key="apiKey" value="123456CokZor" />

Dünyanın en zor parolasını da belirledikten sonra artık publish işlemine geçebiliriz.

### Adım 2: IIS’te NuGet Server’ın Yayınlanması

Eğer ikinci adıma geçebildiysek zaten oldukça kolay olan ilk adımı geçebilmişizdir diye tahmin ediyorum. Vakit kaybetmeden uygulamamıza sağ tıklayıp IIS’te yayınlamak üzere seçtiğimiz klasöre uygulamamızı publish ediyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972055852/TIFp8fxzr.png)

Seçilen klasöre uygulamamızı publish işlemi tamamlandıktan sonra IIS tarafı için gerekli adımları uygulayabiliriz.

> Eğer bir sunucuda değil de bilgisayarımızda bu kurulum işlemini gerçekleştireceksek ve bilgisayarınızda **IIS yok ise Google’a IIS kurulumu yazmak yeterli olacaktır.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972057384/Rj6IZedRI.png)

IIS’i açtıktan sonra **Siteler**’e sağ tıklayıp **Web Sitesi Ekle** diyerek yukarıdaki şekilde gerekli ayarlamaları tamamlayıp **Tamam** dememiz gerekmekte.

Bu işlemi tamamladıktan sonra; Local NuGet Serverımıza bir isim ile erişmek istediğimiz için(tabii ki zorunluluk değil localhost kalabilir.) **Host** dosyasında küçük bir ayar yapmamız gerekiyor, bunun için bu adrese gidip -> **C:\\Windows\\System32\\drivers\\etc** yolu üzerinden **Host** dosyamızı herhangi bir düzenleyici ile açıp şu ayarlamayı yapıyoruz;

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972058903/__Ao96YBS.png)

Yazmamız gereken : 127.0.0.1 nugetserver

Host dosyamıza **127.0.0.1 nugetserver** eklemesini yaptıktan sonra eğer her şey yolunda gittiyse [**http://nugetserver/**](http://nugetserver/) adresinden aşağıdaki görüntüyü alabiliyor olmamız gerekir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972060195/HQbIDZjie.png)

### **Adım 3: NuGet.exe’nın Ortam Değişkenlerine Eklenmesi**

[**Bu adres**](https://www.nuget.org/downloads) üzerinden **NuGet.exe** dosyasını bilgisayarımıza indirip, istediğimiz herhangi bir yere kopyalıyoruz.

> Bu noktada kişisel tavsiyem Program Files’ın altına Nuget adında bir klasör oluşturup bu klasörün altına kopyalamak olacaktır. **(C:\\Program Files\\Nuget)**

Daha sonra Başlat’a gelip “Ortam değişkeni” deyip (eğer İngilizce ise sisteminiz “Environment”) ilk çıkan seçeneğe tıklıyoruz ve **Path kısmına nuget.exe’nin yolunu tanımlıyoruz.** Bu sayede komut isteminde nuget.exe’yi kullanılabilir hale getiriyoruz. Aşağıda görsel olarak aktarmaya çalıştım. Ok yönünde, sırasıyla adımları takip etmemiz yeterli olacaktır.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972061687/dhydzu5DW.png)

**Çalışıp çalışmadığını anlamak için;** komut satırına “nuget” yazdığımızda verilebilecek komutların listesi geliyorsa her şey yolunda demektir. Aksi bir durumda bilgisayarı yeniden başlatmak ya da yolu tekrar gözden geçirmek gerekebilir.

### **Adım 4:** Kendi NuGet’imize Paket Yükleme

Evet keyifli kısımlara gelmeye başladık. Şimdi IIS’te emrimize amade bir paket yöneticimiz olduğuna göre, hem küçük bir örnek teşkil etmesi açısından hem de daha sonra kullanmak üzere bir paket yükleyelim.

Örneğimizi çok basit tutacağım. Hello World döndüren string metot barındıran bir **Class Library (.NET Standard)** olacak. Vakit kaybetmeden oluşturmaya başlayalım. Genel görünüm aşağıdaki gibi olmalı.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972063380/fXSCKAo7f.png)

Uygulamamızı oluşturduktan sonra, kendi paket yöneticimize yüklemek için birden fazla yöntem mevcut. Build ettikten sonra **DLL üzerinden kendimiz paket oluşturabiliriz** ya da direk projemizi **publish ederek** Visual Studio’nun bu işi bizim için yapmasını sağlayabiliriz. Publish yönetimi daha kolay gibi gelse de uzun vadede **paket sayıları ve güncellemeleri arttıkça daha zahmetli bir hal alacaktır.**

#### Adım 4.1: Publish Yöntemi

Projemize sağ tıklayıp **Properties**’i seçiyoruz. Açılan sekmede **Package** kısmına geliyoruz. **Burası zaten hali hazırda otomatik doluyor** açıklama eklemek istersek, icon tanımlamak istersek ya da ilerde sürüm numarasını değiştirmek istersek bu alandan yapıldığını bilmemiz gerekiyor. Özetle paketimiz ile ilgili tüm değişiklikleri buradan yapabiliyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972064671/86CINOe0f.png)

Yukarıdaki görsel zaten hali hazırda default değerlerle otomatik olarak dolmuş bir şekilde geliyor. Bilgi amaçlı bu detayları verdim. Bizim tek yapmamız gereken projemize sağ tıklayıp Publish etmek.

Publish işlemi tamamlandıktan sonra sıra geldi paketimizi yayınlamaya. Publish klasörüne gidip **cmd yazıp enter** diyerek komut istemi başlatıyoruz.(doğrudan o yol ile açılacaktır.)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972065995/ssB2DcxHZ.png)

Komut istemi çalıştıktan sonra, aşağıdaki kodun ilgili alanlarını doldurup enter dememiz yeterli olacaktır. Kodun hemen altında aşağıda bir örnek görüntü paylaştım.

nuget push **PAKET\_ADI.1.0.0**.nupkg -source [**D**](http://nugetserver)**OMAIN** -ApiKey **PUBLIC\_KEY**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972067250/XFgTJ8GXB.png)

#### Adım 4.2: DLL Yöntemi

Öncelikle bu yönteme geçmeden, IIS’ten ilgili yola ulaşıp bir önceki yöntem ile oluşturduğum paketi sildim. (Silmeden de sadece sürüm numarasını yükselterek ya da yeni bir isim vererek de devam edilebilir)  
Şimdi yapmamız gereken uygulamamızı release modda derlemek. Derleme tamamlandıktan sonra projemize sağ tıklayıp **Open Folder in File Explorer** diyerek uygulamamızın yolunu açıyoruz ve **bin/release** klasörünün içinden DLL dosyamızı sabit bir yere alıyoruz. _(aşağıda bonus olarak ekleyeceğim otomatikleştirme aşamasında bu sabit yer önem arz ediyor olacak)._

Paket haline getireceğimiz DLL’imizi, taşıdığımız klasörün adres yoluna **cmd** diyerek, komut istemini başlatıyoruz ve sırasıyla aşağıdaki komutları çalıştırıyoruz.

nuget spech **PAKET\_ADI**

Bu komut bize **MyPackage.nuspec** uzantılı bir dosya oluşturacak. Oluşan bu dosya ne diyecek olursak, publish yöntemindeki kullandığımız paketin özelliklerini belirlediğimiz Package sekmesinin XML hali. Bu dosyayı herhangi bir editör ile açıp yine logo, sürüm vb. özelliklerini düzenleyebiliriz.

nuget pack **PAKET\_ADI**.nuspec

Bu komut bize push edeceğimiz dosyayı oluşturuyor. Push etmeden önce yapacağımız değişiklikler varsa eğer, bu dosyayı editör ile açıp yapmamız gerekiyor.

nuget push **PAKET\_ADI.1.0.0**.nupkg -source [**D**](http://nugetserver)**OMAIN** -ApiKey **PUBLIC\_KEY**

Ve son olarak bu komut ile paketimizi nuget server’ımıza aktarıyoruz.

> Eğer çok çok fazla test vs yaptıysak cache temizlemekte fayda var komut istemcisinde aşağıdaki kodu çalıştırmak yeterli olacaktır.  
> **nuget locals all -clear**

### **Adım 5:** Gerekli Ön Ayarların Yapılması ve Paketimizin Kullanımı

Oluşturduğumuz paketi kullanmak oldukça kolay, klasik nuget.org ile birebir aynı. Fakat bizim kendi NuGet Server’ımızı kullanabilmek için öncesinde küçük bir ayar yapmamız gerekiyor. Bunun için NuGet Package Manager’a gelip **aşağıdaki adımları sırasıyla ok yönünde yapıyoruz.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972068578/PvSfB7opW.png)

Ok dedikten sonra artık projelerimizde kendi paket yöneticimizi kullanabileceğiz. Hemen hızlıca test etmek adına bir konsol uygulaması oluşturuyorum. Sonrasında NuGet Package Manager’ı açıyorum ve **Package source’ta biraz önce oluşturduğum kendi paket yöneticimi seçiyorum.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972069856/_HprpAC6L.png)

Paketi indirdikten sonra, geriye kalan tek şey kullanmak oluyor. Aşağıda küçük bir kullanım örneği mevcut.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972071080/cMZzJYC4M.png)

Şimdi Adım 4.2 de bahsettiğim DLL yöntemini nasıl otomatikleştireceğimizden bahsedeceğim.

Aşağıya scriptin github reposunu iliştiriyorum. README.md dosyasına gerekli açıklamaları adım adım ekledim. İlgili adımlar sırasıyla takip edildiğinde sorunsuzca çalışmasını bekleriz, eğer bir aksilik olursa bana linkedin üzerinden ulaşabilirsiniz.

[hakanyalitekin/AutoPublishNugetPackage](https://github.com/hakanyalitekin/AutoPublishNugetPackage)

Bu yazımda ve otomatikleştirme konusunda desteğini esirgemeyen [Mehmet Aktaş](https://medium.com/u/f790d19fcd1d) ve yazım denetimi için [Birol Gökkaya](https://medium.com/u/2236ea4ed366)’ya özel teşekkürlerimle :)

Umarım okuyan herkese ufak da olsa faydam dokunmuştur. Bir sonraki yazımda görüşmek üzere.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972072375/tveYULWNaV.gif)