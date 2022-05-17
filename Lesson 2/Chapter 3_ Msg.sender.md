**Bölüm 3: Msg.sender**

Artık bir zombinin kime ait olduğunu takip etmek için eşlemelerimiz(mapping) olduğuna göre, onları kullanmak için _createZombie yöntemini güncellemek isteyeceğiz.

>Bunu yapmak için msg.sender adında global değişkeni kullanmamız gerekiyor.

**msg.sender**
Solidity'de, tüm fonksiyonlar için kullanılabilen belirli global değişkenler vardır. Bunlardan biri, mevcut işlevi çağıran kişinin (veya akıllı kontratın) adresini ifade eden msg.sender'dır.

>Not: Solidity'de, fonksiyon yürütmenin her zaman harici bir arayan(caller) ile başlaması gerekir. Bir kontrat, biri onun fonksiyonlarından birini çağırana kadar hiçbir şey yapmadan blokchainde duracaktır. 
**Yani her zaman bir msg.sender olacak**.
```
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ The syntax for storing data in a mapping is just like with arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Retrieve the value stored in the sender's address
  // Will be `0` if the sender hasn't called `setMyNumber` yet
  return favoriteNumber[msg.sender];
}
```
Bu örnekte, herhangi biri setMyNumber'ı arayabilir ve kontratımızın adresine bağlı olacak bir uint saklayabilir. Sonra whatIsMyNumber'ı aradıklarında, depoladıkları uint'e iade edilebilir.

>**Msg.sender'ı kullanmak size Ethereum blok zincirinin güvenliğini sağlar - birinin başka birinin verilerini değiştirmesinin tek yolu, Ethereum adresiyle ilişkili özel anahtarı çalmak olacaktır.**

**Uygulama**
Zombinin sahipliğini fonksiyonu çağıran kişiye atamak için ders 1'deki _createZombie yöntemimizi güncelleyeceğiz.

>İlk olarak, yeni zombinin kimliğini geri aldıktan sonra, bu id altında msg.sender'ı depolamak için zombieToOwner eşlememizi güncelleyelim.

>İkinci olarak, bu msg.sender için OwnerZombieCount değerini artıralım.

**Solidity'de bir uint'i javascript'te olduğu gibi ++ ile artırabilirsiniz:**


    uint number = 0;
    number++;
    // "sayı" artık "1"

>Bu bölüm için son cevabınız 2 satır kod olmalıdır.
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

    function _createZombie(string memory _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        // start here
        //
        zombieToOwner[id]=msg.sender;
        //
        ownerZombieCount[address]++;

        emit NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

    }

```
