## MongoDB ile CURD işlemleri(.Net 5)

### MongoDB ile CRUD işlemleri(.Net 5)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972007431/p4y1dOn6z.jpeg)

Merhabalar,

Uzun bir zamandır bir şeyler paylaşmadığımı fark ettim, kurum içi etkileşimi ve öğrenimi artırmak için hazırladığım içeriği bu platform üzerinden de paylaşmak istedim. Öncelikle tanımlardan başlayalım ve sonrasında küçük bir örnek yapalım.

Doğrudan örnek projeye erişmek için [**GitHub linki.**](https://github.com/hakanyalitekin/MongoDB101)

### **MognoDB Nedir?**

MongoDB resmi sitesinde kendisini şu şekilde tanımlıyor;

_“MongoDB, modern uygulama geliştiricileri ve bulut çağı için oluşturulmuş genel amaçlı, doküman tabanlı, dağıtılmış bir veri tabanıdır.”_

Resmi tanımlamaya ilaveler yapacak olursak; verilerin doküman olarak tutulduğu, **\-dokümandan kasıt JSON formatı-** açık kaynak kodlu, ücretsiz NoSQL bir veri tabanıdır.

MongoDB’yi bankacılık sektörü gibi verinin çok önemli olmadığı durumlar hariç, pek çok senaryo için kullanmak mümkün. Örneğin; loglamalar, birçok tablodan beslenen listeleme(rapor) ekranları en sık kullanılan senaryolardır.

Peki resmi tanımda doküman tabanlı derken kastedilen NoSQL database nedir? MongoDB’nin detaylarına inmeden kısaca NoSQL’den bahsedelim.

### **NoSQL Nedir?**

NoSQL, “Not Only SQL”in kısaltılmış halidir. ilişkisel veri tabanlarına alternatif, ilişkisel olmayan, esnek yapılı, büyük verili ve çok sayıda request-response dönen sistemlerde, yüksek performans ve yönetim kolaylığı sunan veri tabanı çözümüdür.

NoSQL illaki doküman tabanlı olmak zorunda değildir. Bir çok farklı türü mevcuttur.

NoSQL’in avantajları, dezavantajları, detaylı anlatımı ve diğer türleri ile ilgili aşağıdaki kaynaklara göz atabilirsiniz.

**Amazon Türkçe Kaynak** → [https://aws.amazon.com/tr/nosql](https://aws.amazon.com/tr/nosql/)  
**Diğer bir kaynak (**[**Halis Ak**](https://medium.com/u/877110501571)**)** →[https://link.medium.com/pJ5vhQvLReb](https://link.medium.com/pJ5vhQvLReb)

Asıl konumuz olan MongoDB’ye dönecek olursak, konunun daha iyi pekişmesi için CRUD işlemlerinden oluşan küçük bir .Net Core projesi yapalım.

Tabi projemize geçmeden önce MongoDB ile çalışmak için gerekli ortamı sağlayalım.

### **MongoDB ile Çalışma Ortamının Hazırlanması**

MongoDB ile ilgili çalışabileceğimiz bir çok farklı ortam yaratmak mümkün,

*   **İşletim sistemine kurulum yaparak;** [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community) üzerinde işletim sisteminize uygun versiyonu seçip kurulumu yapabiliriz.
*   **MongoCloud kullanarak;** [https://www.mongodb.com/cloud](https://www.mongodb.com/cloud) linki üzerinden hesap açarak, ücretsiz 500mb limitli veri tabanı kümleri oluşturabiliriz.
*   **Docker’a kurulum yaparak;** (bizim örneğimiz) [https://hub.docker.com/\_/mongo](https://hub.docker.com/_/mongo) linki üzerinden gerekli talimatları takip ederek. MongoDB’yi kullanmak mümkün

### Docker Üzerinde MongoDB’nin kurulması

Docker’da MongoDB’yi standart portta ayaklandırmak için, aşağıdaki kodu çalıştırıyoruz. (Eğer yüklü değilse zaten yüklüyor)

```
docker run -d --name Mongo101 -p 27017:27017 mongo:latest
```

Container’imiz ayaklandığına göre, içine girip inceleyebiliriz.

```
docker exec -it Mongo101 bash
```

Daha sonrasında MongoDB’yi incelemek istersek, eriştiğimiz bash’e **mongo** komutunu vererek Mongo Shell'e erişmemiz yeterlidir. Aşağıdaki versiyon bilgilerinin, connection string bilgilerini görebiliyor olmamız gerekir.

```
mongo
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972008876/JrUx5VijC.png)

### Proje Öncesi MongoDB’ye bir göz atalım

Artık mongo komutları verebildiğimize göre ilk kurulumda gelen default veri tabanlarının listesine bir göz atalım.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972011809/03yEHjHii.png)

```
#Mongo shell komutu
show dbs
```
> MongoDB case sensitive (harf duyarlı) bir yapıdadır. Bu yüzden büyük küçük harflere dikkat edilmelidir.

Default gelen DB’lere göz gezdirdiğimize göre artık kendi DB’mizi oluşturma vakti geldi. Bunun için **_use_** komutunu kullanabiliriz. Eğer varsa kullanacak, yoksa yeni oluşturulacak.


```
use eStoreDB

#Extra Bilgi:"use" komutu ile gidilmiş o anki DB'yi siler.(use eStoreDB)
db.dropDatabase()
```

DB’yi oluşturduk, şimdi sıra geldi tablomuzu(collection) oluşturmaya.


```
db.createCollection("users")

#Extra Bilgi: Collection(tablolar)'ı listeler.
db.getCollectionNames()

#Extra Bilgi: Collection'u(tabloyu siler)
db.users.drop()
``` 


DB'miz ve Tablomuz(Collection) hazır olduğuna göre örnek bir kaç kayıt atalım. Bunun için **insertMany** metodunu kullanacağız.


```json
db.users.insertMany([
  {
    "userNo": 203648,
    "firstName": "Ahmet",
    "lastName": "Yılmaz",
    "age": 20,
    "email": "ayilmaz@info.com",
    "phone": "0555-111-11-11",
    "city": "İstanbul"
  },
  {
    "userNo": 203649,
    "firstName": "Mehmet",
    "lastName": "Öztürk",
    "age": 25,
    "email": "mozturk@info.com",
    "phone": "0555-222-22-22",
    "city": "Ankara"
  },
  {
    "userNo": 203650,
    "firstName": "Ayşe",
    "lastName": "Aydın",
    "age": 30,
    "email": "aaydin@info.com",
    "phone": "0555-333-33-33",
    "city": "İzmir"
  },
  {
    "userNo": 203651,
    "firstName": "Yıldız",
    "lastName": "Şahin",
    "age": 35,
    "email": "ysahin@info.com",
    "phone": "0555-444-44-44",
    "city": "Manisa"
  }
])
``` 


**MongoDB’nin Temel CRUD yapısını inceleyelim →** [https://docs.mongodb.com/manual/crud/](https://docs.mongodb.com/manual/crud/)

**MongoDB Karşılaştırma Operatörleri inceleyelim →** [https://docs.mongodb.com/manual/reference/operator/query/](https://docs.mongodb.com/manual/reference/operator/query/)

**MongoDB’nin SQL’e karşılık gelen yapısını inceleyelim →** [https://docs.mongodb.com/manual/reference/sql-comparison/](https://docs.mongodb.com/manual/reference/sql-comparison/)


```
Yukarda insert örneği görmüştük simdi bir kaç örnek listeleme sorgu yazalım;

#Tüm kullanıcıları listele
db.users.find().pretty()

#Yaşı 25'ten büyük olanların Ad-Soyad bilgisini getir.
db.users.find({ age:{$gt:25}} , {firstName:1, lastName:1}).pretty()
``` 

Temel hatlarıyla MongoDB üzerine fikir sahibi olduğumuza göre ilk uygulamamızı kodlamaya başlayabiliriz.

### MongoDB CRUD işlemleri (.Net 5)

#### ➡️1. Adım Uygulamanın Oluşturulması

Öncelikle bir Web API projesi oluşturuyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972013222/nu8Mtlij6.png)

Proje adını **MongoDB101.API,** solution adını **MongoDB101** olarak ayarlıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972015603/hG76JWtsW.png)

Daha sonrasında ise, framework olarak **.NET 5.0** ve beraberinde gelen **swagger** desteği için **Enabel OpenAPI support**’u işaretliyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972016848/ZzkIddg-8.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972018275/cby4msnH7.png)

Default olarak gelen controller ve klası siliyoruz.

Projemizi oluşturduğumuza göre, MongoDB’yi kullanabilmek için ihtiyacımız olan nuget paketini aşağıdaki görsel yardımı ile kuralım.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972019709/C2xO8GquU.png)

#### ➡️2. Adım MongoDB’ye erişmek için gerekli olan konfigürasyonlar

Şimdi sıra geldi MongoDB’ye bağlanma aşamasına. Bunun için öncelikle -— 📂**Settings** adında bir klasör, akabinde **IDbSettings** adında public bir interface oluşturuyoruz ve içeriğine aşağıdaki property’leri ekliyoruz.

```
public interface IDbSettings
    {
        public string ConnectionString { get; set; }
        public string Database { get; set; }
        public string Collection { get; set; }
    }
```

Bu interface’i implemente edecek olan **MongoDbSettings** adındaki klasımızı oluşturuyoruz. **IDbSettings** interface’imizi implemente ediyoruz.


```
public class MongoDbSettings : IDbSettings
    {
        public string ConnectionString { get; set; }
        public string Database { get; set; }
        public string Collection { get; set; }
    }
```

**appsettings.json** dosyasına MongoDB’ye bağlanmak için gerekli olan tanımlamalarımız yapıyoruz.

```
"MongoDbSettings": {
    "ConnectionString": "mongodb://localhost:27017",
    "Database": "eStoreDB",
    "Collection": "users"
  }
```
**Startup.cs** klasımıza gidip aşağıdaki kodlarımızı **ConfigureServices**'in altında tanımlıyoruz. Ne anlama geldiklerini aşağıda yorum olarak ekledim.


```...
/* appsettings.json konfigürasyon dosyamızın içerisinde ki, MongoDbSettings isminde ki section'ın alınacağını 
ve bunun MongoDbSettings sınıfının property'leriyle set edileceğini belirtiyoruz. */
services.Configure<MongoDbSettings>(Configuration.GetSection(nameof(MongoDbSettings)));


/* DI için gerekli olan, Controller'ların contractor'larında IDbSettings interface'inin taşıyabileceği instance'ların,
çözümlenerek enjekte edilebileceğini belirtiyoruz. */
services.AddSingleton<IDbSettings>(sp => sp.GetRequiredService<IOptions<MongoDbSettings>>().Value);
...
``` 


#### ➡️3. Adım Collection’a karşılık gelen modelin oluşturulması

Servis katmanımızı yazmadan önce modelimizi oluşturalım. Uygulamamıza öncelikle 📂**Model** adında bir klasör ve sonrasında **User** adında bir klas ekleyelim ve içeriğini aşağıdaki gibi dolduralım.

property’lerin üzerlerinde bulunan attribute’lerin ne ifade ettiğini yorum olarak belirttim.


```
using MongoDB.Bson;
using MongoDB.Bson.Serialization.Attributes;

namespace Mongo101.Core
{
    public class User
    {
        [BsonId] // Id'kolonumuzu tanımlıyor.
        [BsonRepresentation(BsonType.ObjectId)] //Property'imizin tipini belirtiyoruz.
        public string Id { get; set; }

				//BsonElement -> Collection'umuzda karşılık gelen kolon adını temsil ediyor.
        [BsonElement("userNo")]
        public int UserNo { get; set; }

        [BsonElement("firstName")]
        public string FirstName { get; set; }
        
        [BsonElement("lastName")]
        public string LastName { get; set; }

        [BsonElement("age")]
        public int Age { get; set; }

        [BsonElement("email")]
        public string Email { get; set; }

        [BsonElement("phone")]
        public string Phone { get; set; }

        [BsonElement("city")]
        public string City { get; set; }

    }
}
``` 


#### ➡️4. Adım Service’in yazılması

Projemize 📁**Service** adında bir klasör ve **UserService** adında bir klas oluşturalım. Constructor’ımızı aşağıdaki gibi düzenleyelim. Akabinde ilgili metotlarımızı ekleyelim.


```
using MongoDB.Driver;
using MongoDB101.API.Model;
using MongoDB101.API.Settings;
using System.Collections.Generic;
using System.Linq;

namespace MongoDB101.API.Service
{
    public class UserService
    {
        private IDbSettings _settings;
        private IMongoCollection<User> _user;

        public UserService(IDbSettings settings)
        {
            _settings = settings;
            MongoClient client = new MongoClient(settings.ConnectionString);
            var db = client.GetDatabase(settings.Database);
            _user = db.GetCollection<User>(settings.Collection);
        }


        //Tümünü Listeleme
        public List<User> GetAll()
        {
            return _user.Find(u => true).ToList();
        }

        //Tek bir kullanıcı getirme
        public User GetSingle(string id)
        {
            return _user.Find(u => u.Id == id).FirstOrDefault();
        }

        //Kullanıcı ekleme
        public User Add(User user)
        {
            _user.InsertOne(user);
            return user;
        }

        //Kullanıcı güncelleme
        public long Update(User currentUser)
        {
            return _user.ReplaceOne(u => u.Id == currentUser.Id, currentUser).ModifiedCount;
        }
        
        //Kullanıcı silme
        public long Delete(string id)
        {
            return _user.DeleteOne(u => u.Id == id).DeletedCount;
        }

    }
}
``` 

#### ➡️5. Adım  Controller’ın oluşturulması

**UserController**’ımızı oluşturalım fakat oluştururken **API**’ı seçtiğimize emin olalım.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972020956/VAmk7JJyh.png)

**UserService**'in constructor'ında ihtiyacımız olan **settings** bilgilerini aşağıdaki gibi ekliyoruz. Akabinde ilgili metotlarımızı yazmaya başlıyoruz.


```
using Microsoft.AspNetCore.Mvc;
using MongoDB101.API.Model;
using MongoDB101.API.Service;
using MongoDB101.API.Settings;
using System.Collections.Generic;

namespace MongoDB101.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class UserController : ControllerBase
    {
        UserService _userService;

        public UserController(IDbSettings settings)
        {
            _userService = new UserService(settings);
        }

        //Tümünü Listeleme
        [HttpGet]
        public ActionResult<List<User>> GetAll() => _userService.GetAll();

        //Tek bir kullanıcıyı getirme
        [HttpGet("{id:length(24)}")]
        public ActionResult<User> Get(string id) => _userService.GetSingle(id);

        //Kullanıcı ekleme
        [HttpPost]
        public ActionResult<User> Add(User user) => _userService.Add(user);

        //Kullanıcıyı güncelleme.
        [HttpPut]
        public ActionResult Update(User currentUser)
        {
            var user = _userService.GetSingle(currentUser.Id);
            if (user == null)
                return NotFound();

            _userService.Update(currentUser);
            return Ok();
        }

        //Kullanıcıyı silme
        [HttpDelete("{id:length(24)}")]
        public ActionResult Delete(string id)
        {
            var user = _userService.GetSingle(id);

            if (user == null)
                return NotFound();

            _userService.Delete(id);
            return Ok();
        }
    }
}
``` 


####➡️ 6. Adım  Swagger ile metotların tetiklenmesi

Uygulamamızı çalıştırdığımızda şöyle bir ekranın bizi karşılaması gerekiyor ve bu da sırasıyla endpoint’lerimizi test edebiliriz demektir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972022533/vsMQgG9b6.png)

Umarım ufakta olsa faydam dokunmuştur. Bir sonraki yazıda görüşmek üzere.

**Kaynaklar;  
**[https://docs.mongodb.com/manual/crud/](https://docs.mongodb.com/manual/crud/) [https://www.youtube.com/watch?v=jc09I8-lcKk](https://www.youtube.com/watch?v=jc09I8-lcKk)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972023822/1acF5GMIg.gif)