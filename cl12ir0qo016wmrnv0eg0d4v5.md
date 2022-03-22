## .Net Framework için Swagger ile FileUpload

Merhaba bir önce ki yazımda paylaştığım swagger kurulum, kimlik doğrulama vs. işlemlerine bu [**link**](https://medium.com/@hakanyalitekin/net-framework-i%C3%A7in-swagger-ve-jwt-authentication-f8928cc5db0b) üzerinden erişebilirsiniz. Bu yazı devamı niteliğinde olacak.

**Doğrudan örnek projeyi incelemek için** [**GitHub linki.**](https://github.com/hakanyalitekin/SwaggerWithJWT)

Gelelim asıl meselemize, dosya yükleme işlemlerinde Postman’a bağlı kalmadan swaggerdan from-data üzerinden nasıl dosya yükleme yapılacağından bahsedeceğim.

Bunun için öncelikle bir atrribute’e ihtiyacımız olacak. Bu attribute’ü kullanarak hangi serviste file upload yapacağımızı belirteceğiz ve istersek belirli parametereler yollayarak örneğin required olup olmadığını, açıklamasını belirtebileceğiz.

Projemizin Filters klasörüne **SwaggerParameterAttribute.cs** adında bir attribute sınıfı ekleyelim ve içeriğini aşağıda ki gibi ayarlayalım. Default alanları siz istediğiniz şekilde ayarlayabilirsiniz.


```csharp
using System;

namespace SwaggerWithJWT.Filters
{
    public class SwaggerParameterAttribute:Attribute
    {
        public string Name { get; set; } = "File";

        public string Description { get; set; } = "Select a file to upload";

        public string Type { get; set; } = "file";

        public bool Required { get; set; } = false;
    }
}
``` 


Artık attribute’tümüz hazır olduğuna göre, gelelim swagger ayarlamasına. Bu ayarlamayı yapabilmek için IOperationFilter interface’inden faydalanarak kendi OperationFilter kuralımızı oluşturacağız.

Oluşturacağımız bu class’ı ister swagger’ın halihazırda ki SwaggerConfig.cs’ine ekleyebilir, isterseniz de ayrı bir class oluşturarak yapabilirsiniz. Ben çok dallanıp budaklanmamak için direk içerisine dahil ettim.

OperationFilter’ın nasıl bir mantık ile çalıştığını kodun içerisine yorum satırı olarak ekledim.


```js
public class SwaggerParameterOperationFilter : IOperationFilter
        {
            public void Apply(Operation operation, SchemaRegistry schemaRegistry, ApiDescription apiDescription)
            {
                /*
                 * Yazdığımız bu filter'ın neye göre çağıralacağını burada belirliyorz.
                 * GetControllerAndActionAttributes<SwaggerParameterAttribute> ile Controller'deki atrribute'ü yaklıyoruz.
                 * Any ile var mı diye kontrol ediyoruz eğer sonuç true ise dahil olacak filter'ımızı set ediyoruz.
                 */
                var requestAttributes = apiDescription.GetControllerAndActionAttributes<SwaggerParameterAttribute>();

                if (requestAttributes.Any())
                {
                    operation.parameters = operation.parameters ?? new List<Parameter>();

                    foreach (var attr in requestAttributes)
                    {
                        operation.parameters.Add(new Parameter
                        {
                            name = attr.Name,
                            description = attr.Description,
                            @in = attr.Type == "file" ? "formData" : "body",
                            required = attr.Required,
                            type = attr.Type
                        });
                    }

                    if (requestAttributes.Any(x => x.Type == "file"))
                    {
                        operation.consumes.Add("multipart/form-data");
                    }
                }
            }
        }
``` 


Swagger’da oluşturduğumuz bu OperationFilter’i aktifleştirmemiz gerekiyor bunun içinde SwaggerConfig.cs’in içerisine aşağıdaki kodu ekliyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972088805/Ml8UoTfQp.png)

Artık oluşturduğumuz SwaggerParameterAttribut’ü kullanarak swagger’dan file upload işlemlerimizi gerçekleştirebiliriz. Bunun için örnek bir kullanımı aşağıda paylaşıyorum.


```csharp
[Route("api/Home/FileUpload")]
        [HttpPost]

        //Parametreli örnek
        //[SwaggerParameter(Name = "Dosya", Description = "Excel dosyası", Required = true, Type = "file")]

        //Parametresiz varsayılan değerlerle örnek.
        [SwaggerParameterAttribute]
        public HttpResponseMessage FileUpload()
        {
            HttpPostedFile file = HttpContext.Current.Request.Files[0];
            var httpRequest = HttpContext.Current.Request;

            //var file = httpRequest.Files[fileName];
            var filePath = HttpContext.Current.Server.MapPath("~/" + file.FileName);
            file.SaveAs(filePath);

            return Request.CreateResponse(HttpStatusCode.Created);
        }
``` 


Kodumuzu çalıştırdığımızda görmemiz gereken görüntü aşağıda ki gibi olmalıdır. Eğer değilse bir şeyleri yanlış yapmış olabiliriz. Yazıyı ya da github üzerinde ki projeyi inceleyebilirsiniz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972090209/x6UCGQUWX.png)

Umarım ufakta olsa faydam dokunmuştur.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972091620/R-YueLI6L.gif)