<h1 align="center"> Dusk Network | Node Kurulumu </h1>

<img src="https://media.giphy.com/media/BemKqR9RDK4V2/giphy.gif" width="1000" height="500">

<h1 align="center"> Selamlar arkadaşlar, Dusk ağında dün form doldurmuştuk ve tokenler biraz önce geldi, şimdi nodumuzu kuralım </h1>

## Formu doldurmadıysan: https://github.com/ruesandora/dusk-network

## En önemli konu sistem gereksinimleri:

* Kolay kısım:
```
1 CPU
1 RAM
```
* Ubuntu version öğrenme:
```
lsb_release -r
```
* Sorun olan kısım `22.04 Ubuntu version` gerekiyor.
* Bu herkeste olmayabilir, bunun için yeni sunucu gerekiyor. (genellikle 18 veya 20 kullanıyoruz)
* İsteyen denesin, ubuntu versionu 22 olmayan 2 tane farklı sunucuda denedim, node çalışmıyor.
* Ben Hetznerin 3 euroluk sunucusunu kullandım, küçük nodelarda hep hetzner kullanıyorum, sanırım contaboyu tamamen bıraktım.
* Eğer şu an satın almaya müsait değilseniz ücretsiz nasıl alacağınızı bu floodda anlattım: [Link](https://forum.rues.info/index.php?threads/sunucular-hakkinda-ve-tavsiye-olmayan-fikirlerim.2481/)

![image](https://user-images.githubusercontent.com/101149671/202677911-bac3fa2f-dd34-4589-9d3c-8f6c6f050266.png)

## Sunucuyu çözdüyseniz, sıfırdan okuşturacağınız için cüzdanı taşımak gerekli, burdan başlayalım:

* Hali hazırda ubuntu 22 versionlu sunucuna kurduysan [Buradan başla](https://github.com/ruesandora/dusk-network/blob/main/node-setup.md#%C5%9Fimdi-buras%C4%B1-kar%C4%B1%C5%9F%C4%B1k-arkada%C5%9Flar-dikkatli-okumayana-k%C3%BCserim)
```
sudo apt update
```
## WinsSCP ile yeni sunucuna gir ve dün indirdiğimiz wallet (ZIP) dosyasını yeni sunucunun içine at

* Bu komut ile dosyayı açın:
```
tar -xvf ruskwallet0.13.0-linux-x64.tar.gz
```
* Dizine girin:
```
cd rusk-wallet0.13.0-linux-x64/
```
* Wallet içine girin:
```
./rusk-wallet
```
* Rusk wallet komutu hata verirse (tek tek girin):
```
sudo -i
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb
cd rusk-wallet0.13.0-linux-x64/
./rusk-wallet
```
* Dün kurduğumuz cüzdanın 12 kelimesini import (recovery) edin.
* 12 kelimeyi recover ettikten sonra cüzdanın içine girip `export provisioner key-pair` yapınız

## Şimdi burası karışık arkadaşlar dikkatli okumayana küserim:

* Aslında karışık değil, max 30 saniye sürüyor, daha önce yapmadığınızı düşünüyorum.
* Dün ki gibi winscp ile: `/root/.dusk/rusk-wallet` içine giriyoruz
* dün CPK ile işimiz vardı, bugün .KEY ile.
* .key dosyasını masa üstüne KOPYALIYORUZ
* Masaüstünde ki dosyanın adını: `Consensus.keys` yapıyoruz. 
* DİKKAT: .key değil, .keys.. iyi okuyun arkadaşlar

![image](https://user-images.githubusercontent.com/101149671/202683078-1e8f33e8-a280-49a7-8dd7-d85d608c39d1.png)

## Erişim izni:

* Ayrı ayrı girin

```
ufw allow 9000:9005/udp

curl --proto '=https' --tlsv1.2 -sSfL https://raw.githubusercontent.com/dusk-network/itn-installer/main/itn-installer.sh | sudo sh
```
## Şimdi altta ki komutu çalıştırın:

* Şifre yazan kısma cüzdanı girerken kullandığımız şifreyi yazıp çalıştırın
```
echo 'DUSK_CONSENSUS_KEYS_PASS=ŞİFRE' > /opt/dusk/services/dusk.conf
```

## Şimdi masaüstüne attığımız dosyayı üzerine yazmamız gerekiyor:

* Winscp ile `/opt/dusk/conf` içine girelim
* Kısa yol: Kırmızılı yere tıklayıp, yeşil yere yapıştırarak aratmak:
* Şimdi buraya girdiğimizde burada da bir `consensus.keys` olduğuunu göreceğiz
* Masa üstünde ki `consensus.keys`'i alıp, bunun üstüne sürükleyeceğiz
* Yani: masaüstünde ki key, `/opt/dusk/conf` dosyasında ki `consensus.key`'in üzerine yazacak.

![image](https://user-images.githubusercontent.com/101149671/202684754-dbaec15a-532c-4b90-a389-4d1ab954aa05.png)

## Node kurulumuna hazırlanıyoruz:

* Bu komutları girmeden önce `nano /opt/dusk/services/rusk.conf.user` içi boş olur
* Bu komutları girdikten sonra `nano /opt/dusk/services/rusk.conf.user` içi dolu olur
* Bu komutu girerken `İPADRESİ` kısmını düzenleyin
```
echo 'KADCAST_PUBLIC_ADDRESS=İPADRESİ:9000' > /opt/dusk/services/rusk.conf.user;
echo 'KADCAST_LISTEN_ADDRESS=İPADRESİ:9000' >> /opt/dusk/services/rusk.conf.user;
```

## Şimdi nodu çalıştırın:

* Daha sonra görselde ki ekranı tekrar görmek isterseniz: `service rusk status`
* Çıkış yapmak için 2 kez ctrl + c yapın, node durmaz burada, isterseniz tekrar kontrol edersiniz.

```
service rusk start
service dusk start
```
* Görsel:

![image](https://user-images.githubusercontent.com/101149671/202687571-add761ac-d0fe-4d76-a9e5-9f07a0e9fe0a.png)

## Tokenleri delege edebilmemiz için nodumuzun güncel blokla eşleşmesi lazım, bu komutla takip edebilirsiniz

```
tail -F /var/log/dusk.log | grep "accept_block"
```


##  Node güncel bloğu yakaladıktan sonra son kısım, gelen tokenleri delege etme:

* İçine giriyoruz:
```
./rusk-wallet
```
* Girdikten sonra `Stake Dusk` diyoruz
* Miktar: `29995` (30k geldi, biraz fee için vs. bırakın)
* Gas limit: `2900000000`
* Gas price: Burada direkt tab tuşuna basın, var sayılan dışına çıkamazsınız
* Sonra Y diyip enterliyoruz
* 30/30 işlem gerçekleşecek ve bit TX HASH verecek bize
* Eğer burada SUCCES yazıyorsa aratınca, işlem tamam: [Explorer Linki](https://explorer.dusk.network/transactions/)

## Node kontrol:
```
tail -100 /var/log/dusk.log | grep 'Accepted'
```

* Altta ki çıktıya benzer bir çıktı alırsanız artık keyfinize bakabilirsiniz:

![image](https://user-images.githubusercontent.com/101149671/202689226-c7d167d7-f3a1-4c03-be81-1979ec15dbe4.png)

Artık yemeğimi yiyebilirim..






