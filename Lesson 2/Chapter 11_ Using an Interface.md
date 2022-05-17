Bölüm 11: Bir Arayüz(Interface) Kullanma

NumberInterface ile önceki örneğimize devam edelim, arayüzü şu şekilde tanımlamıştık:

```
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
>Bunu bir kontratta tanımlarsak aşağıdaki gibi kullanabiliriz:
```
contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ Ethereum'daki FavoriteNumber sözleşmesinin adresi
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress);
  // Şimdi `numberContract' diğer sözleşmeye işaret ediyor

  function someFunction() public {
    // Şimdi bu sözleşmeden `getNum`u çağırabiliriz:
    uint num = numberContract.getNum(msg.sender);
    // ...ve burada "num" ile bir şeyler yap
  }
}
```

Bu şekilde, kontratımız, bu fonksiyonları public veya private olarak ortaya koydukları sürece, Ethereum blockchaindeki diğer herhangi bir kontrat ile etkileşime girebilir.

**Uygulama**
CryptoKitties akıllı kontratımızdan okumak için yeni kontratımızı oluşturalım!

Kodda CryptoKitties kontratımızın adresini **ckAddress** adlı bir değişken altında sizin için kaydettik. Sonraki satırda, **kittyContract** adında bir **KittyInterface** oluşturun ve onu **ckAddress** ile başlatın 
 >tıpkı yukarıdaki **numberContract **ile yaptığımız gibi.
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
  // Initialize kittyContract here using `ckAddress` from above

  KittyInterface kittyContract = KittyInterface(ckAddress);

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}

```
