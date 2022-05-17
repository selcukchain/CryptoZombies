**Bölüm 9: Fonksiyon Görünürlüğü hakkında daha fazlası**

Bir önceki dersimizide bir kod hatası var!

Derlemeyi denerseniz, derleyici bir hata verecektir.

>Hatamız, ZombieFeeding içinden _createZombie fonksiyonunu çağırmayı denedik, ancak _createZombie, ZombieFactory içinde public bir fonksiyondur. Bu, ZombieFactory'den devralan hiçbir kontratın ona erişemeyeceği anlamına gelir.

**Dahili (Internal) ve Harici (External)**

>Public  ve Private özelliklerine ek olarak, Solidity'nin işlevler için iki tür görünürlüğü daha vardır: dahili(Internal) ve harici(external).

internal, private ile aynıdır, ancak bu kontratlardan devralan kontratlar tarafından da erişilebilir.
external, public işlevine benzer, ancak bu işlevler YALNIZCA kontratın dışında çağrılabilir - o sözleşmenin içindeki diğer işlevler tarafından çağrılamazlar.
 Neden External veya Public kullanmak isteyebileceğiniz hakkında daha sonra konuşacağız.

**Dahili veya harici fonksiyonları bildirmek için sözdizimi private ve public ile aynıdır:**

```
contract Sandwich {
  uint private sandwichesEaten = 0;

  function eat() internal {
    sandwichesEaten++;
  }
}

contract BLT is Sandwich {
  uint private baconSandwichesEaten = 0;

  function eatWithBacon() public returns (string memory) {
    baconSandwichesEaten++;
    // Dahili olduğu için bunu burada arayabiliriz
    eat();
  }
}
```

**Uygulama**

Sizi zaten uygun sekme olan zombiefactory.sol'e odakladık.
Diğer sözleşmemizin ona erişebilmesi için _createZombie() öğesini private'ten public olarak değiştirin.
```
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    // edit function definition below
  // function _createZombie(string memory _name, uint _dna) public { kısmını internal olarak değiştirildi
    function _createZombie(string memory _name, uint _dna) internal {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        emit NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}

```

