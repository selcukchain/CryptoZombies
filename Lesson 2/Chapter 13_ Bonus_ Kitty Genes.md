**Bölüm 13: Bonus: Kitty Genleri**

Fonksiyon mantığımız artık tamamlandı... ama bir bonus özellik ekleyelim.

>Kittylerden yapılan zombilerin **cat-zombies** olduklarını gösteren bazı benzersiz özelliklere sahip olmasını sağlayalım.

**Bunu yapmak için, zombinin DNA'sına bazı özel kedicik kodları ekleyebiliriz.**

>1. dersten hatırlarsanız, şu anda zombinin görünümünü belirlemek için 16 basamaklı DNA'mızın ilk 12 basamağını kullanıyoruz. Öyleyse, "özel" özellikleri işlemek için kullanılmayan son 2 basamağı kullanalım.

cat-zombies için DNA'larının son iki basamağı olarak 99'a sahip olduklarını söyleyeceğiz (çünkü kedilerin 9 canı vardır (ehe) ). 

**Yani kodumuzda, eğer bir kediden bir zombi gelirse diyeceğiz, sonra DNA'nın son iki basamağını 99 olarak ayarlayacağız.**

>Solidity'deki if ifadeleri aynı javascript'e benziyorsa:

```
function eatBLT(string memory sandwich) public {
  //eşitliği kontrol etmememiz için dizelerin keccak256 hashlerini karşılaştırmamız gerekiyor
  
  if (keccak256(abi.encodePacked(sandwich)) == keccak256(abi.encodePacked("BLT"))) {
    eat();
  }
}
```
**Uygulama**

Kedi genlerini zombi kodumuza uygulayalım.

İlk olarak, feedAndMultiply için işlev tanımını değiştirelim, böylece 3. bir argüman alır: bellekte depolayacağımız _species adlı bir dize.

Sonra, yeni zombinin DNA'sını hesapladıktan sonra, _species'in keccak256 karmalarını ve "kitty" dizesini karşılaştıran bir if ifadesi ekleyelim. Dizeleri doğrudan keccak256'ya geçiremiyoruz. Bunun yerine, sol tarafa bir argüman olarak abi.encodePacked(_species) ve sağ tarafa bir argüman olarak abi.encodePacked("kitty") geçeceğiz.

if ifadesinin içinde DNA'nın son 2 basamağını 99 ile değiştirmek istiyoruz. Bunu yapmanın bir yolu şu mantığı kullanmaktır: newDna = newDna - newDna % 100 + 99;.

>çıklama: newDna'nın 334455 olduğunu varsayın. O zaman newDna % 100 55'tir, yani newDna - newDna % 100 334400'dür. Son olarak 334499 elde etmek için 99 ekleyin.

Son olarak, feedOnKitty içindeki işlev çağrısını değiştirmemiz gerekiyor. FeedAndMultiply'ı çağırdığında, sonuna "kitty" parametresini ekleyin.


```

pragma solidity >=0.5.0 <0.6.0;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes

  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  // Modify function definition here:
  function feedAndMultiply(uint _zombieId, uint _targetDna, string memory_species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    // Add an if statement here
    if (keccak256(abi.encodePacked(_species)) == keccak256(abi.encodePacked("kitty"))){
     newDna = newDna - newDna % 100 + 99;

    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    // And modify function call here:
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }

}



```



