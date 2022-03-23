## .Net Framework için Swagger ve JWT Authentication

---
title: .Net Framework için Swagger ve JWT Authentication
published: true
date: 2020-05-19 19:33:33 UTC
tags: jwttoken,swashbuckle,swagger,jwt
canonical_url: 
---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058024560/WYRVvOugF.png)

Merhaba bu yazıda Swagger ve JWT(JSON Web Token) kavramlarından örnek bir uygulama ile bahsediyor olacağım.

**Doğrudan örnek projeyi incelemek için** [**GitHub linki.**](https://github.com/hakanyalitekin/SwaggerWithJWT)

Öncelikle kavramlardan kısaca bahsedelim.

**Swagger** : Rest API geliştirmek için gerekli bir sözleşme standardı ve bu çerçevede işlev gören yardımcı araçlar sunan bir teknolojidir. Swagger sunduğu standart ve araçlarla API tasarım, geliştirme, **dokümantasyon ve test aşamasında kolaylık sağlamaktadır.**

**JWT(JSON Web Token):** bir JSON nesnesi olarak taraflar arasında güvenli bir şekilde bilgi iletimi için tasarlanmış bir [RFC 7519](https://tools.ietf.org/html/rfc7519) standartıdır. JWT, kullanıcının doğrulanması, web servis güvenliği, bilgi güvenliği gibi birçok konuda kullanılabilir.

**Neden Swagger’ı ayrı JWT’ı ayrı anlatmadım?**  
Bunun nedeni servislerde header alanına token eklemeniz gerektiğinde Swagger’da ki ayarlamaları nasıl yapılacağını gösterebilmek. Hazır Swagger’da kullanmak için bir token’a ihtiyacımız varken neden gayet kullanışlı olan JWT den bahsetmeyeyim dedim.

Bir web api projesi oluşturabildiğinizi ya da hali hazırda bir web api projenizin olduğunu varsayıp direk nuget paket’ini indirerek başlıyorum.

Öncelikle token’ımızı oluşturmak için gerekli olan paketi Nudget’tan indiriyoruruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058026462/eOzRSpkhN.png)

Sonrasında JWT token oluşturma ve çözümleme işlemlerini yöneteceğimiz JwtManager.cs adında bir class oluşturuyoruz. İçeriğini aşağıdaki gibi ayarlıyoruz. Ufak detayları açıklama satırları olarak koda ekledim.

{% gist https://gist.github.com/hakanyalitekin/bb66949952905ddea4bc67a1c8cb0117 %}

Artık elimizde token yapımızı yönetebileceğimiz bir token manager sınıfımız olduğuna göre Authentication Attribute’ümüzü yazabiliriz. Öncelikle eğer yoksa Filters adında bir klasör oluşturup içerisine JWTAuthenticationAttribute.cs adında bir klas ekliyoruz ve içeriğini aşağıda ki gibi oluşturuyoruz. Gerekli açıklamaları yorum satırları olarak koda ekledim.

{% gist https://gist.github.com/hakanyalitekin/99e7226782f8c8b129923c88b76780ad %}

Authentication Attribute’ümüzü de yazdığımıza göre kimlik doğrulaması yapacağımız controller’larımıza gerekli eklemeleri yapıyoruz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058028002/JBnxQxxfI.png)

İlgili attribute’ü eklediğimize göre artık kimlik doğrulaması isteneceği için token alacağımız bir login metodu oluşturuyorum.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058029222/qH3kfeIxE.png)

JWT token ile ilgili tüm işlemlerimizi tamamladığımıza göre artık **Swagger** aşamasına geçebiliriz.

Bunun için **Nudget Packege Manager** ’a **Swashbuckle** yazıp aratıyorum (swagger yazarak ta aratabilirsiniz) .Net Core için olanı indirmediğinize emin olun.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058030601/Ox69VhSeW.png)<figcaption>Doğru paketi indirdiğinize emin olun.</figcaption>

Swasbuckle paketini indirdikten sonra, metotlarımızın üstüne eklediğimiz summery’lerin swagger’da anlam kazanabilmesi için bazı ayarlamalar yapmamız gerekmekte. Bu ayarlamaları aşağıda açıklamalarıyla anlatmaya çalıştım.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058032122/WDCeNW3Ql.png)

Projemizde bu ayarı yaptıktan sonra, Swasbuckle paketini indirdiğimizde **App\_Start’ın** altına eklenen **SwaggerCongfig.cs** ’in içerisinde de bazı ayarlamalar yapmamız gerekmekte. Ben beraberinde gelen açıklama satırlarını kaldırdım. Sizde kaldırmadan önce bir göz gezdirebilirseniz faydasını göreceksiniz.

{% gist https://gist.github.com/hakanyalitekin/548430f8f97f94bb73d5e9327f0d3088 %}

Artık tüm ayarlamalarımızı tamamladığımıza göre ilk testimizi gerçekleştirelim. Ben kolaylık olsun her seferinde linkin sonuna swagger yazmamak için şöyle bir şey yaptım. Bunu yapmadan direk linkin sonuna swagger yazarak swagger’a erişebilirsiniz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058033529/ismd8HN_c.png)

Eğer bu yazı ile ilerlediyseniz görmeniz gereken görüntü şu şekilde olmalı.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058034862/k51vgiLYX.png)

Şimdi JWT token yapımızın çalışıp çalışmadığını deneyelim. Öncelikle tokensız deniyorum ve aşağıda görüldüğü üzere **401 unauthorized** hatasını alıyorum.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058036188/HCxXKQE43.png)

Şimdi ise oturum açıp token ile beraber swagger’ı nasıl kullanacağımıza bakalım. Bunun için öncelikle aşağıda ki gibi oturum açıyorum.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058037616/W3Sc4Hx4z.png)

Oluşan token’ın başına Bearer yazdıktan sonra Swagger’ın api key alanına aşağıda ki gibi ekleyip Explorer’a tıklıyorum. (Tırnak işaretlerini almadığınıza emin olunuz.)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058038992/Agv0Zc6pq.png)

Tekrar HomeController altında ki Get’i tetiklediğimiz de Get metodumuzun sorunsuz bir şekilde çalıştığını görebilirsiniz.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1648058040287/ZXIkd998g.png)

Umarım ufakta olsa faydam dokunmuştur.

**Kaynaklar;**

- [JWT Authentication with Web API - .NET How?](https://dotnethow.net/jwt-authentication-with-web-api-2-1-2/)
- [Create a RESTful API with authentication using Web API and Jwt](https://developerhandbook.com/entity-framework/create-restful-api-authentication-using-web-api-jwt/)
- [domaindrivendev/Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore#install-and-enable-annotations)