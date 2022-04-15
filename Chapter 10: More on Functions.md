**Chapter 10: More on Functions( Fonksiyonlar Hakkında Daha Fazlası)**

Bu bölümde function return values(fonksiyon dönüş değerleri) ve function modifiers(fonksiyon değiştiriciler) konusunu öğreneceğiz. 

**Return Values(dönüş değerleri)**

>Bir fonksiyonlarda bir değer(value) döndürmek için bildirim şu şekilde görünür:

    string greeting = "What's up dog";

    function sayHello() public returns (string memory) {
    return greeting;
    }

Solidity’de fonksiyon bildirimi, dönüş değeri türünü içerir(örneğimizde bu bir string)

**Function modifiers (fonksiyon değiştiriciler)**


Yukarıdaki işlev aslında Solidity'deki durumu değiştirmez kısaca herhangi bir değeri değiştirmez veya hiçbir şey yazmaz.

Bu durumda, onu bir view function olarak ilan edebiliriz, yani yalnızca verileri görüntüleyebiliyor, onları değiştirmiyor:

    function sayHello() public view returns (string memory) { 
    }


Solidity ayrıca pure functions(saf fonksiyon) içerir, yani uygulamadaki hiçbir veriye bile erişmezsiniz. 

>Örenğin:

    function _multiply(uint a, uint b) private pure returns (uint) {
    return a * b;
    }

Bu fonksiyon, uygulamanın durumundan bile okumaz – dönüş değeri(return value) yalnızca fonksiyon parametrelerine bağlıdır. Yani bu durumda işlevi pure olarak ilan edeceğiz.

**Not: İşlevlerin ne zaman pure/view olarak işaretleneceğini hatırlamak zor olabilir. Neyse ki Solidity derleyicisi, bu değiştiricilerden birini ne zaman kullanmanız gerektiğini size bildirmek için uyarılar verme konusunda iyidir.**

**Uygulama**


 Bizden istenen bir string olarak rastgele DNA numarası üreten yardımcı fonksiyon (helper function) üretmek.

> _generateRandomDna adlı private bir funtion oluşturun. _str (string) adlı bir parametre alacak ve bir uint döndürecektir. _str parametresinin data lokasyonunu hafızaya almayı unutmayınız.

>Bu işlev,kontratımızın variables’larından bazılarını görüntüleyecek ancak bunları değiştirmeyecektir, bu nedenle onu view olarak işaretleyin.

**Funtion body bu noktada boş olmalıdır daha sonra doldurulacaktır.**

 	pragma solidity >=0.5.0 <0.6.0;

    contract ZombieFactory {

       uint dnaDigits = 16;
       uint dnaModulus = 10 ** dnaDigits;

        struct Zombie {
          string name;
          uint dna;
       }

      Zombie[] public zombies;

      function _createZombie(string memory _name, uint _dna) private {
         zombies.push(Zombie(_name, _dna));
       }

     //private olarak saklanan generateRandomDna memory_str string verisini uint olarak çevirme 
     function _generateRandomDna(string memory_str) private view returns (uint){
   
      }
   

      }
