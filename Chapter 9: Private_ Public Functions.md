**Chapter 9: Private Public Functions (Bölüm 9: Özel / herkese açık Fonksiyonlar)**

Solidity'de işlevler varsayılan olarak public(genel) olarak gelir. Bu, herhangi birinin (veya herhangi bir başka kontratın) kontratınızın işlevini çağırabileceği ve kodunu çalıştırabileceği anlamına gelir.

Açıkçası bu her zaman arzu edilen bir şey değildir ve kontratınızın saldırılara karşı savunmasız hale getirebilir. Bu nedenle, yapılan işlevlerinizin private olarak işaretlemek ve ardından yalnızca herkese göstemek istediğiniz işlevleri public hale getirmek iyi bir uygulamadır.

>Private bir function’ın nasıl bildirileceğine bakalım:

    uint[] numbers;

     function _addToArray(uint _number) private {
     numbers.push(_number);
    }


Bu, yalnızca kontratımızdaki diğer fonksiyonlarına bu işlevi çağırabileceği ve number array kısmını ekleyebileceği anlamına gelir.

>Gördüğünüz gibi, function adından sonra private anahtar sözcüğünü kullanıyoruz. Ve fonksiyon parametrelerinde olduğu gibi, private function adlarını alt çizgi (_) ile başlatmak bir kuraldır.

**Uygulama**


Kontratımızın createZombie işlevi şu anda public olarak hazır halde beklemektedir.bunun anlamı herkesiz cağırabiliceği ve kontratımızda yeni bir zombie oluşturabiliceği anlamına gelir.

>CreateZombie’yi private bir function olacak şekilde adlandırma kuralını unutmadan yazalım.

     pragma solidity >=0.5.0 <0.6.0;

     contract ZombieFactory {

        uint dnaDigits = 16;
        uint dnaModulus = 10 ** dnaDigits;

   	    struct Zombie {
           string name;
 	         uint dna;
        }

   	    Zombie[] public zombies;
           // CreateZombie funtion private hale getirilmiştir.
           
        function _createZombie(string memory _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
        }

      }
