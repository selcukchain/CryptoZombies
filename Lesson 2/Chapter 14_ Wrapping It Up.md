**Bölüm 14: Paketleme**

İşte bu, 2. dersi tamamladınız!

Eylem halinde görmek için sağdaki demoyu kontrol edebilirsiniz. 
Devam et, bu sayfanın sonuna kadar bekleyemeyeceğini biliyorum 😉. Saldırmak için bir kediciğe tıklayın ve aldığınız yeni kedicik zombiyi görün!

**Javascript uygulaması**

Bu kontratımızı Ethereum'a dağıtmaya hazır olduğumuzda, ZombieFeeding'i derleyip dağıtacağız ( çünkü bu kontrat ZombieFactory'den miras kalan son kontratımızdır ve her iki kontrattaki tüm genel yöntemlere erişime sahiptir.)

>Javascript ve web3.js kullanarak konuşlandırılmış sözleşmemizle etkileşime girme örneğine bakalım:
```
var abi = /* abi generated by the compiler */
var ZombieFeedingContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */
var ZombieFeeding = ZombieFeedingContract.at(contractAddress)

// Zombi ID'miz ve saldırmak istediğimiz kitty ID'miz
let zombieId = 1;
let kittyId = 1;

// CryptoKitty'nin görüntüsünü almak için web API'lerini sorgulamamız gerekiyor.
// Bu bilgi blockchainde saklanmaz, sadece onların web sunucusunda saklanır.
// IHer şey bir blockchainde depolanmış olsaydı, sunucunun çökmesi,
// API'lerini değiştirmesi veya şirket varlıklarını yüklememizi engellemesi konusunda endişelenmemize gerek kalmazdı.
// bzombi oyunumuzu sevmiyorlar ;)

let apiUrl = "https://api.cryptokitties.co/kitties/" + kittyId
$.get(apiUrl, function(data) {
  let imgUrl = data.image_url
  //img gelmesi için 
  
})

// kullanıcı kitty'e tıkladığında 
$(".kittyImage").click(function(e) {
  // kontratımızdan `feedOnKitty` yöntemini çağırın:
  ZombieFeeding.feedOnKitty(zombieId, kittyId)
})

// Görüntülememiz için sözleşmemizden bir NewZombie etkinliği çağıralım.
ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  // Bu fonksiyon, ders 1'deki gibi zombiyi gösterecektir:
  generateZombie(result.zombieId, result.name, result.dna)
})
```

**Deneme Zamanıı !!!**


Beslemek istediğiniz kediciği seçin.Zombinin DNA'sı ve kedinin DNA'sı birleşecek ve orduna yeni bir zombi alacaksın!

Yeni zombinizdeki sevimli kedi bacaklarına dikkat edin? İşte DNA'mızın son 99 hanesi bu 😉

İsterseniz baştan başlayıp tekrar deneyebilirsiniz. Memnun kalacağınız bir kedicik zombiniz olduğunda (sadece bir tanesi sizde kalır), devam edin ve 2. dersi tamamlamak için bir sonraki bölüme geçin!

