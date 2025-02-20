# setup-server for windows
## Virtual Box Kurulumu
- Öncelikle işletim sistemizi sanal olarak kullanmamız için bir sanal makine aracına ihtiyacımıza var. Virtual Box Manager bu iş için kullanılan açık kaynak bir programdır. Bu program kurmak için ;
- [Virtual Box Manager](https://www.virtualbox.org/wiki/Downloads) sitesinden Windows için olan son olan sürümünü indiriyoruz. Bu indirdiğimiz *.exe* dosyasını açıp hızlıcı kuruyoruz.

## Virtual Box Üzerinden kurulum
- Öncelikle kurulum için gerekli olan ISO dosyasını [Ubuntu Server LTS 24.0.1](https://ubuntu.com/download/server) linki üzerinden **Download LTS** butonuna tıklayarak indiriyoruz.
- Virtual Box'ı açalım ve New seçeneğine tıklayalım. Tıkladığımız sekmede öncelikle name kısmına ne için kullanacaksak isim veriyoruz. Biz bu servera WebServer olarak kuracağımız için WebServer yazalım. ISO Image kısmında indirdiğimiz ISO dosyasını seçiyoruz ve *NEXT* tuşuna basarak ilerliyoruz.
- Şimdi ki  gelen kısımda Username ve Password kısmını düzeltelim. Username kısmı için vboxuser kalabilir. Biz bu kurulum için kolaylık olması için vboxuser olarak bırakacağız. Password kısmındaki şifremizi değiştiriyoruz. Sağ kısımdaki Hostname kısmında boşluk olmamasına dikkat ediyoruz. Otomatik olarak gelen WebServer kalabilir.Sonrasında *NEXT* tuşuna basıyoruz.
- Şimdiki kısım bilgisayarımızın bu kuracağımız işletim sistemi için ne kadar CPU çekirdeği ve RAM kullanacağını belirleyeceği kısımdır. Bu kısımda Ubuntu Server için 2048MB Base Memory ve 2 İşlemci Çekirdeği vermek genellikle yeterlidir. Eğer sisteminiz güçlüyse daha fazla verebilirsiniz. Bu adımı da ayarladıktan sonra tekrar *NEXT* tuşuna basıyoruz.
- Bu gelen kısımda disk olarak ne kadar yer kaplayacağını söyleyeceğiz. Bu seçim tamamen size bağlıdır. Biz 25 GB seçerek ilerleyeceğiz. Çünkü içine yeteri kadar paket yüklemeyeceğiz. Bu bizim için yeterli. **Pre-Allocate Full Size** seçeneği sizin diskinizde o alanı direkt tahsis eder ve bu kullanmadığınız alanı da yiyeceği için pek tercih edilmez. Biz işaretlemeden geçeceğiz. Bu işlemi yaptıktan sonra tekrar *NEXT* tuşuna basarak ilerliyoruz.
- Şimdiki ekranda seçtiğimiz şeyleri tekrar kontrol etmemiz için gösteriyor. Bu sayfada kontrol ettikten sonra *FINISH* tuşuna basıyoruz ve sonrasında açılacak olan WebServer adındaki Ubuntu Server'imi kurulmaya başlıyor. Kurulması için 5-10 dakika kadar bekliyoruz.

## Ubuntu üzerindeki Konfigürasyonlar
- Öncelikle kurulumdan sonra bizi bir terminal ekranı karşılıyor. Bu karşılama ekranında Virtual Box'ın kurulumdan girdiğimiz Username ve Password'u kullacanağız. Bu kullandığımız Username giriyoruz. Sonrasında bizden şifre istiyor ve yine o aşamada girdiğimiz şifreyi girerek kullanıcımıza giriş yapıyoruz. Eğer kullanıcı girişi sağlandıysa artık konfigürasyonlara geçebiliriz.
- Öncelikle
```bash
sudo apt update
// Güncelleme yapılacak depoları günceller.
sudo apt upgrade
// Bilgisayar içindeki mevcut paketleri günceller ve yeni sürümlerini yükler.
```
komutlarını girdikten sonra bize tekrar indirip indirmek istemediğimizi soruyor. Bu soruya y tuşuna bastıktan sonra *ENTER* yaparak Ubuntu Server içindeki paketleri güncelliyoruz ve yeni sürümlerini yüklüyoruz. Sistemi güncel tutmak çoğu zaman iyidir.
- Biz Türkçe klavye kullandığımız için öncelikle yazacığımız sistemde kolaylık sağlamak klavye sistemini Türkçe yapmak en doğrusu olacaktır. Eğer siz yapmak istemezseniz bu komutu kullanmanız gerekmiyor. Klavye düzenini Türkçe yapmak için ;
```bash
sudo loadkeys tr
```
komutunu kullanarak sistemdeki klavye düzenimizi Türkçe yapıyoruz. Eğer kalıcı olarak yapmak istersek ; 
```bash
sudo nano /etc/default/keyboard
//nano komutu text dosyaları üzerinde işlem yapmamızı sağlar.
```
diyerek klavye konfigürasyon ayarlarına geliyoruz ve **XKBLAYOUT = "us"** kısmını **XKBLAYOUT = "tr"** olarak değiştiriyoruz. Ok tuşlarıyla imleci hareket ettirebilirsiniz. Bunu yaptıktan sonra *CTRL + X* tuş kombinasyonu yapıyoruz. Bu tuş kombinasyonunu sürekli kullanacağız. Bize bufferdan(Geçici bellek) diske yazılması hakkında bir soru soruyor. Buna y diyerek kaydedeceğimizi söylüyoruz sonrasında dosya ismini değiştirebileciğimiz bir kısım geliyor. Bu kısımda değiştirmeyeceğimiz için *ENTER* tuşuna basarak geçiyoruz. Artık klavyemiz türkçe düzenine döndü.
- Şimdi ise bir user oluşturmamız gerekiyor. User oluşturmak için ;
```bash
sudo adduser new_user
// Yeni bir user oluşturur.
```
komutunu kullanarak user oluşturma yerine geliyoruz. Bu komutu girdikten sonra bize şifresinin ne olması gerektiğini soruyor. Şifremizi giriyoruz. Aynı şifreyi tekrar istiyor. Bunu da girdikten sonra bize isim, telefon gibi sorular soruyor. Bunları şuan için kullanmamız gerekmediği için *ENTER* diyerek geçiyoruz. Sonrasında y diyerek bilgilerin doğruluğunu kabul ediyoruz ve kullanıcımız oluştu. Şimdi ise kullanıcıya sudo yetkisi vereceğiz. Sudo yetkisi bütün sisteme erişebileceğimiz anlamına geliyor. Bu süreçten sonra o kullanıcıdan ilerleyeceğimiz için sudo yetkisi vermek bizim için iyi olacaktır. 
```bash
sudo usermod -aG sudo new_user
// new_user kullanıcısına yetki vermek için kullanıyoruz.
```
bu komutu da uyguladıktan sonra artık new_user kullanıcısına geçebiliriz. Yeni kullanıcıya geçmek için 
**su komutu kullanıcılar arası geçiş yapmak için kullanılır.**
```bash
su new_user
```
bu komuttan sonra gelen şifre yerine şifremizi yazdıktan sonra artık bu kullanıcının yerine geçtik.
- Şimdi Windows bilgisayarımızdan bağlanmak için *ssh* paketini indireceğiz. *ssh* paketi uzaktan bir bilgisayara ya da ağ içindeki bilgisayara bağlanmak için kullanılır. Biz windows bilgisayarımızdan başka bir bilgisayara bağlanmak için bu paketi kullanacağız. İndirmek için
**Bu komut openssh-server paketini indirir ve kurar.**
```bash
sudo apt install openssh-server -y
```
komutunu kullanarak bilgisayarımıza indiriyoruz. Sonrasında aktif etmek için 
- **Bu komut sadece o anlık başlatır.**
```bash
sudo systemctl start ssh
```
- **Bu komut ise her işletim sistemi açılışında otomatik çalıştırmak için kullanılır.**
```bash
sudo systecmtl enable ssh
```
- **Bu komut ssh durumunu kontrol eder.**
```bash
sudo systemctl status ssh 
```
komutuyla kontrol ediyoruz. Eğer Active kısmında **active** yanıyor ve yeşil ise başardık demektir. Şimdi ise makinemizi kapatmamız gerekiyor.
**Makinemizi kapatmak için** 
```bash
sudo shutdown -h now
```
komutunu kullanıyoruz.
## Virtual Box Konfigürasyonları
- Kurulum yaparken Virtual Box Manager Network ayarlarını varsayılan ağ modunu NAT olarak ayarlar. NAT modu kurduğumuz sanal makinenin dış dünyaya çıkarken ki IP adresi host olan IP adresi ile aynı olur. Bu bizim HOST makinemizden sanal makinemize bağlanmamız için sanal makinenin de bir IP alması gerekir. Bunu sağlamak için bu sunucu serverin ağ ayarlarını Bridged Adaptor yapmamız gerekiyor. Bridged adapter modu bu sanal makineyi internete bağlarken bu makineyi sanal değil fiziksel olarak algılar ve o sunucuya bir IP verir. Bu IP'yi kullanarak ssh bağlantısı gerçekleştireceğiz. Aşağıda nasıl ağ ayarlarını değiştirmemiz gerektiği açıklanmaktadır. 
- Sanal makinemize tıklayıp **Settings** kısmına geliyoruz ve Network kısmını buluyoruz. Gördüğünüz üzere varsayılan olarak **Attached to**(Bağlanma durumu) NAT yapılmış durumda bunu seçerek Bridged Adapter diyoruz. Zaten Wi-Fi kartını veya ethernet kartını da aşağıda otomatik tanımlıyor. Bu ayarı yaptıktan sonra **OK** tuşuna basıp kaydediyoruz ve sanal makineyi başlatıyoruz. Başlattıktan sonra yeni oluşturduğumuz *new_user* kullanıcısına giriş yapıyoruz.

## SSH Bağlantısı
- Öncelikle güvenlik önlemi için Firewall yazılımını açacağız. Varsayılan olarak kapalı gelir yine de kontrol edelim.
- **Firewall durumunu kontrol eder.**
```bash
sudo ufw status
```
Eğer status olarak inactive yazıyorsa açmak için 
- **Firewall'ı açar.**
```bash
sudo ufw enable 
```
bunun ardından gelen systemi tekrar başlatın uyarısından sonra 
- **Sistemi yeniden başlatır.**
```bash
sudo reboot
```
komutunu kullanıyoruz. Bilgisayarımız açıldıktan sonra giriş yapıyoruz.
- Şimdi ise genellikle SSH için kullanılan portumuz yani 22 portunu açacağız.
- **Firewallda port izni vermek için** 
```bash
sudo ufw allow 22
```
komutunu girerek artık 22 portunu açmış bulunmaktayız kontrol etmek için 
```bash
sudo ufw status
```
yapıyoruz. Eğer 22 portu için action yerinde allow yazıyorsa başardık demektir.
## SSH Key Oluşturma
- Güvenlik için sadece bizim bilgisayar için bir kullılan bir ssh keyi oluşturacağız. Bir sonraki girişimiz için bizi tanıyacak ve ssh ile bağlantımız hem güvenli hem de kolay olacak.
- Windows bilgisayarımızda **Başlat** tuşuna sağ tıklayarak terminal(admin) açıyoruz. ve terminal kısmına ssh-keygen yazıyoruz. Normal ismimiz kalsın o yüzden **ENTER** tuşuna basıyoruz. Extra bir şifreleme istiyorsanız passphrase kısmında bağlanırken kullanmanız için şifrenizi girebilirsiniz. Artık ssh-keyimiz oluşturuldu. Ssh keyimizi öğrenmek için ssh klasörüne gidelim.
- **.ssh klasörüne girer.**
```bash
cd .ssh
```
komutunu kullanıyoruz.
- **O klasör içindeki dosyaları listeler.**(PowerShell)
```bash
ls
```
- **O klasör içindeki dosyaları listeler.**(CMD)(Command Prompt)
```bash
dir
```
komutunu kullanarak hangi dosyalar olduğunu görelim. Orada bulunan .pub uzantılı dosya içinde bizim public ssh-keyimiz mevcut. Öğrenmek için
- **Dosyayı okur.**(PowerShell)
```bash
cat {.pub uzantılı dosya ismi}
```
```bash
type {.pub uzantılı dosya ismi}
```
diyerek o dosyanın içeriğini okuyoruz. Çıktı olarak gelen bu kodu kopyalayalım. Windows için işlemlerimiz bu kadar.

## SSH Bağlantısı
- Şimdi sanal makinemizin IP'sini öğrenerek bağlantı zamanı.
- **Sanal makinenin IP'sini gösterir.**
```bash
hostname -I
```
komutunu kullanarak sanal makinein IP'sini öğrendik. Şimdi Windows cihazımıza geliyoruz.
- Windows cihazımızdan bağlanmak için PowerShell'i kullanacağız. Eğer PowerShell yoksa veya kullanılamıyorusa Putty programını kullanarak SSH bağlantısını gerçekleştirebilir. Bu dökümantasyonda sadece PowerShell ile giriş açıklanacaktır. İnternetten kısa bir araştırmayla nasıl gireceğinizi öğrenebilirsiniz.
- Powershell terminaline geldik. Şimdi bağlanmak için
- **SSH bağlantısı ile bağlar.**
```bash
ssh new_user@{ip adresi} -p 22
```
komutunu kullanarak kullanıcı adınız farklı ise kullanıcı adınızı yazın. Şifrenizi girerek giriş yapabilirsiniz. Artık ssh ile giriş yapmış bulunmaktayız.
## SSH Key İle Kolay Giriş
- Şimdi ssh keyimizi tanımlayarak şifre girmeden girebileceğiz ve başka bir bilgisayardan bağlantı sağlandığında onu direkt reddedecek.
- Windows bilgisayarımızdan girişi sağlamıştık ve *.pub* dosyasındaki ssh keyimizi kopyalamıştık. Bu dosyayı yazacağımız dosyayı oluşturacağız.
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.authorized_keys
```
komutlarını çalıştarak dosyamızı oluşturuyoruz ve izinlerini ayarlıyoruz. *mkdir* komutu klasör oluşturmak için kullanılıyor. *chmod* komutu dosya veya klasörler için kullanıcı ve grup için izinleri ayarlayan bir komuttur. 700 yaparak bizim kullandığımız kullanıcı için *.ssh* klasöründe okuma, yazma ve çalıştırma izni vermiştir. *touch* komutu yeni bir dosya oluşturmak için kullanılan bir komuttur. Touch komutu ile keylerimizi tutacağımız bir *.authorized_keys* dosyası oluşturduk. Sonrasında bu dosyanın iznini diğer kullanıcı ve gruplardan alarak sadece kendimiz için okuma,yazma izni vermek için *chmod* komutu ile 600 parametresini kullandık.
- Şimdi ise o dosya içine bu ssh keyimizi yapıştıracağız. Bunun için
```bash
nano ~/.ssh/authorized_keys
```
komutunu kullanıyoruz ve bu *.pub* dosyası içindeki ssh keyimizi buraya kopyalayıp yapıştırıyoruz. Sonrasında çıkmak için **CTRL+X** tuşlarına basıyoruz. Sonrasında y tuşuna basıyoruz ve **ENTER** yaparak çıkış yapıyoruz. 
- Şuan bizim bilgisayarımızı tanıyor fakat biz şifreyi kaldırmak istiyoruz ve root kullanıcısı ile ssh girişi istemiyoruz. Bunun için config dosyasındaki izinleri kaldıracağız.
```bash
sudo nano /etc/ssh/sshd_config
```
komutunu kullanarak config dosyasını açıyoruz. **CTRL+W** tuşlarına basarak aşağıdaki kelimeeleri aratarak bunları karşısındaki parametre ile değiştireceğiz ve eğer başında **#** işareti varsa kaldıracağız.
```bash
* PermitRootLogin no
* PubKeyAuthentiation yes
* AuthorizedKeysFile .ssh/authorized_keys
* PasswordAuthentication no
* UsePAM no
```
yerlerini bu şekilde değiştiriyoruz ve kaydediyoruz.(Daha önce söylendiği için bundan sonra söylemeyeceğiz.)
- **SSH servisini sıfırlar.**
```bash
sudo systemctl restart ssh
```
komutunu kullanalım.
- Şimdi Windows bilgisayarımızda deneyelim. Zaten bilgisayarımızda girmiştik. Bilgisayarımız ile girmiştik zaten ssh bağlantısını koparmak için
```bash
exit
```
komutunu kullanıyoruz ve tekrar ssh ile bağlantı yapmaya çalışınca(daha önce gösterilen şekilde) artık bağlantı direkt sağlanıyor. Ayrıca root kullanıcı ile giriş de iptal olmuş oldu. Eğer root ile bağlanmadığını görmek isterseniz sadece new_user tarafını root olarak değiştirebilirsiniz.

## APACHE Web Server Kurulumu
- Şimdi ise Apache kuracağız. Apache yaygın olarak kullanılan özgür, açık kaynak bir yazılımdır. Bu yazılım HTTP portu üzerinde çalışan bir web sayfalarını istemcilere dağıtmak için kullanılır.
- **Apache'yi kurar.**
```bash
sudo apt update
sudo apt install apache2 -y
```
komutlarını kullanarak Apache Web Serverini indirelim. İndirdikten sonra sistem her açıldığında başlatmak için 
```bash
sudo systemctl enable apache2
```
ve kontrol etmek için 
```bash
sudo systemctl status apache2
```
yapabiliriz.

## Apache Konfigürasyonları
- Aynı Ip adresini yönlendireceğim için Virtual Host olarak kullanmalıyım Apache'yi Bu Virtual hostları ayarlamak için
```bash
cd /etc/apache2/sites-available
```
komutunu kullanarak Apache'nin bazı konfig dosyalarının olduğu klasöre girdik. Burada her site için komutunu kullanıyoruz. Gördüğünüz üzere buğday.org.conf başka bir dosya oluşturduk. Türkçe karakter veya Çince gibi farklı karakterler kullanımı için PunyCode çevirimi yapmamız gerekiyor. Uyum açısından dolayı böyle bir şey yapmamız gerekmekteydi. PunyCode bu tarz karakterler için farklı bir kod oluşturan bir özelliktir. Biz bu özelliği kullandığımız için buğday.org.conf dosyasının punycode'u olaran *xn--buday-l1a.org.conf* ismini kullandık ve bu şekilde ilerleyeceğiz.
```bash
sudo nano 2025ozgur.com.conf
sudo nano bugday.org.conf
sudo nano xn--buday-l1a.org.conf
```
dosyalarını hem oluşturduk hem de içlerine
**bugday.org.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@bugday.org
    ServerName 192.168.1.106 # Buraya hostname -I ile oluşan IP'yi yazalım.
    DocumentRoot /var/www/bugday.org/

    <Directory /var/www/bugday.org/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
**buğday.org.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@bugday.org
    ServerName 192.168.1.106 # Buraya hostname -I ile oluşan IP'yi yazalım.
    DocumentRoot /var/www/bugday.org/

    <Directory /var/www/bugday.org/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
**2025ozgur.com.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@2025ozgur.com
    ServerName 2025ozgur.com
    ServerAlias www.2025ozgur.com
    DocumentRoot /var/www/2025ozgur.com/

    <Directory /var/www/2025ozgur.com/yonetim>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
yazalım. Config dosyalarımı oluşturduk. Şimdi ise bu config dosyalarının root klasörlerini oluşturacağız. Bunun için 
```bash
cd /var/www 
```
diyerek apachenin html dosyalarının olduğu yere geliyoruz. Burada bulunan html klasörünü silmek için 
```bash
sudo rm -rf html 
```
komutunu kullanıyoruz ve kaldırıyoruz. Kaldırdıktan sonra yeni root dosyalarını oluşturacağız.
- **Klasör oluşturur**
```bash
sudo mkdir bugday.org
sudo mkdir 2025ozgur.com
```
klasörlerini oluşturuyoruz. *bugday.org* ve *buğday.org* aynı Wordpress'e bağlanacağı için extra bir klasör oluşturmadık.
- Apachenin yapılandırması bitti. Şimdi Apache yapılandırmasını sağlamk için
```bash
sudo a2ensite bugday.org.conf
sudo a2ensite 2025ozgur.com.conf
sudo a2ensite buğday.org.conf
```
komutlarını kullanıyoruz. Apache servisini tekrar başlatalım
```bash
sudo systemctl restart apache2
```
- Artık Apache'yi hem konfigüre ettik ve yapılandırdık. Şimdi Wordpress kurulumuna geçelim.
