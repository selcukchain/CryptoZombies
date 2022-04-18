Chapter 5: Structs(Yapılar)

Birden çok özelliğe sahip daha karmaşık veri türleri yazımına ihtiyaç duyulur ve solidty de karmaşık veri türleri oluşturmamız için devreye Struct’lar girer. Structlar, farklı veri tipindeki değişkenleri bir yapı altında toplayıp, bu özelliklere yapı üzerinden erişilmesine izin verir.

    struct Person {
      uint age;
      string name;
     }

Burada yeni bir type,string belirttiğimizi unutmayalım.Stringler rastgele uzunlutaki UTF-8 verileri için kullanılır.

>Örneğin

    tring greeting = "Hello world!"


**Uygulama**

Ordumuz için bazı zombiler yaratmak isteyeceğiz ve zombilerin birden fazla özelliği olacaktır.Bu nedenle burada Zombie struct olarak name (a string) ve dna (a uint) oluşturmamız istenmektedir.

	pragma solidity >=0.5.0 <0.6.0;
	
	contract ZombieFactory {
	
	    uint dnaDigits = 16;
	    uint dnaModulus = 10 ** dnaDigits;
	    	    // Zombi struct yapısı
     struct Zombie {

         string name;
         uint dna;
	        }

