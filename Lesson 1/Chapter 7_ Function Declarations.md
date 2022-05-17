**Chapter 7: Function Declarations(İşlev Bildirimleri)**

>Solidity de bir Function Declaration işlemi aşağıdaki gibi görünür.

    function eatHamburgers(string memory _name, uint _amount) public {

     }

EatHumburgers adlı fonksiyon 2 parametre alan bir işlemdir:bir string ve bir uint.Public olarak belirlendiğini unutmamak gerek. Ayrıca _name yazarak bellekteki yeri için talimat da sağladık.Bu arrays,structs,mapping ve strings için gereklidir.

**Sorduğumuz referans türü nedir?**

Bir Solidity işlevine bir argüman iletmenin iki yolu vardır:

>Değere göre: Solidity derleyicisinin parametre değerinin yeni bir kopyasını oluşturduğu ve bunu işlevinize ilettiği anlamına gelir. Bu, fonksiyonunuzun, ilk parametrenin değerinin değişmesinden endişe etmeden değeri değiştirmesine izin verir.

>Referans olarak: Yani fonksiyonunuzun orijinal değişkene referansla çağrıldığı anlamına gelir. Böylece, fonksiyonunuz aldığı değişkenin değerini değiştirirse, orijinal değişkenin değeri de değişir.

Not: Genel değişkenlerden ayırt etmek için işlev parametresi değişken adlarını alt çizgi (_) ile başlatmak bir kuraldır (ancak gerekli değildir). Bu kuralı eğitimimiz boyunca kullanacağız. 

Bu işlevin çağırlması aşağıda verilmiştir.

      eatHamburgers("vitalik", 100);

**Uygulama**

Görevimize gelirsek eğer Uygulamamızda bazı zombiler yaratabilmemiz gerekecek. Bunun için bir fonksiyon oluşturalım.

>createZombie adlı bir public function oluşturun. _name (a string), and _dna (a uint) adlı iki parametre vereceğiz. memory anahtar sözcüğünü kullanarak değere göre ilk argümanı iletmeyi unutmayın ve body kısmını daha sonra doldurmak üzere boş bırakın.

    pragma solidity >=0.5.0 <0.6.0;

    contract ZombieFactory {

       uint dnaDigits = 16;
       uint dnaModulus = 10 ** dnaDigits;

       struct Zombie {
          string name;
          uint dna;
         }

       Zombie[] public zombies;

       // Public olarak oluşturulan createZombie function metodu
       function createZombie(string memory_name, uint _dna) public {
         }
       }
       
