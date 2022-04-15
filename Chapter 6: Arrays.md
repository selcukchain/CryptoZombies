**Chapter 6: Arrays(Dizin)**
                 

Bir şeyin koleksiyonunu istediğinizde, bir array kullanabilirsiniz. Solidity'de iki tür array vardır: fixed(sabit) arrays ve dynamic(dinamik) arrays:

>sabit uzunlukta 2 elemanlı bir array tanımı
      
      uint[2] fixedArray;

>başka bir sabit dizi,5 array içerebilir:

     string[5] stringArray;
 
>Dynamic(dinamik) array-sabit bir boyutu yotur,büyümeye devam eden bir yapıya sahiptir:

    uint[] dynamicArray;

>Ayrıca önceki kişi yapısı kullanılarak da bir array oluşturulabilir:

    Person[] people; // dynamic Array, eklemeye devam etmek için kullanılabilir.


State variables(sabit değişkenler) ise blok zincirinde kalıcı olarak saklanır.Dolayısışya dinamik bir dizi oluşturmak ,bir tür veritabanı gibi , kontratlarda yapılandırılmış verileri depolamak için yararlı olabilir.

**Public arrays**

Bir diziyi Public olarak bildirebilirsiniz ve solidity bunu için otomatik olarak bir alıcı yöntemi oluşturur.

>Örnek

    Person[] public people;


Diğer kontratlar daha sonra bu diziden okuyabilir, ancak yazamaz. Dolayısıyla bu, sözleşmenizde herkese açık verileri depolamak için kullanışlı bir kalıptır.

**Uygulama**

Gelelim uygulamamıza, bir zombi ordusu depolamak isteyeceğiz. Ve tüm zombilerimizi diğer uygulamalara göstermek isteyeceğiz, bu yüzden herkese açık olmasını istiyoruz.

>Herkese açık bir Zombie array  oluşturmamızı ve buna zombies adını vermemiz isteniliyor.

    pragma solidity >=0.5.0 <0.6.0;
    contract ZombieFactory {

       uint dnaDigits = 16;
       uint dnaModulus = 10 ** dnaDigits;

        struct Zombie {
          string name;
          uint dna;
         }
           // herkese açık Zombie ordusuna ait zombiler 
      Zombie[] public zombies;
     }


