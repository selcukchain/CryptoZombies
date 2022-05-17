**Bölüm 5: Miras(Inheritance)**
 
Zombi Oyun kodumuz gittikçe uzamakta bu yüzden son derece uzun bir kontrat yapmak yerine, bazen kodu düzenlemek için kod mantığınızı birden çok kontrata bölmek mantıklıdır.

>Solidity'nin, kontrat miras(contrat inheritance) özelliği ile daha yönetilebilir bir özellik sunmaktadır.

    contract Doge {
     function catchphrase() public returns (string memory) {
       return "So Wow CryptoDoge";
     }
    }

    contract BabyDoge is Doge {
      function anotherCatchphrase() public returns (string memory) {
       return "Such Moon BabyDoge";
    }
    }


BabyDoge, Doge'dan miras alır. Bu, BabyDoge'u derler ve dağıtırsanız, onun hem **catchphrase()**'e hem de **otherCatchphrase()**'e (ve Doge'da tanımlayabileceğimiz diğer genel işlevlere) erişimi olacağı anlamına gelir.

Bu, mantıksal kalıtım için kullanılabilir (örneğin, bir alt sınıfla birlikte, bir Kedi bir Hayvandır). Ancak benzer mantığı farklı kontratlarda gruplandırarak kodunuzu düzenlemek için de kullanılabilir.

**Uygulama**

Sonraki bölümlerde, zombilerimizin beslenmesi ve çoğalması için işlevselliği uygulayacağız. Bu mantığı ZombieFactory'den tüm yöntemleri miras alan kendi kontratımıza koyalım.

>ZombieFactory'nin altında ZombieFeeding adlı bir kontrat yapın. Bu kontrat ZombieFactory sözleşmemizden miras kalmalıdır.



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

    // Start here
    //ZombieFactory'nin altında ZombieFeeding adlı bir kontrat yapıldı.
    contract ZombieFeeding is ZombieFactory{
    
    }



