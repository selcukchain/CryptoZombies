**Chapter 11: Keccak256 and Typecasting(Keccak256 ve Typecasting)**

>_generateRandomDna fonksiyonu (semi/yarım) rastgele bir uint döndürmesini istiyoruz. Bunu nasıl başarabiliriz?

Ethereum, SHA3'ün bir sürümü olan yerleşik keccak256 hash fonksiyonuna sahiptir. Bir hash fonksiyonu temel olarak bir girişi rastgele 256 bitlik onaltılık bir sayıya eşler. Girdideki küçük bir değişiklik, hash'te büyük bir değişikliğe neden olur.

**Ethereum'da birçok amaç için kullanışlıdır, ancak şimdilik onu sadece pseudo-random(sözde rastgele) sayı üretimi için kullanacağız.**

>Ayrıca, keccak256 bayt türünde single parameter bekler. Bu, keccak256'yı çağırmadan önce herhangi bir parametreyi "pack" yapmamız gerektiği anlamına gelir:

>Örneğin:

    //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
          keccak256(abi.encodePacked("aaaab"));

    //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
          keccak256(abi.encodePacked("aaaac"));

Gördüğünüz gibi, girdide yalnızca 1 karakterlik bir değişikliğe rağmen döndürülen value tamamen farklıdır.

**Not: Blok zincirinde Güvenli rastgele sayı üretimi çok zor bir problemdir. Buradaki yöntemimiz güvensizdir, ancak Zombi DNA'mız için güvenlik birinci öncelik olmadığından, şu an ki amacımıza yeterlidir .**

**Typecasting**

Bazen veri türleri(data type’lar) arasında dönüştürme yapmanız gerekir.

>Örneğin:

    uint8 a = 5;
    uint b = 6;
    // a * b uint8 değil, bir uint döndürdüğü için bir hata verir:

    uint8 c = a * b;
    // çalışması için b'yi uint8 olarak yazmalıyız:
    uint8 c = a * uint8(b);

>Yukarıda a * b, bir uint döndürür, ancak onu bir uint8 olarak saklamaya çalışıyorduk, bu da olası sorunlara neden olabilir. Bir uint8 olarak yayınlayarak çalışır ve derleyici hata vermez.

**Uygulama**

_generateRandomDna body function’ı dolduralım! İşte yapılması gerekenler:

>Kodun ilk satırı, pseudo-random bir hexadecimal oluşturmak için abi.encodePacked(_str)'nin keccak256 hash’ini almalı, onu bir uint olarak yazmalı ve son olarak da sonucu rand adlı bir uint'te saklamalıdır.

>DNA'mızın yalnızca 16 basamak uzunluğunda olmasını istiyorduk (dnaModulus'umuzu hatırlıyor musunuz?). Bu nedenle, ikinci kod satırı yukarıdaki değer modülünü (%) dnaModulus döndürmelidir.

     pragma solidity ^0.4.25;

    contract ZombieFactory {

       uint dnaDigits = 16;
       uint dnaModulus = 10 ** dnaDigits;

      struct Zombie {
           string name;
           uint dna;
           
         }

       Zombie[] public zombies;

       function _createZombie(string memory _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
       }

        function _generateRandomDna(string memory _str) private view returns (uint) {
       // sözde random oluşturduğumuz string abi.encodePacked’i keccak256 hash aldırma ve bunu rand altında saklama kod betiği
        uint rand =uint (keccak256(abi.encodePacked(_str)));
       //oluşturulan rand’ı %dnaModulus sayesinde 16 basamak uzunluğuna çevirme
       return rand %dnaModulus;
       }

    }
