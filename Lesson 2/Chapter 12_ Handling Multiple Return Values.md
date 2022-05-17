** Bölüm 12: Çoklu Dönüş Değerlerini(Multiple Return Values) Kullanma**

getKitty fonksiyonu, birden çok değer döndüren gördüğümüz ilk örnektir. 

>Onlarla nasıl başa çıkacağımıza bakalım:

```
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // Çoklu atamayı şu şekilde yaparsınız:
  (a, b, c) = multipleReturns();
}

// Ya da değerlerden yalnızca birini önemsiyorsak:
function getLastReturnValue() external {
  uint c;
  // Diğer alanları boş bırakabiliriz:
  (,,c) = multipleReturns();
}
```
**Uygulama**
CryptoKitties kontratımızla etkileşime geçme zamanı!

**Kitty genlerini sözleşmeden alan bir fonksiyon yapalım:**

feedOnKitty adlı bir işlev yapın. _zombieId ve _kittyId olmak üzere 2 uint parametresi alacaktır ve genel bir fonksiyon olmalıdır.

**Fonksiyon önce kittyDna adında bir uint bildirmelidir.*

>Not: KittyInterface'imizde genler bir uint256'dır - ancak ders 1'e geri dönerseniz, uint uint256 için bir takma addır - **aynı şeydir**.

Fonksiyonumuz daha sonra **kittyContract.getKitty** fonksiyonunu  **_kittyId** ile çağırmalı ve genleri **kittyDna**'da depolamalıdır. 
>Unutmayın - getKitty bir ton değişken döndürür. (Tam olarak 10 tane ve bunları sizin için saydım ). Ama tek umursadığımız genlerin sonuncusu. Virgüllerinizi dikkatlice sayın!

Son olarak, fonksiyon **feedAndMultiply**'ı çağırmalı ve hem **_zombieId** hem de **kittyDna**'yı iletmelidir.

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

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

  // define function here
function feedOnKitty(uint _zombieId, uint _kittyId) public{

uint kittyDna;
(,,,,,,,,, kittyDna) = kittyContract.getKitty(_kittyId);
feedAndMultiply(_zombieId, kittyDna);


}
}


```

