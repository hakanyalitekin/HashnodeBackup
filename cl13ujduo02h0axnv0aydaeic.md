## LINQ Nedir?

LINQ; açılımı **Dil Tümleşik Sorgu (Dile Entegre Edilmiş Sorgu)** olan, Microsoft tarafından kullanılan ve C # 3.0 ile hayatımıza giren farklı veri kaynaklarından sorgulama yapabilmemize imkan verir.

LINQ, koleksiyonlar, ADO.Net DataSet, XML, SQL Server, Entity Framework ve diğer veritabanları gibi farklı veri kaynağı türlerinden veri almak için oluşturulmuş bir sorgu söz dizimidir. Alttaki görsel durumu daha net anlatacaktır.


LINQ’da iki farklı söz dizimi mevcuttur.

- **Sorgu Sözdizimi** (Sorgu söz dizimş) -> from ile başlar.
- **Yöntem Sözdizimi** (Metot söz dizimi)

Eğer daha önce SQL komutları yazdıysak, Sorgu Sözdizimi yordamı bize hiç de yabancı gelmeyecektir. Küçük bir örnek ile iki söz dizimini inceleyelim.

```
// Veri Kaynağı (Bu veri yukarıda basettiğim gibi her hangi bir yerden sağlanıyor olabilir.)

int[] sayilar = new int[] {5,4,1,3,9,8,6,7,2,0};

// LINQ sorgusu -> QUERY SÖZ DİZİMİ
var kucukSayilar1 = from sayi in sayilar
where sayi < 5
select sayi;

//LINQ sorgusu -> METHOD SÖZ DİZİMİ (Bayılırım 🙂)
var kucukSayilar2 = sayilar.Where(x => x < 5);

// Yazılan sorguyu kullanma
foreach(int sayi in kucukSayilar1) {
Console.WriteLine(sayi);
}
```

LINQ ile tanıştığımıza göre , kullanım pratiklerini iyice kavrayabilmek adına GitHub’ta ki dersleri indirip satır satır okuyabiliriz. Aşağıda ön görünümü olan desleri indirebileceğiniz [**link**](https://github.com/hakanyalitekin/LINQ).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648056916384/PUBEIAJxW.png)