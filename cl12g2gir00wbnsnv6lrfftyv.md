## Docusaurus ile Harika Dokümanlar Oluşturalım

Bir projeyi devralırken ya da yeni bir şeyler öğrenirken ilk görmek istediğimiz şey genellikle dokümanlar oluyor. Aramızda dokümanın öneminin farkında olmayan yoktur diye umut ediyorum.🥊 Bundan sebep, bu yazıda dokümanın öneminden değil, nasıl harika dokümanlar oluşturabileceğimizden bahsedeceğim.

### 🦖Docusaurus nedir?

[Docusaurus](https://docusaurus.io/)(doki-sörz) Facebook tarafından react ile geliştirilen, tamamıyla ücretsiz [açık-kaynak](https://github.com/facebook/docusaurus) bir doküman oluşturma sayfasıdır.

💡Nasıl bir görünüme sahip olduğun şu örneklere bakarak anlayabilirsiniz;

➡️ [https://devarchitecture.netlify.app/](https://devarchitecture.netlify.app/) (benim de ilk gez karşılaştığım yer)  
➡️ [https://tutorial.docusaurus.io/](https://tutorial.docusaurus.io/) (resmi tutorial)

### 🛠️Projemize Nasıl Entegre Edebiliriz?

Doğrudan projenin içerisinde harici bir klasör oluşturup entegre edebileceğimiz gibi harici bir repoda da tutabiliriz. [Sonuçta kimsenin hayatına kimse karışamaz.](https://www.youtube.com/watch?v=JR9eB4JjYBY) Biz bu yazıda harici değil, doğrudan projemize dahil oluşturacağız. Bence en makul yöntem de bu gibi görünüyor.(YTD)

Ben bu yazıda docusaurus’u örnek olması için Angular projesine dahil edeceğim ki sizde var olan projelerinize nasıl kolay entegre edilebiliyor görmüş olun.

#### 🔴1.Adım — Entegrasyon başlasın! 🚀

Ben docusaurus’u dahil etmek için çalıştıracağım komutu, projemde **📂src** klasörünün altına çalıştıracağım. Bu bir zorunluluk değil, tercih meselesi. İstenilen yere istenilen isimle bir klasör oluşturulup da çalıştırabilirsiniz.

Projemize docusaurus’u dahil etmek için çalıştıracağımız komut oldukça basit

``` 
npx create-docusaurus@latest docs classic 

```

**💡docs** dediğim yere herhangi bir şey yazılır illa docs olmak zorunda değil.

#### 🔴2.Adım — Çalışıyor mu bir bakalım! ⚙️

Hem her şeyin yolunda gittiğinden emin olmak, hem de oluşan harika taslağı görmek için çalıştırmamız gereken standart komut **npm start** . Çalıştırmadan önce, **cd docs** ile ilgili yola gitmeyi unutmamalıyız. Eğer her şey yolunda gittiyse aşağıdaki gibi bir görüntü oluşmuş olmalı. Ve nereden görüntüleyebileceğimizin ipucu veriliyor olmalı. bknz: [http://localhost:3000/](http://localhost:3000/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971914130/3TCFIuFjL.png)

#### 🔴3.Adım — Biraz özelleştirelim! 🌈

Standart taslağımız başarılı bir şekilde oluştuğuna göre, sıra geldi projemizi özelleştirmeye.

Özelleştirme ile ilgili en çokomelli dosyamız **docusaurus.config.js** dosyası. Projemizin adından tutunda, nerede deploy edileceği(değineceğim), routing bilgisine kadar her şey burada toplanıyor. Hatta eklentilerimizi bile buraya ekliyoruz.

Özelleştirme meselesi temel seviyede **html, css** bilgisi gerektiriyor tabii ama gözünüz korkmasın her şey oldukça basit.

Taslakta gördüklerimizi VS Code yardımı ile projede aratırsak işimiz oldukça kolaylaşacaktır. **_BUL_** _ve_ **_DEĞİŞTİR_** mottomuz bu! 💪🏽 Bir kaç dokunuşla aşağıdaki görüntüyü elde etmek oldukça zahmetsiz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971915790/gP5A8AATP.png)

#### 🔴4.Adım — Hiyerarşiyi kavrayalım 👮🏽‍♂️

Giriş sayfamız oldukça güzel görünüyor. Şimdi de sayfalama hiyerarşisini kavrayalım. Logomuzun hemen yanında görünen öğreticiye tıklayalım, (eğer değiştirmediyseniz tutoriala). Şöyle bir görüntü bizi karşılıyor.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971917117/Jr_3NCdpa.png)

Bunları temizlemekle uğraşmayacağım, ama yenileri nasıl eklenir göstereceğim. Şimdi yukarıda ki gibi biz de bir klasör ve md(markdown) dosyası oluşturalım.

Öncelikle isimler kafamızı karıştırmasın lütfen, doğru yerde olduğumuza yani;**📂docs’un altında ki 📂docs klasöründe olduğumuza emin olalım** ve bir klasör oluşturup içerisine **\_category\_.json** adında bir json dosyası oluşturalım. Default gelenlerden birini içinden jsonı kopyalayabiliriz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971918402/3RiBCyG_r.png)

Bu json dosyası klasörümüzün ara yüzde hangi isimle görüneceğini ve hangi sırada olacağını belirleyecek. Ve içeriği şöyle olacak;

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971919807/N2MrV7e7U.png)

Md dosyalarımız standart md dosyaları aslında ama burada da küçük bir kaç detay var. Klasörümüzün adını ve sıralamasını belirlerken json’a ihtiyaç duymuştuk, md dosyalarında ise bu json’a ihtiyaç yok. Kendi içlerinde belirtiyoruz. Yani şöyle;

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971921191/o1AvYPAxC.png)

Eğer yukarıdaki adımları başarılı bir şekilde uygulayabildiysek, görünümümüz aşağıdaki gibi olacak. Değişikliklerin yansımaz ise, belki bir durdurup **(CTRL + C)** başlatmak **(npm start)**gerekebilir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971922538/csdpUIBIn.png)

#### 💡Bir sonraki adıma geçmeden önce şunları belirteyim;

➡️Sadece **MD** değil **MDX** dosyası da desteklenir.  
➡️ **React.js** konsepti ile özgün sayfalar oluşturulabilir.  
➡️Blog sayfası düzenlenecekse, blog postlarının **başına tarih eklenmelidir.**  
➡️Yazar bilgileri **authors.yml** dosyasından düzenlenebilir.  
➡️Renkler **custom.css** dosyasından ayarlanabilir.

#### 🔴5.Adım — Dokümanımızı yayınlayalım! 🌍

Biz her ne kadar github.io üzerinde yayınlayacak olsak dahi **docusaurus** bir çok platformda kolayca yayınlama hazır halde geliyor. [Buradan](https://docusaurus.io/docs/deployment) detaylara ulaşabilirsiniz.

github.io üzerinde yayınlamaya geçmeden önce **docusaurus.config.js** bir takım ayarlamalar yapmamız gerekiyor. Neler yapılması gerektiğini yorum satırları olarak ekledim.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971923817/Uq1mlWArg.png)

Gerekli ayarlamaları yaptıktan sonra; biraz sonra vereceğim kod normal terminalde çalışmayacağı için **git bach** ‘e geçiyoruz ve komutu çalıştıracağımız yolu yazıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971925159/Y8mS6-B2W.png)

💡Eğer **yarn** kurulu değilse **npm install --global yarn** komutu ile kurabilirsiniz.

Ardından deploy için çalıştırmamız gereken kodu yapıştırıyoruz;

```git
GIT_USER=hakanyalitekin yarn deploy
```
Eğer her şey yolunda giderse şöyle bir görüntü oluşmalı, ve bize dokümanımızın yayınlandığı URL’i veriyor olması gerekiyor.  
🌍 [**https://hakanyalitekin.github.io/docDemo/**](https://hakanyalitekin.github.io/docDemo/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971926458/2hDofSJJL.png)

Artık her güncellemede yukarıdaki komutu çalıştırmamız yeterli olacaktır, değişiklikler otomatikman yansıyacaktır.

💡**Bir sonraki adıma geçmeden, meraklısına detay vereyim. Bu komut bizim için neler yaptı;**  
➡️Bizim için bir branch oluşturdu (gh-pages)  
➡️Oluşan branch’in içerisine bizim oluşturduğumuz md dosyalarından index.html’ler yaratıp koydu.  
➡️Her değişiklikte güncellenmenin gerçekleşebilmesi için Git Action oluşturdu.  
➡️Ve tabiki GitHub settings’in altına pages oluşturdu.([buyrunuz](https://hakanyalitekin.github.io/docDemo/))  
➡️Bunlar fark edebildiklerim, fark etmediğim şeylerde yapmış olabilir.🤭

#### 🔴6.Adım (BONUS )— Aradığımızı kolayca bulalım! 🔍

Hiç şüphesiz çılgınlar gibi doküman yazdık(!) Peki ya aradığımızı kolayca bulmak istersek? İnsanlar bunu da düşünmüş ve bize eklentiler sunmuş.

Biz arama eklentisini dokümanımıza dahil edeceğiz fakat topluluğun oluşturduğu farklı eklentileri de [**buradan**](https://docusaurus.io/community/resources#search) inceleyip kurmak pek tabii mümkün.

Doküman içinde arama ile ilgili bir çok alternatif var [**bu adrese**](https://docusaurus.io/docs/search) göz atabilirsiniz.

Ben topluluk tarafında oluşturulan, aralarından kendimce en pratik uygulanabilir ve şık olanını dokümanıma dahil edeceğim.

[**Bu adresten**](https://github.com/easyops-cn/docusaurus-search-local) arama için kullanacağımız ilgili repo incelenebilir. Uygulaması oldukça basit. Şu komutu çalıştırıyoruz;

``` 
npm install --save @easyops-cn/docusaurus-search-local

``` 


npm paket kurulumu tamamlandıktan sonra, **docusaurus.config.js** dosyamızın en altına (farklı yerde olabilir) aşağıdaki kod bloğunu yapıştırıyoruz.

```
themes: \[  
    \[  
      require.resolve("[@easyops](http://twitter.com/easyops)\-cn/docusaurus-search-local"),  
      {  
        hashed: true,  
      },  
    \],  
  \],
``` 
Yani şöyle oluyor.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971927779/pnAlKVVOx.png)

Artık arama kutucuğumuz hazır. Lokalde çalışırken şöyle bir uyarı alıyoruz. Bu çok normal çünkü; aramayı proje çıktısında yani html üzerinden yapıyor. Bu tüm arama eklentileri için geçerli bir uyarı. Dokümanı paylaştığınızda böyle bir uyarı ile karşılamayacaksınız.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971929023/TEiVMQff1.png)

Yukarıda paylaştığım **GIT\_USER=hakanyalitekin yarn deploy** komutuyla dokümanımın güncel halini tekrar yayınlıyorum.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971930254/SYr1M9GaT.png)

VE MANZARA MUHTEŞEM! 🌍

Umarım ufakta olsa Türkçe kaynak oluşturmaya, dokümantasyon oluşturmaya faydam dokunmuştur. Bir sonraki yazıda görüşmek üzere.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971931498/ZVV7gKSyw.gif)