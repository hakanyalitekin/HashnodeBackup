## Örneklerle En Popüler Sıralama Algoritmaları (C#)

Bu kapak görseli “Insertion(Araya Ekleme) Sıralaması” için popüler bir örnektir.

🤷‍♂️Öncelikle harika bir yazı olmayabilir, beklentiyi düşürerek devam etmenizi rica edeceğim. Ama “işte aradığım sadelik” de diyebilirsiniz tam kestiremiyorum.😊 Derleme notlar gibi düşünülebilir. Elimdeki sadeleştirdiğim kod örneklerini, gerek danslar gerek gif’lerle destekleyerek aktarmaya çalışacağım.

Sıralama algoritmaları oldukça fazla. Ben aralarından, popüler ve anlaşılması kolay olanları seçmeye çalıştım.

🤭İş hayatınızda çok işinize yarayacak gibi bir taahhüttüm yok ama bir bakış açışı katabilir. En kötü mülakatlarda adlarını sayabilirsiniz.

☕Bu yazı içerisinde paylaşılan kod örneklerini bakarak anlamak zor olabilir. Kahvenizi de soğutmadan sakin sakin debug etmenizi şefkatle öneriyorum.  
[Github Linki](https://github.com/hakanyalitekin/SortingAlgorithms)

**Algoritmaları tek tek ele almadan önce, faydalı olacağını düşündüğüm bazı linkler paylaşmak istiyorum. Devam etmeden önce mutlaka tek tek incelenmelidir.**

➡️**Büyük resmi görmek:** [https://www.toptal.com/developers/sorting-algorithms](https://www.toptal.com/developers/sorting-algorithms)

➡️**Dans Videoları İle Olayı Kavramak:** [https://www.youtube.com/watch?v=lyZQPjUT5B4&list=PLcX11VWS1PdA4dSPip8-1JfKxFa32X53y&index=6](https://www.youtube.com/watch?v=lyZQPjUT5B4&list=PLcX11VWS1PdA4dSPip8-1JfKxFa32X53y&index=6)

➡️**Adım Adım İncelemek 1:** [https://sadanandpai.github.io/sorting-visualizer/dist/](https://sadanandpai.github.io/sorting-visualizer/dist/)

➡️**Adım Adım İncelemek 2:** [https://visualgo.net/en/sorting](https://visualgo.net/en/sorting?slide=1)

### 📌 Bilmekte fayda var, sıralama algoritmalarının bazı kriterlere göre sınıflandırılır:

*   **Bellek Kullanımı:** Çalışırken ek bellek ihtiyacı duyan algoritmalarda kullanılabilecek bir ölçüttür.
*   **Hesaplama Karmaşıklığı:** Temel üç grup ölçek kullanılır. Bunlar **best**, **average** ve **worst** durumu olarak belirtilir. İşlem yoğunluğu, zaman işleyişiyle paralel olduğundan algoritmanın işleyiş süresini de etkiler.
*   **Yer Değiştirme Karmaşıklığı:** İçerisinde ek bellek kullanmayan (in place) algoritmalarda kullanılan karşılaştırılabilmesi için önemli bir ölçüttür.
*   **Kararlılık(Stability):** Algoritmanın uygulanması sırasında sıralanmış bir verinin tekrar sıralamaya tabi tutulup tutulmadığını belirten ölçektir.
*   **Rekürsiflik(Tekrar):** İç içe kendi kendini çağıran algoritmalarda kullanılan bir ölçüttür. Burada en önemli kriter stack dediğimiz maksimum iç içe çağırım kapasitesine dikkat edilmesi ve bu kapasitenin kullanılma sıklığıdır.

**En önemli kriterler**

*   **Hafıza Verimliliği** (Memory Efficiency)
*   **Zaman Verimliliği** (Time Efficiency)

### 📌**Bubble (Kabarcık) Sorting**

Sıralanacak dizinin üzerinde sürekli iki bitişik öğeyi karşılaştırır. Öğelerin yanlış sırada olmaları durumunda yerlerini değiştirir. Sıralama gerçekleşene kadar devam eder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971954947/psq7eDh_5.gif)

Bubble Sorting GIF

```cs
public static void BubbleSort(int[] array)
   {
     for(int i = 0; i <= array.Length - 1; i++)
     {
       for(int j = 1; j <= array.Length - 1; j++)
        {
          if(array[j - 1] > array[j])
           {
              int temp = array[j - 1];
              array[j - 1] = array[j];
              array[j] = temp;
           }
        }
      }
   }
```

### 📌**Selection (Seçmeli) Sorting**

Sıralanacak olan dizide, en küçük öğeyi seçer ve bu öğeyi sıralanmamış listenin başına yerleştirir. Sıralama gerçekleşene kadar devam eder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971956841/Ga2-8yTTu.gif)

```cs
public static void SelectionSort(int[] array)
   {
     for(int i = 0; i < array.Length - 1; i++)
       {
         for (int j = i; j < array.Length; j++)
           {
              if (array[j] < array[i])
                {
                   int temp = array[j];
                   array[j] = array[i];
                    array[i] = temp;
                 }
            }
       }
   }
```

### 📌**Insertion(Araya Ekleme) Sorting**

Sıralanacak olan dizi içerisindeki öğeyi sırasıyla alıp sürekli bir öncekiyle karşılaştırır. Kendinden daha küçük bir öğe bulana kadar öne geçmeye devam eder. Genellikle kart oyunlarında farkında olmadan uyguladığımız sıralama algoritmasıdır.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971958281/Y0CdjcY4H.gif)

Insertion Sorting GIF

```cs
public static void InsertionSort(int[] array)
    {
      for (int i = 1; i < array.Length; i++)
        {
           int currentValue = array[i];
           int previousIndex = i - 1;
           while(previousIndex >= 0 && currentValue < array[previousIndex])
            {
               array[previousIndex + 1] = array[previousIndex];
               previousIndex = previousIndex - 1;
            }
           array[previousIndex + 1] = currentValue;
        }
    }
```

### 📌 **Merge (Birleştirme) Sorting**

Sıralanacak olan diziyi, ikişer elemanı kalana kadar sürekli olarak ikiye böler ve daha sonra bu parçaları kendi içlerinde sıralayarak birleştirilir. Elde edilen dizi, sıralı dizinin kendisidir.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971959797/FMMvDXXKU.gif)

Merge Sorting GIF


```cs
public static void Sort(int[] array, int left, int right)
{
  
   //Recursive yani kendi kendini çağıran bir metot.
int leftEnd; //Bu aslında ikiye bölünmüş listenin, birinci parçasının yani sol listenin son indeksini temsil ediyor.
   if (left < right)
   {
       leftEnd = (left + right) / 2;
//Sol listeyi sürekli 2'ye bölüyoruz.
       Sort(array, left, leftEnd);
//Sağ listeyi sürekli 2'ye bölüyoruz.
       Sort(array, (leftEnd + 1), right);
DoMerge(array, left, (leftEnd + 1), right);
Printer.Print(array);
   }
}
public static void DoMerge(int[] array, int left, int rightStart, int right)
{
    int[] temp = new int[array.Length];
    int leftEnd, numElements, tempLeft;
tempLeft = left;
    leftEnd = rightStart - 1;
    numElements = right - left + 1;
while ((left <= leftEnd) && (rightStart <= right))
    {
        if (array[left] <= array[rightStart])
        {
            temp[tempLeft++] = array[left++];
        }
        else
        {
            temp[tempLeft++] = array[rightStart++];
        }
    }
    while (left <= leftEnd)
    {
        temp[tempLeft++] = array[left++];
    }
while (rightStart <= right)
    {
        temp[tempLeft++] = array[rightStart++];
    }
for (int i = 0; i < numElements; i++)
    {
        array[right] = temp[right];
        right--;
    }
}
``` 


Yukarıda eklediğim github linki üzerinden uygulamayı indirip. Tek tek incelemek, daha iyi kavranmasını sağlanabilir.

Bazı sıralama algoritmalarını direkt kodun içerisinde bıraktım buraya eklemedim.

Bazılarının da ikinci versiyonlarını ekledim.

Umarım ufakta olsa faydam dokunmuştur. Bir sonraki yazıda görüşmek üzere.

**Kaynaklar;  
**[https://www.wikiwand.com/tr/S%C4%B1ralama\_algoritmas%C4%B1](https://www.wikiwand.com/tr/S%C4%B1ralama_algoritmas%C4%B1) [https://www.halildurmus.com/2021/02/22/siralama-algoritmalari-sorting-algorithms/](https://www.halildurmus.com/2021/02/22/siralama-algoritmalari-sorting-algorithms/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647971961338/tapa89bPK.gif)