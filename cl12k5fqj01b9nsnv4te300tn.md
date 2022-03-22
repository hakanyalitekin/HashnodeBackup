## C# ile PDF Oluşturma (Örnek fatura uygulaması)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972077532/Wktuc2ba-.jpeg)

Merhabalar bu yazımda günlük hayatta olmazsa olmazlar arasında bulunan PDF’leri, C# ile Server side Handlebars template engine kullanarak nasıl oluşturabileceğimizden bahsedeceğim.

**Doğrudan örnek projeyi incelemek için** [**GitHub linki.**](https://github.com/hakanyalitekin/SampleCreatePDF)

Kullanım alanları olarak gerek fatura, irsaliye gerek blog sitelerimize ekleyeceğimiz CV’lerde kullananabileceğimiz örnekler yapmak mümkün.

Burada ben, günümüzde çok daha yaygın olan Web Servis ile örnek uygulama yapacağım fakat aynı kodlar ile ister .Net Core ister .Net Framework 4.5+ fark etmeksizin tüm platformlar için kullanabilirsiniz. Yani aynı kodları Console’a da WinForm’a da dahil edebilirsiniz.

C# ile PDF oluşturmak için iki adet NuGet paketi ve bir adet Html şablonu gereksinimimiz olacak;

**1-** [**https://www.nuget.org/packages/Select.HtmlToPdf**](https://www.nuget.org/packages/Select.HtmlToPdf)**2-** [**https://www.nuget.org/packages/Handlebars.Net**](https://www.nuget.org/packages/Handlebars.Net) **(Template Engine )  
3- Örnek şablona yukarıda ki** [**github linkinden**](https://github.com/hakanyalitekin/SampleCreatePDF/blob/master/SampleCreatePDF/PDF_Template.html) **erişilebilir.**

Ben PDF ile ilgili 5 sayfaya kadar ücretsiz olan SelectPDF’i kullandım fakat siz ihtiyaçlarınıza göre [**bu link**](https://www.it-swarm.dev/tr/c%23/htmlyi-.nette-pdfye-donusturun/958245890/) üzerindeki tartışmaya göz atarak farklı bir package kullanabilirsiniz. (Eğer SelectPDF’i kullanacaksanız dahi bir göz atmanızda fayda var)

Kodlamaya geçmeden önce template engine’lerin çalışma mantığından kısaca bahsedeyim. Template engine’leri Html input’ların yer tutucularına(placeholder) benzetmek mümkündür. Yaptığı iş örneğin Html içerisinde {{isim}} gördüğü yeri göndereceğimiz isim değeri ile yer değiştirmesidir. Handlebars’ın yeteneklerine [**bu link**](https://handlebarsjs.com/guide/) üzerinden erişebilirsiniz. Ben basit expression ve each yeteneklerinden faydalanacağım.

Html içerisinden küçük bir örnek gösterim;

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972078839/e8jipfB5O.png)

Gelelim kod kısmına. Öncelikle bir PdfManager oluşturuyoruz ve içerisine SelectPDF ile ilgili kısmı yazıyoruz.

> _Ben portrait bir A4 kağıdı üzerinden gittim fakat siz farklı ihtiyaçlarınıza göre farklı kağıt seçenekleri ekleyebilirsiniz. PdfPageSize enum’u üzerinden kağıt seçeneklerini görebilirsiniz._


```
 HtmlToPdf _htmlToPdf = new HtmlToPdf();
        public byte[] HtmlToPdf(string html)
        {
            //Kağıdımızı oluşturuyoruz. Duruma göre farklı kağıt ölçüleri çağırılabilir.
            PageA4();

            //PDF'i oluşturuyoruz.
            var doc = _htmlToPdf.ConvertHtmlString(html);

            return doc.Save();
        }

        private void PageA4()
        {
            _htmlToPdf.Options.PdfPageSize = PdfPageSize.A4;
            _htmlToPdf.Options.PdfPageOrientation = PdfPageOrientation.Portrait;
            _htmlToPdf.Options.MarginLeft = 10;
            _htmlToPdf.Options.MarginRight = 10;
            _htmlToPdf.Options.MarginTop = 20;
            _htmlToPdf.Options.MarginBottom = 20;
        }
``` 


Atrık Pdf oluşturma metodumuz olduğuna göre template engine’imizi kullanarak **Html - Model** birleştirme işlemini gerçekleştirebiliriz. Fakat bu birleştirme işleminine geçmeden önce Handlebars’ın nimetlerinden biraz daha faydalanmak istiyorum. Para ve tarih formatların da küçük ayarlamalar yapan metodumu yazıyorum. _(Ben tarih formatını açıklayacağım bunu anladığınızda para formatını da anlamış olacaksınız.)_

Öncelikle metodumuzda kullandığımız parameters’in değerlerini html içerisinde önceden ayarlıyoruz ve ekran görüntüsünün aşağıdaki gibi olduğundan emin oluyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972080193/9f47C-mgQ.png)

Ve sonrasında metodumuzu yazıyoruz. Bu metot ile ilgili önemli ayrıntıları yorum satırı olarak ekledim fakat tekrar bahsedeyim.

Neden ikinci parametre olarak tarih formatını aldık, elle yazsak olmaz mıydı?

Kesinlikle olurdu. Fakat birden fazla format kullanılmak istendiğinde bunu nasıl çözeceğinizi göstermek istedim.

```

public string CombineModelToHtml(string html, object model)
{
    //Tarih Formatı
    Handlebars.RegisterHelper("dateFormat", (writer, context, parameters) =>
    {
        writer.WriteSafeString(DateTime.Parse(parameters[0].ToString()).ToString(parameters[1].ToString()));

        /* 
         Bu kısım biraz kafa karıştırıcı olabilir. Bu yüzden biraz detaylandırıyorum
         
         * * 1.Parametre -> parameters[0].ToString() -> örn. 30.01.2020
         * 2.Parametre -> parameters[1].ToString() -> "dd/MM/yyyy"
         
          Neden tarih formatını parametik yaptık da elimizle yazmadık.
          Bunun sebebi birden fazla tarih foramtı istenmesi durumunda bu isteği karşılayabilmek için.

         */
    });

    // Para Formatı
    Handlebars.RegisterHelper("moneyFormat", (writer, context, parameters) =>
    {
        writer.WriteSafeString($"{parameters[0]:N2}");
    });

    return HttpUtility.HtmlDecode(Handlebars.Compile(html)(model));
}
```

PdfManager sınıfımız hazır olduğuna göre şimdi kullanma aşamasına geçelim. Tüm detaylarını yorum satırı olarak ekledim, tek tek burada yeniden bahsedip gereksiz yere vaktinizi almayacağım :)


```
 [HttpGet]
        [Route("api/Home/CreatePDF")]
        public HttpResponseMessage CreatePDF()
        {
            PdfManager _htmlToPdf = new PdfManager();
            /*
             Örnek HTML şablonumuza erişiyoruz.
             Gerçek hayatta veritabanından erişilir.
            */
            string temp = System.IO.File.ReadAllText(AppDomain.CurrentDomain.BaseDirectory + "PDF_Template.html");

            // Örnek sipariş oluşturan metodumuzu çağırıyoruz.
            var order = GetOrder();

            //Handlebars'tan faydalanarak modelimiz ile template'imizi eşitliyoruz. (Öncesinde bir kaç format ayarı yapıyoruz.)
            var html = _htmlToPdf.CombineModelToHtml(temp, order);

            //Ve pdf'imizi oluşturuyoruz.
            byte[] pdf = _htmlToPdf.HtmlToPdf(html);

            /*
             Bu kısmı sonucu görebilmek adına ekledim. 
             Gerçek hatta örneğin Azure Storage'a yüklenip linki istenebilir
             ya da herhangi bir yere kaydedip DB'ye yolunun yazılması istenebilir.
             */
            #region PDF'ile alakası olmayan kısım.
            HttpResponseMessage result = null;
            result = Request.CreateResponse(HttpStatusCode.OK);
            result.Content = new ByteArrayContent(pdf);
            result.Content.Headers.ContentDisposition = new System.Net.Http.Headers.ContentDispositionHeaderValue("attachment");
            result.Content.Headers.ContentDisposition.FileName = "Invoice" + ".pdf";
            #endregion

            return result;
``` 


localhost:(port)/api/home/CreatePdf’ü tetikleyerek pdf’in oluşup oluşmadığını test edebilirsiniz. Örnek görüntü aşağıdaki gibi olmalıdır.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972081418/PMSOeoyyJ.png)

Umarım ufakta olsa faydam dokunmuştur.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972082890/OKDhJT9q-.gif)