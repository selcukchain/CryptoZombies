
Chapter 2: Contracts (Kontratlar)

Bir Contract(kontrat), Ethereum uygulamalarının temel yapı taşıdır . Solidity'nin kodu kontratlarda yer almaktadır. Tüm değişkenler ve işlevler bir kontrata aittir ve bu tüm projelerinizin başlangıç noktası olmaktadır.

>HelloWorld adlı bir boş sözleşme yapmak istersek 

    pragma solidity >=0.5.0 <0.6.0;

     contract HelloWorld {

     }

Her kaynak kodu pragma solidity sürüm bilgisi ile başlamalıdır.Bu sayede derleyici hangi sürümde kullanalıcağı belli olunur. Bu, potansiyel olarak kodunuzu bozabilecek değişiklikler getiren gelecekteki derleyici sürümleriyle ilgili sorunları önlemek için bir önlemdir.

>Bizler sürümü 0.5.0 (dahil) ile 0.6.0 (hariç) aralığındaki herhangi bir derleyici sürümüyle derleyebiliceğimizi belirttik.

    pragma solidity >=0.5.0 <0.6.0;

 Uygulama

Artık Zombie ordumuzun ilk basamaklarını atmaya başlayabiliriz.
Bizden ZombieFactory adlı bir kontrat oluşturmamızı ve sürüm bilgisini de solidity version >=0.5.0 <0.6.0. şeklinde ayarlanması istenmektedir.

>Bizden istenilenler aşağıdaki gibi bir kod betiği olarak yazmamız gerekmektedir.

    pragma solidity >=0.5.0 <0.6.0; 
    //Soliditynin kullanacağı versiyon bilgisi

    contract ZombieFactory {
         //ZombieFactory adlı kontrat

     }

Bitirdiğinizde, aşağıdaki "check answer"butonuna tıklayın. Sıkışırsanız, "hint" butonuna tıklayabilirsiniz.
