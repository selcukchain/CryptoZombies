 
**Bölüm 8:Zombie DNA**

Hadi “FeedAndMultiply” fonksiyonunu yazmayı bitirelim.

Yeni bir zombinin DNA'sını hesaplamanın formülü basittir: Beslenen zombinin DNA'sı ile hedefin DNA'sı arasındaki ortalama bize yeni zombinin DNA’sını verecek.
newZombieDna = (zombieDna + targetDna) / 2 

Örneğin:

    function testDnaSplicing() public {
    uint zombieDna = 2222222222222222;
    uint targetDna = 4444444444444444;
    uint newZombieDna = (zombieDna + targetDna) / 2;
     // 3333333333333333 eşit olacak
    }        

Daha sonra, yeni zombinin DNA'sına biraz rastgelelik eklemek gibi, istersek formülümüzü daha karmaşık hale getirebiliriz. Ancak şimdilik basit tutacağız - daha sonra her zaman geri dönebiliriz.


**Uygulama**

Öncelikle _targetDna'nın 16 basamaktan uzun olmadığından emin olmamız gerekiyor. Bunu yapmak için, _targetDna'yı yalnızca son 16 haneyi alacak şekilde _targetDna % dnaModulus'a eşit olarak ayarlayabiliriz.

Ardından, fonksiyonumuz newDna adında bir uint bildirmeli ve bunu myZombie'nin DNA'sının ve _targetDna'nın ortalamasına eşit olarak ayarlamalıdır (yukarıdaki örnekte olduğu gibi).

>Not: myZombie'nin özelliklerine myZombie.name ve myZombie.dna kullanarak erişebilirsiniz.

Yeni DNA'ya sahip olduğumuzda, _createZombie'yi arayalım. Bu fonksiyonun hangi parametreleri çağırması gerektiğini unutursanız, zombiefactory.sol sekmesine bakabilirsiniz. Bir isim gerektirdiğini unutmayın, bu yüzden yeni zombimizin adını şimdilik "NoName" olarak ayarlayalım - daha sonra zombilerin isimlerini değiştirmek için bir fonksiyon yazabiliriz.


>Not: Solidity meraklıları için, burada kodumuzla ilgili bir sorun fark edebilirsiniz! Endişelenme, bunu bir sonraki bölümde düzelteceğiz;)


```
pragma solidity >=0.5.0 <0.6.0;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    // start here
    _targetDna = _targetDna % dnaModulus;
        uint newDna = (myZombie.dna + _targetDna) / 2;
        _createZombie("NoName", newDna);

  }

}

```
