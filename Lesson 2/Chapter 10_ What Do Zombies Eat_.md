**Bölüm 10: Zombiler Ne Yiyor?**

Zombilerimizi beslemenin zamanı geldi! 

Zombiler en çok ne yemeyi sever?

>Şey, CryptoZombieler KriptoKitty  yemeyi sever 😱😱😱
(Evet ciddiyim 😆 )

Bunu yapmak için CryptoKitties akıllı sözleşmesinden kittyDna'yı okumamız gerekecek. Bunu yapabiliriz çünkü CryptoKitties verileri blok zincirinde açık bir şekilde depolanır. Blok zinciri harika değil mi?!

**__Endişelenme - oyunumuz aslında kimsenin CryptoKitty'sine zarar vermeyecek. Sadece CryptoKitties verilerini okuyoruz, gerçekten silemiyoruz 😉__**

Diğer kontratlarla etkileşim

kontratımızın, sahip olmadığımız blok zinciri üzerinde başka bir kontratla konuşması için önce bir arayüz tanımlamamız gerekiyor.

Basit bir örneğe bakalım. Diyelim ki blok zincirinde şuna benzeyen bir sözleşme var:
```
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```
Bu, herkesin şanslı numarasını saklayabileceği basit bir kontrat olacak ve Ethereum adresiyle ilişkilendirilecek. O zaman başka biri, adresini kullanarak o kişinin şanslı numarasını arayabilir.

Şimdi diyelim ki bu kontrattaki verileri getNum fonksiyonunu kullanarak okumak isteyen harici bir kontratımız var.

Öncelikle LuckyNumber kontratının bir arayüzünü tanımlamamız gerekir:
```
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
**Uygulama**
Sizin için CryptoKitties kaynak kodunu aradık ve "genleri" de dahil olmak üzere kedinin tüm verilerini döndüren getKitty adında bir fonksiyon bulduk (zombi oyunumuzun yeni bir zombi oluşturması için gereklidir!).

>Fonksiyon şöyle görünür:
```
function getKitty(uint256 _id) dış görünüm döndürür (
    bool Gestasyonel,
    bool Hazır,
    uint256 cooldownIndex,
    uint256 sonrakiAksiyonAt,
    uint256 siringWithId,
    uint256 doğumZaman,
    uint256 matronId,
    uint256 sireId,
    uint256 nesil,
    uint256 genleri
) {
    Kedicik saklama kiti = kedicikler[_id];

    // eğer bu değişken 0 ise, hamile değildir
    isGesating = (kit.siringWithId != 0);
    isReady = (kit.cooldownEndBlock <= blok.numara);
    cooldownIndex = uint256(kit.cooldownIndex);
    nextActionAt = uint256(kit.cooldownEndBlock);
    siringWithId = uint256(kit.siringWithId);
    doğumZamanı = uint256(kit.birthTime);
    matronId = uint256(kit.matronId);
    sireId = uint256(kit.sireId);
    nesil = uint256(kit.jenerasyon);
    genler = kit.genler;
}
```
Fonksiyon, alıştığımızdan biraz farklı görünüyor. bir sürü farklı değerin geri döndüğünü görebilirsin... Javascript gibi bir programlama dilinden geliyorsanız, bu farkı farkedebilirsiniz. 

>**Solidity'de bir fonksiyondan birden fazla değer döndürebilirsiniz.**

Artık bu fonksiyonun neye benzediğini bildiğimize göre, onu bir arayüz oluşturmak için kullanabiliriz:

>KittyInterface adlı bir arabirim tanımlayın. Unutmayın, bu tıpkı yeni bir kontrat oluşturmaya benziyor - contract anahtar sözcüğünü kullanıyoruz.

>Arayüzün içinde, getKitty fonksiyonunu tanımlayın (yukarıdaki işlevin bir kopyala/yapıştır olması gerekir, return ifadesinin parantezleirnden sonra noktalı virgül (;) koymayı unutmayın.)
```
pragma solidity >=0.5.0 <0.6.0;

import "./zombiefactory.sol";

// Create KittyInterface here

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

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}
```











