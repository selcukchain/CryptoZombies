Bölüm 2: Eşlemeler(mapping) ve Adresler(address)

Veritabanımızdaki zombileri sahiplendirerek oyunumuzu çok oyunculu hale getirelim.
>Bunu yapmak için 2 yeni veri türüne ihtiyacımız olacak: mapping(eşleme) ve address(adresler).
**Adresses(adresler)**
Ethereum blok zinciri, banka hesapları gibi düşünebileceğiniz hesaplardan oluşur. Bir hesabın bir Ether bakiyesi (Ethereum blokchainde kullanılan para birimi) vardır ve tıpkı banka hesabınızın diğer banka hesaplarına para transferi yapabilmesi gibi, diğer hesaplara Ether ödemeleri gönderip alabilirsiniz. Ayrıca Ethereum blok zincirin de 2 tür hesap(adres) vardır. Bunlar akıllı kontratların adresleri ve Ethereum kullanıcı hesap adresleridir.
Bu adresler, Ethereum ağında kimlik oluşturma, fonlarımızı yönetme ve Ethereum uygulamaları için oturum açmamıza olanak sağlar.

>Her hesabın bir banka hesap numarası gibi düşünebileceğiniz bir adresi vardır. Bu hesaba işaret eden benzersiz bir tanımlayıcıdır ve şöyle görünür:
`0x571743307078d0859A3f9C7476f898e0793b6b2A`


(Bu adres SelcukBlockchain ekibine aittir. SelcukBlockchain Ekibinden hoşlanıyorsanız, bize biraz Ether gönderebilirsiniz!😉 )

*Adreslerin küçük ayrıntılarına daha sonraki bir derste gireceğiz, ancak şimdilik bir adresin belirli bir kullanıcıya (veya akıllı bir kontrata) ait olduğunu anlamanız yeterlidir.*

Böylece zombilerimizin sahipliği için benzersiz bir kimlik olarak kullanabiliriz. Bir kullanıcı uygulamamızla etkileşim kurarak yeni zombiler oluşturduğunda, bu zombilerin sahipliğini, işlevi çağıran Ethereum adresine ayarlayacağız.

**Mappings(Eşlemeler)**
Ders 1'de yapılara(structs) ve dizileri(arrays) konularını işledik. Eşlemeler(Mappings), organize verileri Solidity'de saklamanın başka bir yoludur.

>Bir eşlemenin tanımlanması şuna benzer:
```
// Bir finansal uygulama için, kullanıcının hesap bakiyesini tutan bir uint depolamak:

mapping (address => uint) public accountBalance;

// Veya userId'ye göre kullanıcı adlarını depolamak/aramak için kullanılabilir

mapping (uint => string) userIdToName; 
```
Bir eşleme, esas olarak, verileri depolamak ve aramak için bir anahtar-değer(key-value) deposudur. İlk örnekte anahtar bir adres ve değer(a value) bir uint'tir ve ikinci örnekte anahtar bir uint ve değer bir dizedir(a string).

**Uygulama**

Zombi sahipliğini depolamak için iki eşleme kullanacağız: biri bir zombiye sahip olan adresin kaydını tutar, diğeri ise bir sahibinin kaç tane zombiye sahip olduğunu takip eder.

zombieToOwner adlı bir mapping oluşturun. Anahtar(key) bir uint (zombiyi kimliğine göre saklayacağız ve arayacağız) ve bir adres(address) değeri olacaktır. Bu haritalamayı public hale getirelim.

>Anahtarın bir adres ve değerinin bir uint olduğu OwnerZombieCount adlı bir eşleme oluşturun. Bu eşleme de ise bir adresin kaç adet zombi ye ait olduğu tutulacaktır.

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


    // zombieToOwner adlı key uint olarak saklanan ve adress değerinde public giden bir mapping

    mapping (uint => address) public zombieToOwner;
 
    // ownerZombieCount adlı adress ve değerin uint olduğu bir mapping 

    mapping(address=>uint)   ownerZombieCount;

    function _createZombie(string memory _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
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

