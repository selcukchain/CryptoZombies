**Chapter 8: Working With Structs and Arrays(Yapılar ve Dizilerle Çalışmak)**

**Yeni structs(yapılar) oluşturma**

>Önceden oluşturduğumuz Person Struct hatırlayalım

    struct Person {
     uint age;
     string name;
    }


>Yeni bir Person oluşturma:

    Person satoshi = Person(172, "Satoshi");

>Person’ı  array olarak ekleme:
 
    people.push(satoshi);

>Bunları bir araya getirerek daha temiz bir kod dizimi elde edebiliriz:

    people.push(Person(16, "Vitalik"));

“array.push()” öğesinin dizisi ögeleri eklediğimiz sırada alır.

>Aşağıdaki örneğe bakın

    uint[] numbers;
    numbers.push(5);
    numbers.push(10);
    numbers.push(15);
    
    //sayılar artık [5, 10, 15]'e eşittir.

**Uygulama**

Sıra geldi Zombi ordumuzdaki kodlara

Oluşturduğumuz CreateZombie fonksiyonu üzerinde yeni bir zombie oluşturmak için zombie array ögesini oluşturmamız istenmektedir. 

>Yeni Zombie’nin name ve dna’sı fonksiyon argümanlarından gelmesi istenmektedir ve bunu tek bir kod satırında yapalım.

    pragma solidity >=0.5.0 <0.6.0;

    contract ZombieFactory {

        uint dnaDigits = 16;
        uint dnaModulus = 10 ** dnaDigits;

        struct Zombie {
          string name;
           uint dna;
         }

         Zombie[] public zombies;

         function createZombie (string memory _name, uint _dna) public {
       
         // Zombie’lere name ve dna argümanlarının entegresi
         
          zombies.push(Zombie(_name,_dna));
         }

      }
