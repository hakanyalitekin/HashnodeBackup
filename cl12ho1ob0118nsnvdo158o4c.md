## Detaylı RabbitMQ Anlatımı ve Tüm Exchange Türleriyle Örnekler (Docker/.Net 5)

Merhabalar,

[**Notion.so**](https://www.notion.so/) üzerinde oldukça ciddi emek harcayarak kendim için tuttuğum notları biraz toparlayıp burada yayınlamanın kamu yararı için oldukça faydalı olacağını düşündüm. Umarım yanılmamışımdır.

Çok iddialı konuşup beklentiyi de yükseltmek istemiyorum ama RabbitMQ için Türkçe olarak en kapsamlı kaynağı okuyor olabilirsiniz.🙂

Eğer Exchange tipleri ile ilgili örneklere açıklama satırları eklediysen uğraştırma bizi bir çatal alıp kaçalım, bir ara inceleriz diyen arkadaşlar için doğrudan erişebilecekleri [**GitHub linki.**](https://github.com/hakanyalitekin/BasicRabbitMQ) **🍴**

Eğer notion kullanan arkadaşlar varsa kopyalamaları için ve oradan devam etmeleri için köprüden önceki son çıkış [**burası**](https://www.notion.so/RabbitMQ-9b60e97fc4a14593a2a102dd3f6bc744).🚗

Bu yazıda, Medium’un tekdüzeliğinden biraz olsun arınmak için fazlaca emoji kullandım umarım Kobra Murat’ın evine benzememiştir. 😅

### **📌RabbitMQ nedir?**

RabbitMQ, Erlang dili ile açık kaynak olarak geliştirilen, popüler ve hızlı bir **mesaj-kuyruk** yapısıdır. **Advanced Message Queuing Protocol (AMQP)** protokolünü kullanarak uygulamalar ve sunucular arası veri alışverişini sağlar.

Bu uygulamalar arası iletişim sağlanırken, aynı teknolojileri kullanma zorunluluğu yoktur. Yani mesajı yayınlayan uygulama(Publisher) **Python** diliyle, mesajı dinleyen uygulama(consumer) ise **C#** diliyle yazılmış bir .Net teknolojisi olabilir.

> **💊message broker:** mesaj kuyruk sistemlerine verilen genel bir isimdir.

**RabbitMQ’ya alternatif diğer message broker’lar;**

🔹 Azure Queue Storage  
🔹 Azure Service Bus  
🔹 Kafka  
🔹 MSMQ

### 📌Neden ve nerelerde RabbitMQ kullanmalıyız?

RabbitMQ ya da diğer message broker sistemlerini kullanmak için birçok neden olabilir, bu nedenlerden en ön plana çıkanı tabii ki **kullanıcıyı gereksiz bekleyişlerden kurtarmak** ve bu sayede kullanıcı deneyimini arttırmak.

Biraz daha açacak olursak eğer; çok uzun süren bir PDF işlemi ya da kullanıcının sonucunu çok da önemsemediği geri dönüş mailleri gibi senaryolarda message brokerları kullanmak oldukça mantıklı olacaktır.

**Özetleyecek olursak aşağıdaki senaryolarda RabbitMQ kullanılabilir;**

**🔹**Son kullanıcıyı etkilemeyen arka plan işlemlerinde  
**🔹**Farklı platformlardaki uygulamalar arasında asenkron veri transferi işlemlerinde  
**🔹**Anlık işlemlerin çok önemli olmadığı işlemlerde (Örneğin; siparişiniz yola çıktı maili)  
**🔹**Veri tutarlılığının önemli olduğu işlemlerde (Örneğin; birçok pazaryerinden gelen siparişler sonucu stok miktarının tutarlılığı)  
**🔹**İşlem hacminin büyük olduğu sunucuyu yoran işlemlerde

Örnekleri arttırmak pek tabii mümkün.

### **📌RabbitMQ nasıl çalışır?**

RabbitMQ Publisher ve Consumer mantığıyla çalışır. Yani mesajı üretip kutuya(**queue**) atan ve kutudaki mesajları okuyan olmak üzere iki temel yapı üzerine inşa edilmiştir. Bir de senaryomuza göre bu üretilen mesajların hangi kutuya atılması gerektiğinden sorumlu olan **exchange** yapımız mevcut.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971967681/kdfQ2scTo.gif)

#### **💡Bazı terimleri inceleyecek olursak;**

**🔹Producer/Publisher(**yayıncı**):** Queue'ya mesajı gönderen yapıya verilen isimdir.  
**🔹Exchange(**yönlendirici**):** Routing, yani kuyruğa iletilen mesajı yönlendiren yapı.  
**🔹Queue(**kuyruk**):** First-in-first-out(FIFO) ilk giren ilk çıkar mantığıyla çalışan kuyruk yapısı.  
**🔹Consumer(**tüketici**):** Queue'yu dinleyerek ilgili mesajları teslim alan yapıya verilen isimdir.

**Genel hatlarıyla iş akışı aşağıdaki gibidir;**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971969156/XrbsFeOyd.png)

Bir yayıncı mesajı yayınlar, yönlendirici ilgili mesajı ilgili kuyruğa iletir ve dinleyiciler ilgili kuyruktan dinleme işlemini gerçekleştirir.

> **💊**Producer/Publisher illa ki Exchange kullanmak zorunda değildir. Doğrudan Queue’ya mesajı iletebilir.  
> **💊**Consumer birden fazla kuyruğu dinleyebildiği gibi bir kuyruğu da birden fazla consumer dinleyebilir.

#### **Olası bir senaryo üzerinden inceleyecek olursak eğer;**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971970610/34585avxP.png)

**1️⃣** Kullanıcı PDF oluşturma isteğini iletir.  
2️⃣ Producer ilgili PDF oluşturma isteğini Message Broker’a(RabbitMQ) iletir.  
3️⃣ RabbitMQ ilgili isteği, Exchange ile ya da doğrudan kuyruğa ekler.  
4️⃣ Consumer kuyruktaki PDF oluşturma isteğini alır ve PDF’i oluşturur.

### **📌 RabbitMQ’yu Docker Üzerinde Nasıl Kurabiliriz?**

RabbitMQ ile ilgili çalışabileceğimiz bir çok farklı ortam yaratmak mümkün, [**bu link**](https://www.rabbitmq.com/download.html) üzerinden bir çok kurulum ortamı için bilgi edinilebilir.

🔹**CloudAMQP** **kullanarak;** [https://customer.cloudamqp.com/signup](https://customer.cloudamqp.com/signup) linki üzerinden hesap açarak, maksimum 100 kuyruk ve 28 gün mesaj saklama limitleri dahilinde kullanmak mümkün.

🔹**Docker’a kurulum yaparak;** [https://hub.docker.com/\_/rabbitmq](https://hub.docker.com/_/rabbitmq) linki üzerinden gerekli talimatları takip ederek.

🔹**İşletim sistemine kurulum yaparak;** [https://www.rabbitmq.com/install-windows.html](https://www.rabbitmq.com/install-windows.html) üzerinde ilgili talimatları takip ederek kullanmak mümkün.

#### 💡RabbitMQ’nun Docker Üzerinde Ayaklandırılması

Doğrudan aşağıdaki komut ile RabbitMQ’yu ayaklandırabiliriz.

docker run -d --name myRabbitMQ -p 5672:5672 -p 15672:15672 rabbitmq:3.8.14-management

> 💊İkinci port yönetim paneli için.  
> 💊Eğer management tagını eklemezsek yönetim panelini eklemez. Sadece RabbitMQ’yu kurmuş oluruz.

**Her şeyin sorunsuzca tamamlandığından emin olmak istersek eğer;** [http://localhost:15672/](http://localhost:15672/) adresine gidip açılan ekranda kullanıcı adı ve şifre kısma **guest** yazarak oturum açabiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971972002/X22349PgX.png)

Sisteme giriş yaptıktan sonra bizi, kullanıcılarımızı yönetebileceğimiz, kuyruklarımızı, kanallarımızı vs. gözlemleyebileceğimiz basit bir ara yüz karşılıyor. Devam etmeden önce biraz kurcalamanızı öneririm.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971973291/bH52Wnl-T.gif)

### 📌Exchange Nedir ve Çeşitleri Nelerdir?

En basit tabiriyle yönlendirici diyebiliriz, yani Publisher’dan gelen mesajı ya da mesajları alıp Queue’ya yönlendiren yapı olarak tanımlamak mümkün.

> 💊Exchange Type ile sadece Publisher ilgilenir. Consumer, Queue ile ilgilenir.

Exchange tiplerine ve örneklerine geçmeden önce temel hatlarıyla kullanacağımız yapı aşağıdaki gibi olacak. Ayrıca tüm örneklerde kullanacağımız **tek nuget paketi** **RabbitMQ.Clientolacak.**

İlk oluşturması biraz sıkıcı ve zaman alıcı olsa da ileride hatırlamakta zorlandığımız exchange tip olduğunda bulması ve anlaşılabilir olması benim için önemliydi. Bundan sebep her birini bağımsız **Consumer** ve **Publisher** konsol uygulamaları olarak oluşturmak istedim.

_Evet çok fazla kod tekrarı var kabul ama burada harika bir yapı kurmaya çalışmıyoruz, RabbitMQ’yu öğrenmeye çalışıyoruz :)._

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971974891/SQWniO_G6.png)

Yukarıdaki görselde anlatmaya çalıştığım gibi klasörleri ve ilgili konsol uygulamalarını oluşturduktan sonra ihtiyacımız olan nuget paketini kurduysak eğer **Exchange türlerine geçmeden, öncelikle temel hatlarıyla bir kuyruk okuma-yazma işlemi gerçekleştirelim.**  
Sonrasındaysa Exchange routing yapılandırmalarını tek tek inceleyerek her biri için basit örnekler yapalım.

> 💊 RabbitMQ’da Exchange kullanmak zorunda değiliz, doğrudan Queue’ya mesajlarımızı gönderebilir ve Consumer(lar)’ın dinlemesini sağlayabiliriz.

### 1️⃣Exchange Kullanmadan Doğrudan Queue Kullanım Örneği

Exchange kullanmadan basit bir RabbitMQ örneği yapalım. Daha önceden suda beklettiğimiz **WithoutExchange.Publisher** uygulamamıza geliyoruz ve içeriğini aşağıdaki gibi kodluyoruz.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/4152fe1ea702af0930aaff463408bef2/href">https://medium.com/media/4152fe1ea702af0930aaff463408bef2/href</a></iframe>

Burada neler yaptık. Öncelikle bağlantımızı oluşturup bu bağlantı üzerinden bir kanal oluşturduk. Hemen akabinde **QueueDeclare** metodunu kullanarak herhangi bir yönlendirme(exchange) kullanmadan doğrudan kuyruğumuzu oluşturduk.

**💡Tabi kuyruğumuzu oluştururken belirlememiz gereken bazı parametreler vardı bunlardan biraz bahsedelim;**

**🔹Queue:** Kuyruğumuzun adı. Herhangi bir exchange kullanamadığımız için bu kuyruk adı önemli. Consumer tarafında kuyruğumuza doğrudan ismiyle erişeceğiz.  
🔹**Durable(dayanıklı):** Kelime anlamı dayanıklı olan bu parametre sayesinde kuyruğumuzun nerede saklanacağını belirliyoruz. Eğer true yaparsak kuyruğumuz fiziksel, false yaparsak hafızada saklanır.  
**🔹Exclusive(özel):** Kelime anlamı özel olan bu parametre ile kuyruğumuza farklı kanallar üzerinden erişilip erişilemeyeceğini belirliyoruz. True sadece bu kanal, false farklı kanallar üzerinden de erişilebilir demek. Kuyruğumuza Consumer uygulaması üzerinden, yani farklı bir kanal üzerinden erişeceğimiz için false yapmalıyız.  
**🔹AutoDelete:** Kuyrukta yer alan veri Consumer’a ulaştığında silinip silinmemesi belirtilir.

İşlenmeye hazır elli adet mesajımızın oluştuğunu ara yüzümüzden kontrol edelim.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971976321/0vDdxdAXy.png)

Şimdi gelelim bu oluşturduğumuz mesajların dinleneceği Consumer tarafına. Yapı yine oldukça basit. Öncesinde **WithoutExchange.Consumer**’e kodlarımızı yazalım sonrasında da üzerine konuşalım.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/67f4b5a72428aeefe30cc004be848363/href">https://medium.com/media/67f4b5a72428aeefe30cc004be848363/href</a></iframe>

**Yapılan tüm işlemleri yorum satırı olarak yazdım** fakat yine de Consumer tarafında neler yaptık üstünden geçelim.

Öncelikle standart Publisher tarafında yaptığımız gibi, RabbitMQ’ya bağlandık. Bağlantımızı açtıktan sonra bu bağlantı üzerinden bir kanal oluşturduk.  
Daha sonra bu kanal üzerinden her ihtimale karşı Publisher tarafında oluşturduğumuz kuyruğu, birebir aynı özellikleriyle tekrar oluşturduk. Tabi burası tercih meselesi eğer publisher tarafında kanalımızın oluştuğuna yüzde yüz eminsek Consumer tarafında tekrar oluşturmaya gerek yok.

Hemen akabinde de oluşturduğumuz kanal ile **EventingBasicConsumer** nesnesinden faydalanarak Consumer'ımızı yarattık.

Daha sonra **BasicConsume** metodunu kullanarak Consumer'ın hangi kuyruğu dinleyeceğini belirttik. Burada **autoAck** önemli.

**🔹AutoAck** (Auto Acknowledgement)**:** Mesajın Consumer’a ulaştıktan sonra onaylanıp onaylanmama, haliyle silinip silinmeme durumunu belirlediğimiz parametre. Özetle;  
**Eğer true dersek ->** Mesajın doğru işlenip işlenmediğine bakılmaksızın sil.  
**Eğer false dersek ->** Sen silme ben haber vereceğim sileceğin zaman

Consumer’ı çalıştırdığımızda tüm logs mesajlarımızın işlendiğini görebiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971977591/YO6h5Z1Pk.png)

### 2️⃣Fanout Exchange Tanım ve Örneği

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971978795/QAAaeYE2v.png)

En sade exchange tipimizdir. **Publisher** tarafından gönderilen mesajları alıp route key tanımlaması gibi ayrımlara ihtiyaç duymadan,**Fanout Exchange**’e bind olan bütün kuyruklara aynı ve eşit bir şekilde iletilmesini sağlayan yapıdır.

Consumer‘ın tek yapması gereken kuyruktaki mesajları okuyup işlemektir.

Bu yapıyı radyo yayını yapan bir frekansa ya da Youtube üzerinden yayın yapan bir Youtuber’a benzetmek mümkün. Tüm dinleyiciler ilgili kanala eriştiğinde aynı mesajları alır.

Az çok kafamızda bir şeyler oluştuysa eğer kodlarımızı yazmaya başlayabiliriz.

Bir önceki örneğimizde yaptığımız işlemlere oldukça benzer bir yapı olacak, bundan sebep bir önceki kodlarımızın aynısını alıp daha önceden oluşturduğumuz **FanoutExchange\_Publisher** 'a yapıştırıyorum.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/729a86db217d5c744b1fe2e390ec8074/href">https://medium.com/media/729a86db217d5c744b1fe2e390ec8074/href</a></iframe>

Yukarıdaki kodları biraz açacak olursak; bir önceki **WithoutExchange.Publisher** 'den çok farklı değil.

Önceden mesajlarımızı herhangi bir Exchange kullanmayıp doğrudan Queue’ya gönderdiğimiz için **channel.BasicPublish()** metodunda Exchange tipimizi boş geçip, doğrudan kuyruğumuzun adını yazıyorduk. Şimdi ise tam tersini yapıyoruz.

Artık **QueueDeclare** metodu oluşturmamıza gerek kalmadı. Biz sadece Exchange'imizi tanımlıyoruz(logs-fanout). İsteyen Consumer kuyruğu oluşturup dinlemeye başlayabiliyor.

> 💊Fanout kullanınca illa Consumer tarafında oluşturulacak diye bir şey söz konusu değil, senaryomuza göre burada da oluşturmak pek ala mümkün.

Eğer her şey tamamsa uygulamamızı çalıştırıp RabbitMQ panelini açalım. Gördüğünüz üzere biz tip olarak **Fanout** kullanarak yayın yapmaya başladık. İsteyen Consumer Queue'sunu oluşturup, dinleyebilir ve işlemlerini gerçekleştirebilir.

_Yani Consumer’lar kuyruk oluşturup bizim kanalımıza abone olurlarsa, biz onların posta kutularına mektuplarımızı atabiliriz._

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971980215/JTqJPLE21-.gif)

Gördüğünüz gibi henüz bir bind olan bir consumer yok. Hadi şimdi **FanoutExchange\_Publisher**’a gidip consumer tarafını kodlayalım.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/2a8e69062b318058d9e4bab1394dd77a/href">https://medium.com/media/2a8e69062b318058d9e4bab1394dd77a/href</a></iframe>

İlk Consumer örneğimizde yaptığımız işlemlerin çoğu burada da aynıydı bundan sebep aynı kısımlara yorum satırı eklemeyip değişiklik olan kısımlara açıklama satırları ekledim.

**Bir önceki örneğimizden tek farkı oluşturduğumuz kuyruk ile Exchange’imizi Bind ediyoruz yani bağlıyoruz.**

Şimdi kodladığımız Consumer’ın çalışıp çalışmadığını ve birden fazla Consumer’a aynı mesajın gidip gitmediğini doğrulayalım.

> 💊Bunun için iki adet terminal açabileceğiniz gibi yan yana hizalı görmek için VS Code’un terminali de kullanabilirsiniz. Tek yapmamız gereken terminalde Consumer’ın yoluna gidip **dotnet run** komutunu çalıştırmak

**Burada dikkat edilmesi gereken husus önce Consumer’ın çalıştırılmasıdır.** Eğer önce Publisher’ı çalıştırırsak mesajları iletilecek bir Queue olmadığı için boşa/boşluğa gider ve Consumer(lar)’ı çalıştırdığımızda okuyacak herhangi bir mesaj bulamaz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971981865/TAKJVeQOA.gif)

Mesajlarımız okunurken biz o sıra RabbitMQ’nun paneline gidip, gerçekten de rastgele isimlerle kuyruklarımızın oluşup oluşmadığını ve Fanout Exchange’imize Bind olup olmadıklarını doğrulayabiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971983335/10It09NpM.gif)

Terminal üzerinde çalışan Consumer’larımızı durduğumuz da kuyrukların ve bind’ların otomatik silindiğini görebilirsiniz.

### 3️⃣Direct Exchange Tanım ve Örneği

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971984732/ngc5j8KZr.png)

Fanout’tan farklı olarak şimdi mesajlarımıza bir yol haritası çizeceğiz yani routing key kullanmaya başlayacağız.

**Direct Exchange**'te işleyiş şu şekilde; **Publisher** tarafında mesajımız gönderilirken içerisinde bir **routing\_key** barındırıyor **Direct Exchange** bu key'e bakıyor ve kuyruğumuzla eşleştiriyor. **Consumer** yine aynı key üzerinden gidip ilgili kuyruk ile eşleşip mesajları okuyabiliyor.

Örneğimizi şöyle yapalım, **Error**, **Warning**, **Info** tipinde üç adet hata türümüz olsun. Gerçek hayatta API ya da farklı kanallardan gelir bu hatalar ama biz rastgele üretelim. Yine gerçek hatta belki Error tipinde olanlar bir yere yazılıp, Info ve Warning tipinde olanlar bir dashboard'tan izlenebilir. Biz istediğimiz türe göre konsola yazdıralım.

Bir önceki **Fanout** örneğinde kuyruk deklare etme işini **Consumer** tarafına bırakmıştık. Burada ise **Publisher** tarafında oluşturup, yine bind etme işlemini de burada gerçekleştirelim.

Yine daha önceden oluşturduğumuz **DirectExchange.Publisher** 'a geçip bir önceki örneğimiz olan **FanoutExchange.Publisher**'ı birebir kopyalıyoruz ve aşağıdaki gibi revize ediyoruz.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/3ba187f5a2590a80a596c29a71770b25/href">https://medium.com/media/3ba187f5a2590a80a596c29a71770b25/href</a></iframe>

Gerekli bilgileri yorum satırı olarak ekledim. Kabaca bahsedecek olursam eğer gerçek hayat senaryosuna benzetmek için rastgele üç adet hata tipi ile oluşan mesajlar ve route key’ler ürettik. Bu sayede Consumer tarafında route key yardımı ile isteyen istediği log türünü dinleyebilecek.

Uygulamamızı çalıştırdığımızda aşağıdaki gibi bir konsol görünümü oluşmalı.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971986101/tFOwQm4FxM.png)

RabbitMQ panelinde ise şu şekilde bir görünüm oluşmuş olmalı; üç adet Queue, bir adet log-direct adında ve direct tipinde exchange. Bu oluşan Exchange’imize tıkladığımızda ise oluşan üç kuyruğun bind edilmiş olması gerekir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971987608/4c53hoQl6.gif)

Şimdi Consumer tarafını oluşturalım. Yine **FanoutExchange.Consumer**’i **DirectExchange.Consumer**’a kopyalayıp aşağıdaki gibi revize ediyoruz.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/3bd19eb8a7722af881f624b29178615c/href">https://medium.com/media/3bd19eb8a7722af881f624b29178615c/href</a></iframe>

Yapılan işlem oldukça basit. Her şey zaten Publisher tarafında oluştuğu için, Consumer tarafında tek yapmamız gereken dinlemek istediğimiz kuyruğun adını belirlemek. Fanout’tan farklı olarak ne yapmış olduk mesajlarımızı route key’ler ile ayırmış olduk. Bu sayede isteyen istediğini kanalı/kuyruğu dinleyebiliyor.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971989114/TWxef_-SH.gif)

### 4️⃣Topic Exchange Tanım ve Örneği

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971990453/620l8oTOB.png)

**Topic Exchange** çok daha detaylı routing yapmak istediğimiz zaman kullanılan tiptir. Direct Exchange'ten tek farkı routing key tanımlarken **wildcard**'lar ile beraber belirli pattern'ler kullanabiliyor olmamız.

> **💊 Wildcard:** Türkçedeki karşılığı Özel/Yıldızlı karakterler diye bilinir ve yer tutucular(placeholder) olarak kullanılır.

**Route Key’i** **Publisher tarafında oluştururken** aralara sadece nokta koyarak ayırırız → a.b.c.d

**Route Key’i Consumer tarafında çağırırken** pattern içerisinde bizim için fark etmeyen bir alan ya da alanlar varsa **\***(yıldız) kullanarak geçebileceğimiz gibi sadece başı ve sonuyla ilgileneceksek eğer **#** işareti kullanarak ilgilenmeyeceğimizi belirtebiliriz.

**🔹 a.\*.\*.d** → route key’in başında a ve sonunda d olanları dinlemek için

**🔹 \*.\*.c.\*** → route key’in üçüncü sırasında c olanları dinlemek için.

**🔹 a.# →** route key’in başında a olan ve gerisiyle ilgilenmediğim kuyrukları dinlemek için

**🔹 #.d →** yukarıdakinin tam tersi başıyla ortasıyla ilgilenmeyip sonuyla ilgilendiğimiz senaryolar

**💡Biraz somutlaştırılmış örnek verecek olursa eğer;**  
Publisher tarafında dünyadaki anlık doğum oranlarının raporlarının excel ve pdf formatında **aşağıdaki gibi bir routing key pattern’i kullanılarak** yayınlandığını varsayalım;

**avrupa.fransa.paris.pdf  
avrupa.fransa.paris.excel  
avrupa.almanya.berlin.pdf  
avrupa.almanya.munih.pdf  
asya.cin.pekin.pdf  
asya.cin.pekin.excel**

**Consumer tarafında çağırırken;**

**🔹 #.pdf →** başında ve ortasında ne olduğu önemli değil tüm şehirlerin pdf’leri getir.

**🔹 \*.\*.\*.pdf →** yukarıdaki ile aynı işlemi yapar.

**🔹 avrupa.# →** ortasında ve sonunda ne olduğu beni ilgilendirmez, Avrupa’nın tüm şehirlerinin hem pdf hem de excel’lerini getir.

**🔹 \*.almanya.\*.pdf →** Almanya’nın tüm şehirlerinin doğum raporlarının pdf halini getir.

**🔹 asya.\*.\*.excel →** Asya kıtasındaki tüm ülkelere ait tüm şehirlerin excel raporlarını getir.

Şimdi kodlama tarafına geçelim. Bir önceki örneğimizin kopyasını alıp üzerinde düzenleme yaparak gideceğim. Bir önceki örneğimizden farklı olarak Queue oluşturma işlemini bu sefer Consumer tarafına bırakacağız.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/214d376941faaeaf0746ee71bd925fdb/href">https://medium.com/media/214d376941faaeaf0746ee71bd925fdb/href</a></iframe>

Yine her zamanki gibi tüm detayları açıklama satırları olarak ekledim. Bizim için aşağıdaki gibi rastgele üçlü serilerden oluşan hata tipleri oluştursun Publisher tarafı.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971992051/SqbN8trTd.png)

Şimdi Consumer tarafında örneğin; ortasında Info olanları ya da başında Warning olan ortasında ve sonunda ne olduğunun önemli olmadığı gibi örekler belirleyerek hata tiplerini ekrana yazdıralım.

Yine bir önceki örneğimizi kopyalayıp aşağıdaki gibi revize ettim. Değiştirdiğim yerlerin başına mutlaka açıklama satırı ekledim.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/950f375e4525c3a849491e9099c86609/href">https://medium.com/media/950f375e4525c3a849491e9099c86609/href</a></iframe>

Şimdi kuyruk mekanizmamızın çalıştığını doğrulayalım. Burada unutulmaması gerekir ki Queue oluşturma işlemi Consumer tarafında bu yüzden önce Consumer tarafını ayaklandırıp ardından Publisher’ı çalıştırmak gerekir. Önce Publisher çalıştırılırsa mesajlar boşa gider.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971994024/9bP6PKnX8.gif)

💣Yukarıda da görüleceği üzere **önce Consumer tarafını ardından da Publisher tarafını çalıştırıyorum.**

Consumer tarafında route key’lerin başıyla ve sonuyla ilgilenmeyip sadece ortasında Error olanlarla ilgileneceğimi belirttiğim için sadece ortasında Error olanlar listeleniyor.

Consumer’da rastgele isimle oluşturduğumuz Queue Name ve bind işlemi aşağıdaki gibi RabbitMQ panelinde gözlemlenebilir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971995839/XgV0K9G9a.gif)

### 5️⃣Header Exchange Tanım ve Örneği

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971997352/0sBsBwbaG.png)

**Header Exchange** diğerlerinden biraz farklı, diğerleri genelde birbirine benziyordu. Route key'den bahsettik ve ufak tefek detaylandırmalar yaparak ilerledik. Bu sefer **header** kavramıyla tanışacağız.

> **💊Header Exchange** en az kullanılan exchange tipidir.

**Header Exchange** tipinde eşleştirmelerimiz **routing key** ile değil, Publisher tarafından mesajla birlikte gönderilen **header**ile olacak.

Header’da gönderilen değerin **x-match** key’ine verilen **all** veya **any** değerine göre eşleşenler kuyruğa iletilecek.

**🔹 any→** değerlerden herhangi birinin eşleşmesi şartı  
🔹 **all →** tüm değerlerin eşleşmesi şartı

Biraz havada kalmış olabilir yine küçük bir örnek ile kafamızı biraz daha berraklaştıralım. Yine her zamanki gibi daha önce oluşturduğumuz **HeaderExchange\_Publisher**'a geliyorum ve içeriğimi aşağıdaki gibi düzenliyorum.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/5190ec0f42912efc710f84492eff0fe4/href">https://medium.com/media/5190ec0f42912efc710f84492eff0fe4/href</a></iframe>

Açıklama satırlarında neler yaptığımızı aktarmaya çalıştım.

Kabaca özetleyecek olursak artık mesajımızı **routing-key** ile değil **header** ile gönderdik. Mesajımıza header ekleyebilmek için de mesajın **Property**'lerinden Headers'a kendi oluşturduğumuz **header**'ı atadık.

Şimdi Consumer tarafında kuyruğumuzu oluşturalım ve **Headers**'tan  
**x-match** yardımı ile dinleme işlemini gerçekleştirelim. **HeaderExchange\_Consumer**'a geliyorum ve içeriğimi aşağıdaki gibi düzenliyorum.

<iframe src="" width="0" height="0" frameborder="0" scrolling="no"><a href="https://medium.com/media/e4941f4fea22ba79c0febf5b41d7d32d/href">https://medium.com/media/e4941f4fea22ba79c0febf5b41d7d32d/href</a></iframe>

Temel işlemler yine sabit kaldı. Yeni eklenen ya da değişen kısımlara açıklamaları ekledim.

Ne yaptık diyecek olursak eğer; İşimizi sağlama almak adına Publisher tarafında oluşturduğumuz **Headers** tipindeki Exchange'imizi Consumer tarafında yani burada tekrar tanımladım. Tanımlamasak da olur muydu? Olurdu ama önce Consumer'ı çalıştırsak patlardı.💣

Daha sonrasında beklediğimiz header’ı ayarladık ve Publisher’dakinden farklı olarak **x-match** key'ini kullanıp value olarak **all** diyerek birebir eşleşme olanları getirdik. İstersek **any** diyerek headers da ki herhangi bir değerle eşleşenleri de getirtebilirdik tercih ve kurgu meselesi.

Son olarak da **QueueBind** metodunun **routingKey** parametresini boş bırakıp argümanlara **header**'ımızı ekledik.

Kurgumuzun çalışıp çalışmadığını test edelim. Önce sol tarafta Consumer’ı çalıştırıp dinlemeye başlıyorum. Sonrasında da sağ tarafta Publisher’ı çalıştırıyorum.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971998933/jZd6aMBcN.gif)

RabbitMQ panelinin Exchanges sekmesinde **Routing key**'imizin boş olduğunu ve **Arguments** kısmının da **header**'ımızın dolu olduğunu gözlemleyebiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972000511/Z5vjf3iKI.png)

Son olarak hem genel bir tekrar hem de farklı bir örnek görmek açısından aşağıdaki beş dakikalık videoyu kesinlikle izlemenizi öneririm.

<iframe src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2Fo8eU5WiO8fw%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Do8eU5WiO8fw&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2Fo8eU5WiO8fw%2Fhqdefault.jpg&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" frameborder="0" scrolling="no"><a href="https://medium.com/media/633b23ad3b4b7060b4b4177fff5f6760/href">https://medium.com/media/633b23ad3b4b7060b4b4177fff5f6760/href</a></iframe>

Bu yazımda aktaracaklarım bu kadar. Fazlasıyla uzun bir yazı oldu farkındayım. Ayrı ayrı seri halinde yapılabilirdi elbette ama tek bir yerde topluca erişilebilecek sağlam bir kaynak(kendimce) olsun istedim.

Umarım ufak da olsa bir faydam dokunmuştur.

📕Bu yazı serisindeki örneklerde [Faith çakıroğlu](https://medium.com/u/8f672d2fb705)’nun Udemy eğitiminden faydalanılmıştır.

📝Yazım hatalarındaki desteğinden dolayı [Birol Gökkaya](https://medium.com/u/2236ea4ed366) özel teşekkürler.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972002138/WiUhcYTTe.gif)