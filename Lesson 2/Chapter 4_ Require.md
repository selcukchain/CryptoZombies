Bölüm4:Gerektir(Require)

İlk dersimizde, kullanıcıların createRandomZombie'yi arayarak ve bir ad girerek yeni zombiler oluşturabilmelerini sağladık. Fakat, kullanıcılar ordularında sınırsız zombi yaratmak için bu işlevi çağırmaya devam etselerdi, oyun çok eğlenceli olmazdı.
Bizde daha eğlenceli hale getirmek için Her oyuncunun bu işlevi yalnızca bir kez çağırabilmesini sağlayım. Bu şekilde yeni oyuncular, ordularında ilk zombiyi yaratmak için uğraşması ve öğrenmesi gereksin.

Bu şekilde yeni oyuncular, ordularında ilk zombiyi yaratmak için oyuna ilk başladıklarında onu arayacaklardır.

**Bu fonksiyonun  oyuncu başına yalnızca bir kez çağrılabilmesini nasıl sağlarız?**

Require fonksiyonu burada devreye girmektedir, require fonksiyonu bir hata oluşturmasını ve bazı koşullar doğru değilse yürütmeyi durdurmasını sağlar:

    function sayHiToVitalik(string memory _name) public returns (string memory) {
 
    // _name'nin "Vitalik"e eşit olup olmadığını karşılaştırır. Bir hata verir ve doğru değilse çalışma durur.
  
  >Yan not: Solidity'nin yerel dize karşılaştırması yoktur, bu nedenle dizelerin eşit olup olmadığını görmek için keccak256 haslerini karşılaştırın.
          
    require(keccak256(abi.encodePacked(_name)) == keccak256(abi.encodePacked("Vitalik")));
 
     // Doğruysa, fonksiyona devam edin:
 
     return "Hi!";
    }

Bu fonksiyon **sayHiToVitalik("Vitalik")** ile çağırırsanız, **"Hi!"** döndürür. Başka bir girdi ile çağırırsanız, bir hata verir ve yürütülmez.

Bu nedenle, bir fonksiyonu çalıştırmadan önce belirli doğrulukları test etmek için kullanmak oldukça kullanışlıdır.

**Uygulama**

Zombi oyunumuzda, kullanıcının createRandomZombie tekrar arayarak ordusunda sınırsız zombi yaratabilmesini istemiyoruz.

>Require fonksiyonu ile kullanıcının (createRandomZombie) ilk zombilerini ürettiklerinde bunu bir kez kullandıklarından  emin olanacak şekilde fonksiyon oluşturalım .
createRandomZombie'nin başına bir require ifadesi koyun. Fonksiyon, OwnerZombieCount[msg.sender] öğesinin 0'a eşit olduğundan emin olmak için kontrol etmelidir ve aksi takdirde bir hata vermelidir.

>Not: Solidity'de hangi terimi ilk sıraladığınızın önemi yoktur. Bununla birlikte, yanıt denetleyicimiz gerçekten basit olduğundan, yalnızca bir yanıtı doğru olarak kabul edecektir 

Projemiz,ilk önce OwnerZombieCount[msg.sender] öğesinin gelmesini bekliyor.

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
        // start here
        require(ownerZombieCount[msg.sender]==0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

    }

