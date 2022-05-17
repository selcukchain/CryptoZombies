**Bölüm 7: Depolamaya Karşı Bellek (Veri konumu)**

Solidity’de değişkenler iki şekilde saklanabilir.Bunlar Storage(Depolama) ve Memory( Bellek).

>Storage(Depolama), blok zincirinde kalıcı olarak depolanan değişkenleri ifade eder. Bilgisayarımızın Sabit diski gibi düşünülebilir.
>Memory(Bellek) değişkenleri geçicidir ve sözleşmenize yapılan harici işlev çağrıları arasında silinir. Bilgisayarımızın RAM’i gibi düşünülebilir.

Solidity bunları varsayılan olarak işlediğinden çoğu zaman bu anahtar kelimeleri kullanmanıza gerek yoktur. 
Durum değişkenleri(State variables: fonksiyonların dışında bildirilen değişkenler) varsayılan olarak depolanır ve blok zincirine kalıcı olarak yazılırken, fonksiyonların içinde bildirilen değişkenler hafızadır ve fonksiyon çağrısı sona erdiğinde kaybolur.

Ancak, bu anahtar kelimeleri, yani işlevler içindeki yapılar ve dizilerle uğraşırken kullanmanız gereken zamanlar vardır:
```
contract SandwichFactory {
  struct Sandwich {
    string name;
    string status;
  }

  Sandwich[] sandwiches;

  function eatSandwich(uint _index) public {
    // Sandwich mySandwich = sandwiches[_index];

    // oldukça tatlı görünüyor fakat solidty burada bize bir uyarı verecektir.
    // burada açıkça `storage` veya `memory` tanımlamamız gerekmektedir.

    // Bunun yerine aşağıdaki `storage` anahtar kelimesini kullanalım:
    Sandwich storage mySandwich = sandwiches[_index];
    // Bu durumda `mySandwich` ,`sandwiches[_index]` için bir işaretçi oldu.
    // storage içinde, ve
    mySandwich.status = "Eaten!";
    //  `sandwiches[_index]` blockchain içinde kalıcı olarak değişecektir.

    // sadece bir kopya istiyorsanız `memory` kullanabilirsiniz:
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // sadece bir kopya istiyorsak `anotherSandwich` anahtar kelimesini kullanabiliriz 
    // bellekteki verilerde sadece geçici değişkenleri değiştirecek ve hiçbir etkisi olmayacak
    anotherSandwich.status = "Eaten!"; 
    // `sandwiches[_index + 1]` üzerinde fakat aşağıdaki gibi yazarsak değişikliklikleri blockchain deposunda geri kopyalabiliriz.
    sandwiches[_index + 1] = anotherSandwich;
 
  }
}
```
**Hangisini ne zaman kullanacağınızı henüz tam olarak anlamamamış olabilirsiniz lakin  bu eğitim boyunca siz Solidity derleyicisi ne zaman depolamayı ve ne zaman belleği kullanacağınzı bildirecek ve bizlerde uyaracağız.**

Şimdilik, depolama veya bellek bildirmeniz gereken durumların olduğunu bilmemiz yeterlidir.

**Uygulama**
Zombilerimize beslenme ve çoğalma yeteneği vermenin zamanı geldi!!!

Bir zombi başka bir yaşam formuyla beslendiğinde, DNA'sı yeni bir zombi oluşturmak için diğer yaşam formunun DNA'sı ile birleşir.

Öncelikle “feedAndMultiply” adlı bir fonksiyon oluşturun. 
İki parametre alacaktır: 
_zombieId (bir uint) 
_targetDna (ayrıca bir uint). Bu işlev public olmalıdır.

Zombimizi başka birinin beslemesine izin vermek istemiyoruz! İlk önce, bu zombiye sahip olduğumuzdan emin olalım. msg.sender öğesinin bu zombinin sahibine eşit olduğunu doğrulamak için bir require ifadesi ekleyin (createRandomZombie işlevinde yaptığımıza benzer).

>Not: Yine, yanıt denetleyicimiz ilkel olduğundan, msg.sender'ın önce gelmesini bekler ve sırayı değiştirirseniz yanlış işaretler. Ancak normalde kod yazarken, tercih ettiğiniz sırayı kullanabilirsiniz - ikisi de doğrudur.

Bu zombinin DNA'sını almamız gerekecek. Bu yüzden fonksiyonumuzun yapması gereken bir sonraki şey, myZombie adlı yerel bir Zombie (bir depolama işaretçisi olacak) bildirmektir. Bu değişkeni, zombi dizimizde _zombieId dizinine eşit olacak şekilde ayarlayın.

>Şu ana kadar } kapanışını içeren satır da dahil olmak üzere 4 satır kodunuz olmalıdır.

**Bu fonksiyonun  detayları bir sonraki bölümde devam edecektir.**

```
pragma solidity >=0.5.0 <0.6.0;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

  // Start here
  function feedAndMultiply (uint _zombieId, uint _targetDna ) public{
       require(msg.sender == zombieToOwner[_zombieId]);
       Zombie storage myZombie== zombies[_zombieId];
        
  }

}
```
