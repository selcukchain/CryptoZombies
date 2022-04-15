Chapter 12: Putting It Together(Bir araya getirmek)

1. createRandomZombie adlı bir public fonksiyon oluşturun. _name adlı bir parametre alacaktır (veri konumu belleğe ayarlanmış bir string). (Not: Bu fonksiyon, önceki private function olarak bildirdiğiniz gibi public olarak bildirin)

2.Fonksiyon(Function) olarak ilk satırı, _name üzerinde _generateRandomDna function çalıştırmalı ve onu randDna adlı bir uint'te saklamalıdır.

3.İkinci satır _createZombie function çalıştırmalı ve _name ve randDna iletmelidir.

4.Çözüm 4 satır kod olmalıdır (fonksiyonun } (kapanışı) dahil).

    pragma solidity  >=0.5.0 <0.6.0;

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
          uint rand = uint(keccak256(abi.encodePacked(_str)));
           return rand % dnaModulus;
       }
