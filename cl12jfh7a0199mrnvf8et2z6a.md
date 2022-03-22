## WSL 2 Linux ve Docker özelinde bir kaç sorun ve çözümü

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972028709/f3TZ88mys.jpeg)

Öncelikle bu yazının, **WSL2 -Linux -Docker** üçlüsünü kullanırken **yaşadığım sorunların çözümünü** içerdiğini belirtmek isterim.

**WLS 2 ile ilgili ayrıntılı bilgiyi** aşağıdaki [Deniz İrgin](https://medium.com/u/5512c90ef9e0)’in yazısından edinebilirsiniz. **Kesinlikle çok güzel bir yazı ve mutlaka okunmalıdır.** Yazıyı okurken bazı kısımlarda belirtilen **preview** kısımlarının artık **preview** olmadığını belirtmek isterim.

[Windows Subsystem for Linux 2 (WSL 2) — En büyük aşklar kavgayla başlar](https://medium.com/codefiction/windows-subsystem-for-linux-2-wsl-2-en-b%C3%BCy%C3%BCk-a%C5%9Fklar-kavgayla-ba%C5%9Flar-4cdc11770c7f)

#### Neden WSL2 Linux Kullanmaya Başladım?

Windows’un lüksünden ve yıllardır ara ara kafa dağıtmak için oynadığım LOL oyunundan vazgeçmemek için hiçbir zaman tam anlamıyla Linux’a geçmek gibi bir planım olmadı ama ilgimi de çekmiyor değildi.

Üşengeçlikten olsa gerek ki dual boot ya da sanal makine ile uğraşıp Linux kurma zahmetine de girmedim.

Lakin internette **Docker** üzerine bir şeyler okurken anlatımların Linux ya da macOS üzerinden olduğu makalelerde zorlanmaya başladığımı ve komutlar ile ilgili kısımların Windows uyarlamalarını bulmak iyiden iyiye canımı sıkmaya başladığını görünce artık dual boot kurma vaktinin geldiğini düşünmeye başladım.

Daha sonra Linux dual boot ve sanal makine üzerine bir şeyler araştırırken tesadüfen WSL ile tanıştım ve aşık oldum. **Artık hem Windows’un tadını çıkara bilecek hem de Linux kullanmak istediğimde bunu zahmetsizce Windows üzerinden halledebilecektim…**

Linux tecrübesi olanlar için sorunlar ufak ve çözümleri basit gibi görünse de yıllardır Windows kullanan benim için bu sorunların bazıları çok zor oldu ve [**notion.so**](https://www.notion.so/) üzerinde ki notlarımı bir araya toplayıp paylaşma gereği hissettim.

#### **Sorun 1 : İnternete çıkamama problemi**

Bunu Ubuntu üzerinde apt updatekomutunu çalıştırıp sistemi güncellemek istediğimde fark ettim. Daha sonrasında internetten bazı çözümler buldum fakat bunlarda her Ubuntuyu yeniden başlattığımda sıfırlanıyordu. En en önemli olan son satırı bulmak zor oldu :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972030136/LObhyJN6T.png)

#### **Çözüm 1:**

Aşağıdaki kodları ister sırası ile isterseniz tek satır olarak çalıştırıp kalıcı çözüm elde edebilirsiniz.


```
sudo -i #root'a geçmek için. 172.19.150.144
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "192.168.1.1" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
sudo chattr +i /etc/resolv.conf # Ayarları kalıcılaştırmak için dosyayı kitleme işlemi

#Tek Satır hali.
sudo rm /etc/resolv.conf && bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf' && bash -c 'echo "192.168.1.1" > /etc/wsl.conf' && bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf' && chattr +i /etc/resolv.conf
``` 


#### **Sorun 2: Her seferinde root’a geçme zahmeti**

Gerek Docker kullanırken gerekse ihtiyaç doğrultusunda klasör, dosya oluştururken sürekli izne takılıyordum ve **sudo -i** komutu ile root’a geçmem ve parola girmem gerekiyordu. Bunun bir çözümü olmalıydı.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972031333/tlbc4wYha.png)

#### Çözüm 2:

**Windows komut satırında** aşağıdaki kodu çalıştırmak bizi sürekli root’a geçme zahmetinden kurtarır ve Ubuntuyu artık her açılışta root kullanıcısı ile başlatır.

```
ubuntu.exe config — default-user root

```
Eğer tekrar normal kullanıcıya geçmek istersek **su - kullaniciAdi** komutu kullanılabilir.

#### **Sorun 3 : Docker Volume’ün konumunu bulma sorunu**

**Entegre bir Linux ile çalıştığımızı unutmamız gerekiyor.** Ubuntu ile Docker üzerinde çalışıyorsak eğer **docker volume create v\_Test** komutu ile bir volume oluşturup **docker inspect v\_Test** komutu ile oluşturduğumuz volume’ün yerini bulmak istediğimde o konumda olmadığını fark ettim ve bu durum Nginx ile çalışırken canımı sıktı ve oldukça vakit kaybettirdi.

#### **Çözüm 3:**

4.satırda bulunan komut ile volume’lerimize ulaşabiliriz. İlgili dosya yoluna ister **windwos + r** istersek de doğrudan adres çubuğuna yazarak erişebiliriz.


```
#Otomatik Oluşan Yol
/var/lib/docker/volumes/v_Test/_data
#Windows Yolu 
\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes\
``` 


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972032705/96rLUHRcx.png)

#### Sorun 4: Ubuntu’nun Docker daemon’a bağlanamama sorunu (Docker in Docker )

Sorunu biraz detaylandıracak olursam; örneğin Docker içerisine **Portainer** ve ya **Jenkins** kurduğumda ilk etapta sorunsuz çalışsa da ertesi gün kaldığım yerden devam etmek istediğimde her şeyin alt-üst olduğunu ve WSL2 Ubuntuyu baştan kurmama sebep olacak kadar sıkıntılı bir süreçle karşı karşıya kalıyordum. Bu durum oldukça motivasyonumun kırılmasına sebep oluyordu. Çözümü ise oldukça basitmiş.

**Çözüm 4:**

Linux üzerinde çalıştırmamız gereken kodları sırasıyla aşağıya ekliyorum. Bu kodları çalıştırdıktan sonra gönül rahatlığıyla Docker In Docker yapabiliriz :)


```
DOCKER_HOST=tcp://localhost:2375
DOCKER_BUILDKIT=1
``` 


Yeni sorunlarla karşılaşmam durumunda burayı güncelliyor olacağım.

Umarım ufakta olsa faydam dokunmuştur. Bir sonraki yazıda görüşmek üzere.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1647972034050/ux4vYConZ.gif)