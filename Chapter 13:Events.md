**Chapter 13: Events (Etkinlikler)**

Kontratımızın bitmesine az kaldı.Şimdi bir etkinlik ekleyelim.

>Events,kontratımızın blok zincirinde bir şey olduğunu uygulamamızın front-end ısmına iletmesinin bir yoludur;bu,bir dinleme ve gerçekleştiğinde harekete geçeceği anlamına gelebilir.

>Örneğin
       
        // Event'in ilan edilmesi
    event IntegersAdded(uint x, uint y, uint result);

    function add(uint _x, uint _y) public returns (uint) {
     uint result = _x + _y;
    // uygulamaya işlevin çağrıldığını bildirmek için bir olay başlat:
     emit IntegersAdded(_x, _y, result);
     return result;
    }

>Uygulamanızın front-end event kısmı dinleyebilir. Bir javascript uygulaması ise şuna benzer:

    YourContract.IntegersAdded(function(error, result) {
     // sonucu olan bir şey yapmak istiyoruz.
     }
     
   **Uygulama**

Sıra geldi Zombilerimize,Uygulamanın bunu gösterebilmesi için her yeni zombi oluşturulduğunda front-end kısmında bildirecek bir olay istiyoruz.

>NewZombie adlı bir event bildirin. zombieId (bir uint), name (bir string) ve dna'yı (bir uint) olmalıdır.

>Zombies array kısmına new zombie ekledikten sonra NewZombie event’ini başlatmak için _createZombie function kısmını değiştirin.

**Zombinin kimliğine ihtiyacın olacak. array.push() dizinin yeni uzunluğunun bir uint'ini döndürür - ve dizideki ilk öğenin indeksi 0 olduğundan, array.push() - 1 az önce eklediğimiz zombinin indeksi olacaktır. zombies.push() - 1 sonucunu id adlı bir uint'te saklayın, böylece bunu bir sonraki satırdaki NewZombie olayında kullanabilirsiniz.**

    pragma solidity >=0.5.0 <0.6.0;

    contract ZombieFactory {

    // NewZombie event ve zombieId,name,dna ile
    event NewZombie(uint zombieId , string name, uint dna);
    
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string memory _name, uint _dna) private {
       // zombies.push(Zombie(_name, _dna)) -1 ile az önceki zombienin indeksini koruduk.
      uint id= zombies.push(Zombie(_name, _dna))-1 ;
        
        //NewZombie(id,_name,_dna) emit ile event tetiklendi
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


