**Chapter 4: Math Operations(Matematik Operatörleri)**

Solidity'de Matematik oldukça basittir.

Aşağıdaki işlemler çoğu programlama dilindeki ile aynıdır:

•Toplama: x + y

•Çıkarma: x - y

•Çarpma: x * y

•Bölüm: x / y

•Modül / kalan: x % y (örneğin, %13 5, 3'tür, çünkü 5'i 13'e bölerseniz, 3 kalandır)

Solidity ayrıca üstel bir operatörü de destekler (yani, "x üzeri y", x^y):

•	uint x = 5 ** 2; // equal to 5^2 = 25

**Uygulama**

Sıra zombimizde
Zombimizin DNA ‘sını sadece 16 karakter olduğundan emin olmak için 10^16’ya eşit olacak DnaModulus adında  bir unit yapalım ve dnaDigits gücünü 16 ya eşitleyin.
 
 **16 basamağını kısaltmak için % modülü operatörü kullanabiliriz.**

    pragma solidity >=0.5.0 <0.6.0;

    contract ZombieFactory {

    uint dnaDigits = 16;
    
    //dnaModulus’ün 16 basamak olduğuna emin olmak için dnaDigits ile eşitleyen bir uint yazıldı
   
    uint dnaModulus = 10 ** dnaDigits;
    
    
     }

