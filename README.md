# setup-server for windows
## Virtual Box Kurulumu
- Öncelikle işletim sistemizi sanal olarak kullanmamız için bir sanal makine aracına ihtiyacımıza var. Virtual Box Manager bu iş için kullanılan açık kaynak bir programdır. **Bu programı kurmak için**
- [Virtual Box Manager](https://www.virtualbox.org/wiki/Downloads) sitesinden Windows için olan son olan sürümünü indiriyoruz. Bu indirdiğimiz *.exe* dosyasını açıp hızlıca *NEXT* diyerek kuruyoruz.

## Virtual Box Üzerinden kurulum
- Öncelikle kurulum için gerekli olan ISO dosyasını [Ubuntu Server LTS 24.0.1](https://ubuntu.com/download/server) linki üzerinden **Download LTS** butonuna tıklayarak indiriyoruz.
- Virtual Box'ı açalım ve New seçeneğine tıklayalım. Tıkladığımız sekmede öncelikle Name kısmına ne için kullanacaksak bir isim veriyoruz. Biz bu sanal makineyi WebServer olarak kuracağımız için WebServer yazalım. ISO Image kısmında indirdiğimiz ISO dosyasını seçiyoruz ve *NEXT* tuşuna basarak ilerliyoruz.
- Şimdi ki gelen kısımda Username ve Password kısmını düzeltelim. Username kısmı için vboxuser kalabilir. Biz bu kurulum için kolaylık olması için vboxuser olarak bırakacağız. Password kısmındaki şifremizi değiştiriyoruz. Sağ kısımdaki Hostname kısmındaki kelimenin arasında boşluk olmamasına dikkat ediyoruz. Otomatik olarak gelen WebServer kalabilir. Sonrasında *NEXT* tuşuna basıp ilerliyoruz.
- Şimdiki kısım bilgisayarımızın bu kuracağımız işletim sistemi için ne kadar CPU çekirdeği ve RAM kullanacağını belirleyeceği kısımdır. Bu kısımda Ubuntu Server için 2048MB Base Memory ve 2 İşlemci Çekirdeği vermek genellikle yeterlidir. Eğer sisteminiz güçlüyse daha fazla verebilirsiniz. Bu adımı da ayarladıktan sonra tekrar *NEXT* tuşuna basıp bir sonraki kısma geçiyouz.
- Bu gelen kısımda disk olarak ne kadar yer kaplayacağını söyleyeceğiz. Bu seçim tamamen size bağlıdır. Biz 25 GB seçerek ilerleyeceğiz. Çünkü içine yeteri kadar paket yüklemeyeceğiz. Bu bizim için yeterli. **Pre-Allocate Full Size** seçeneği sizin diskinizde o alanı direkt tahsis eder ve bu kullanmadığınız alanı da yiyeceği için pek tercih edilmez. Biz işaretlemeden geçeceğiz. Bu işlemi yaptıktan sonra tekrar *NEXT* tuşuna basarak ilerliyoruz.
- Şimdiki ekranda seçtiğimiz şeyleri tekrar kontrol etmemiz için gösteriyor. Bu sayfada kontrol ettikten sonra *FINISH* tuşuna basıyoruz ve sonrasında açılacak olan WebServer adındaki Ubuntu Server'imi kurulmaya başlıyor. Kurulması için 5-10 dakika kadar bekliyoruz.

## Ubuntu üzerindeki Konfigürasyonlar
- Öncelikle kurulumdan sonra bizi bir terminal ekranı karşılıyor. Bu karşılama ekranında Virtual Box'ın kurulumdan girdiğimiz Username ve Password'u kullanacağız. Bu kullandığımız Username'i giriyoruz. Sonrasında bizden şifre istiyor ve yine o aşamada girdiğimiz şifreyi girerek kullanıcımıza giriş yapıyoruz. Eğer kullanıcı girişi sağlandıysa artık konfigürasyonlara geçebiliriz.
- Öncelikle
- **Güncelleme yapılacak depoları günceller.**
```bash
sudo apt update
```
- **Bilgisayar içindeki mevcut paketleri günceller ve yeni sürümlerini yükler.**
```bash
sudo apt upgrade
```
komutlarını girdikten sonra bize tekrar indirip indirmek istemediğimizi soruyor. Bu soruya y tuşuna bastıktan sonra *ENTER* yaparak Ubuntu Server içindeki paketleri güncelliyoruz ve yeni sürümlerini yüklüyoruz. Sistemi güncel tutmak çoğu zaman iyidir.
- Biz Türkçe klavye kullandığımız için öncelikle yazacığımız sistemde kolaylık sağlamak klavye sistemini Türkçe yapmak en doğrusu olacaktır. Eğer siz yapmak istemezseniz bu komutu kullanmanız gerekmiyor. Klavye düzenini Türkçe yapmak için :
```bash
sudo loadkeys tr
```
komutunu kullanarak sistemdeki klavye düzenimizi Türkçe yapıyoruz. **Eğer kalıcı olarak yapmak istersek :**
- **nano komutu text dosyaları üzerinde işlem yapmamızı sağlar.**
```bash
sudo nano /etc/default/keyboard
```
diyerek klavye konfigürasyon ayarlarına geliyoruz ve **XKBLAYOUT = "us"** kısmını **XKBLAYOUT = "tr"** olarak değiştiriyoruz. Ok tuşlarıyla imleci hareket ettirebilirsiniz. Bunu yaptıktan sonra *CTRL + X* tuş kombinasyonu yapıyoruz. Bu tuş kombinasyonunu sürekli kullanacağız. Bize bufferdan(Geçici bellek) diske yazılması hakkında bir soru soruyor. Buna y diyerek kaydedeceğimizi söylüyoruz sonrasında dosya ismini değiştirebileciğimiz bir kısım geliyor. Bu kısımda değiştirmeyeceğimiz için *ENTER* tuşuna basarak geçiyoruz. Artık klavyemiz türkçe düzenine döndü. Şimdi ise bir user oluşturmamız gerekiyor. 
- **User oluşturur.**
```bash
sudo adduser new_user
```
komutunu kullanarak user oluşturma yerine geliyoruz. Bu komutu girdikten sonra bize şifresinin ne olması gerektiğini soruyor. Şifremizi giriyoruz. Aynı şifreyi tekrar istiyor. Bunu da girdikten sonra bize isim, telefon gibi sorular soruyor. Bunları şuan için kullanmamız gerekmediği için *ENTER* diyerek geçiyoruz. Sonrasında y diyerek bilgilerin doğruluğunu kabul ediyoruz ve kullanıcımız oluştu. Şimdi ise kullanıcıya sudo yetkisi vereceğiz. Sudo yetkisi bütün sisteme erişebileceğimiz anlamına geliyor. Bu süreçten sonra o kullanıcıdan ilerleyeceğimiz için sudo yetkisi vermek bizim için iyi olacaktır.
- **new_user kullanıcısına sudo yetkisi verir.**
```bash
sudo usermod -aG sudo new_user
```
bu komutu da uyguladıktan sonra artık new_user kullanıcısına geçebiliriz. Yeni kullanıcıya geçmek için : 
- **su komutu kullanıcılar arası geçiş yapmak için kullanılır.**
```bash
su new_user
```
bu komuttan sonra gelen şifre yerine şifremizi yazdıktan sonra artık bu kullanıcının yerine geçtik.
- Şimdi Windows bilgisayarımızdan bağlanmak için *ssh* paketini indireceğiz. *ssh* paketi uzaktan bir bilgisayara ya da ağ içindeki bilgisayara bağlanmak için kullanılır. Biz windows bilgisayarımızdan başka bir bilgisayara bağlanmak için bu paketi kullanacağız.
- **Bu komut openssh-server paketini indirir ve kurar.**
```bash
sudo apt install openssh-server -y
```
komutunu kullanarak ssh servisini bilgisayarımıza indiriyoruz. **Sonrasında aktif etmek için :** 
- **Bu komut ssh servisini hem başlatır hem de her işletim sistemi açılışında otomatik çalıştırmak için kullanılır.**
```bash
sudo systecmtl enable ssh
```
- **Bu komut ssh durumunu kontrol eder.**
```bash
sudo systemctl status ssh 
```
komutuyla kontrol ediyoruz. Eğer Active kısmında **active** yanıyor ve yeşil ise başardık demektir. Şimdi ise makinemizi kapatmamız gerekiyor.
- **Makinemizi kapatmak için** 
```bash
sudo shutdown -h now
```
komutunu kullanıyoruz.
<h2 id="virtualbox">Virtual Box Konfigürasyonları</h2>
- Kurulum yaparken Virtual Box Manager Network ayarlarındaki varsayılan ağ modunu NAT olarak ayarlar. NAT modu kurduğumuz sanal makinenin dış dünyaya çıkarken IP adresi host olan IP adresi ile aynı olur. Bu bizim HOST makinemizden sanal makinemize bağlanmamız için sanal makinenin de bir IP alması gerekir. Bunu sağlamak için bu sunucu serverin ağ ayarlarını Bridged Adaptor yapmamız gerekiyor. Bridged adapter modu bu sanal makineyi internete bağlarken bu makineyi sanal değil fiziksel olarak algılar ve o sunucuya bir IP verir. Bu IP'yi kullanarak ssh bağlantısı gerçekleştireceğiz. Aşağıda nasıl ağ ayarlarını değiştirmemiz gerektiği açıklanmaktadır. 
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
komutunu kullanıyoruz. Bilgisayarımız açıldıktan sonra *new_user* kullanıcısına tekrar giriş yapıyoruz.
- Şimdi ise genellikle SSH için kullanılan portumuz yani 22 portunu açacağız.
- **Firewallda port izni vermek için** 
```bash
sudo ufw allow 22
```
komutunu girerek artık 22 portunu açmış bulunmaktayız. **Kontrol etmek için** 
```bash
sudo ufw status
```
yapıyoruz. Eğer 22 portu için **action** kısmında **allow** yazıyorsa **BAŞARDIK** demektir.
## SSH Key Oluşturma
- SSH Key'leri SSH protokolü kullılırken kullanılan bir şifreleme yöntemidir. SSH kullanılırken bu şifreleme yönteminde public keyler sunucuda saklanır ve SSH bağlantısında verilerini şifreler. Bu şifreyi sen sadece private key ile çözersin. Bu private key senin bilgisayarında saklanır ve bu sebeple bu veriyi sadece senin bilgisayarın çözebilir. Public ve Private keyler birlikte çalışır.
- Güvenlik için biz de bu yönetmi kullanacağız ve bir ssh keyi oluşturacağız. Bu keyler bir sonraki girişimiz için bizi tanıyacak ve ssh ile bağlantımızı hem güvenli hem de kolay bir hale getirecek.
- Windows bilgisayarımızda **Başlat** tuşuna sağ tıklayıp oradaki çıkan seçeneklerden terminal(admin) açıyoruz. ve terminale
```bash
ssh-keygen
```
yazıyoruz. Normal ismimizin kalmasını istediğimiz için **ENTER** tuşuna basarak ilerliyoruz. Extra bir şifreleme istiyorsanız passphrase kısmında bağlanırken kullanmanız için şifrenizi girebilirsiniz. Artık ssh-keyimiz oluşturuldu. Ssh keyimizi öğrenmek için ssh klasörüne gidelim.
- **.ssh klasörüne girer.**
```bash
cd .ssh
```
komutunu kullanıyoruz.
- **O klasör içindeki dosyaları listeler.(PowerShell)**
```bash
ls
```
- **O klasör içindeki dosyaları listeler. (CMD)**
```bash
dir
```
komutunu kullanarak hangi dosyalar olduğunu görelim. Orada bulunan .pub uzantılı dosya içinde bizim public ssh-keyimiz mevcut. Öğrenmek için
- **Dosyayı okur. (PowerShell)**
```bash
cat {.pub uzantılı dosya ismi}
```
- **Dosyayı okur. (CMD)**
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
- Windows cihazımızdan bağlanmak için PowerShell'i kullanacağız. Eğer PowerShell yoksa veya kullanılamıyorusa Putty programını kullanarak SSH bağlantısını gerçekleştirebilir. Bu dökümantasyonda sadece PowerShell ile giriş açıklanacaktır. İnternetten kısa bir araştırmayla Putty ile nasıl gireceğinizi öğrenebilirsiniz.
- Powershell terminaline geldik. Şimdi bağlanmak için
- **SSH bağlantısı ile bağlar.**
```bash
ssh new_user@{ip adresi} -p 22
```
komutunu kullanarak kullanıcı adınız farklı ise kullanıcı adınızı yazın. Şifrenizi girerek giriş yapabilirsiniz. Artık ssh ile giriş yapmış bulunmaktayız.
## SSH Key İle Kolay ve Güvenli Giriş
- Şimdi ssh keyimizi tanımlayarak şifre girmeden girebileceğiz ve başka bir bilgisayardan bağlantı sağlandığında onu direkt reddedecek.
- Windows bilgisayarımızdan girişi sağlamıştık ve *.pub* dosyasındaki ssh keyimizi kopyalamıştık. Bu dosyayı yazacağımız dosyayı oluşturacağız.
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.authorized_keys
```
komutlarını çalıştarak dosyamızı oluşturuyoruz ve izinlerini ayarlıyoruz. *mkdir* komutu klasör oluşturmak için kullanılıyor. *chmod* komutu dosya veya klasörler için kullanıcı ve grup için izinleri ayarlayan bir komuttur. 700 kodunu kullanarak bizim kullandığımız kullanıcı için *.ssh* klasöründe okuma, yazma ve çalıştırma izni vermiştir. *touch* komutu yeni bir dosya oluşturmak için kullanılan bir komuttur. Touch komutu ile keylerimizi tutacağımız bir *.authorized_keys* dosyası oluşturduk. Sonrasında bu dosyanın iznini diğer kullanıcı ve gruplardan alarak sadece kendimiz için okuma,yazma izni vermek için *chmod* komutu ile 600 parametresini kullandık.
- Şimdi ise o dosya içine bu ssh keyimizi yapıştıracağız. Bunun için
```bash
nano ~/.ssh/authorized_keys
```
komutunu kullanıyoruz ve bu *.pub* dosyası içindeki ssh keyimizi buraya kopyalayıp yapıştırıyoruz. Sonrasında çıkmak için **CTRL+X** tuşlarına basıyoruz. Sonrasında y tuşuna basıyoruz ve **ENTER** yaparak çıkış yapıyoruz. 
- Şuan bizim bilgisayarımızı tanıyor fakat biz şifreyi kaldırmak istiyoruz ve root kullanıcısı ile ssh girişi istemiyoruz. Bunun için config dosyasındaki izinleri kaldıracağız.
```bash
sudo nano /etc/ssh/sshd_config
```
komutunu kullanarak config dosyasını açıyoruz. **CTRL+W** tuşlarına basarak aşağıdaki ayarları aratarak bunları karşısındaki parametre ile değiştireceğiz ve eğer başında **#** işareti varsa kaldıracağız.
```bash
PermitRootLogin no                          // Root Girişini engeller.
PubKeyAuthentiation yes                     // Public key ile doğrualamayı açar.
AuthorizedKeysFile .ssh/authorized_keys     // Oluşturduğumuz public keylerin yerini tanıtıyoruz.
PasswordAuthentication no                   // Şfire ile girişi kaldırıyoruz.
UsePAM no                                   // Şifreli girişleri kapatır.
```
kısımları bu şekil olacak şekilde değiştiriyoruz ve kaydediyoruz.
- **SSH servisini sıfırlar.**
```bash
sudo systemctl restart ssh
```
komutunu kullanarak ssh servisini yeniden başlatıyoruz.
- Şimdi Windows bilgisayarımızda deneyelim. Zaten bilgisayarımızda girmiştik. Bilgisayarımız ile girmiştik zaten ssh bağlantısını koparmak için
```bash
exit
```
komutunu kullanıyoruz ve tekrar ssh ile bağlantı yapmaya çalışınca(daha önce gösterilen şekilde) artık bağlantı direkt sağlanıyor. Ayrıca root kullanıcı ile giriş de iptal olmuş oldu. Eğer root ile bağlanmadığını görmek isterseniz sadece new_user kısmını root olarak değiştirebilirsiniz.

## Apache Web Server Kurulumu
- Şimdi ise Apache kuracağız. Apache yaygın olarak kullanılan özgür, açık kaynak bir yazılımdır. Bu yazılım HTTP portu üzerinde çalışan bir web sayfalarını istemcilere dağıtmak için kullanılır.
- **Apache'yi kurar.**
```bash
sudo apt update
sudo apt install apache2 -y
```
komutlarını kullanarak Apache Web Serverini indirelim. İndirdikten sonra 
- **Sistem her açıldığında başlatmak için** 
```bash
sudo systemctl enable apache2
```
- **Apache'nin durumunu kontrol etmek için** 
```bash
sudo systemctl status apache2
```
komutlarını kullanabiliriz.
## Apache Konfigürasyonları
- Aynı Ip adresini yönlendireceğim için Virtual Host olarak kullanmalıyım Apache'de bu Virtual hostları ayarlamak için
```bash
cd /etc/apache2/sites-available
```
komutunu kullanarak Apache'nin bazı config dosyalarının olduğu klasöre girdik. Burada her site için aşağıda bulunan komutunları kullanıyoruz. Gördüğünüz üzere buğday.org.conf dosyası için *.conf* dosyasının ismi buğday.org.conf değil. Bunun sebebi Türkçe karakter ve ya Çince gibi farklı karakterler kullanımı. Uyum açısı yüzünden bu site için PunyCode çevirimi yapmamız gerekiyor. PunyCode bu tarz karakterler için farklı bir kod oluşturan bir özelliktir. Biz bu özelliği kullandığımız için buğday.org.conf dosyasının Punycode'u olaran *xn--buday-l1a.org.conf* ismini kullandık ve bu şekilde ilerleyeceğiz.
```bash
sudo nano 2025ozgur.com.conf
sudo nano bugday.org.conf
sudo nano xn--buday-l1a.org.conf
```
dosyalarını teker teker oluşturduk ve bu *.conf* uzantılı dosyalarını içeriğini

- **bugday.org.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@bugday.org                   // Bu sayfa adminin e- posta adresi herhangi bir hata durumunda mail gidecek.
    ServerName bugday.org                             // Domain Name kısmı 
    DocumentRoot /var/www/bugday.org/wordpress/        // Web sitesinin ana dizinini belirler.

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log            // Hata loglarını kaydedecek dosyayı belirler.
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined            // Bu ziyaretçilerin erişim loglarını kaydedecek dosyayı belirler.
</VirtualHost>
```

- **buğday.org.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@xn--buday-l1a.org
    ServerName xn--buday-l1a.org
    DocumentRoot /var/www/bugday.org/wordpress/

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```

- **2025ozgur.com.conf için**
```plaintext
<VirtualHost *:80>
    ServerAdmin new_user@2025ozgur.com
    ServerName 2025ozgur.com
    ServerAlias www.2025ozgur.com        //Extra domainler belirler. Birden fazla olabilir.
    DocumentRoot /var/www/2025ozgur.com/

    <Directory /var/www/2025ozgur.com/yonetim> 
        AllowOverride AuthConfig             // yonetim dizini için şifreli bir giriş uygular.
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
buradaki gibi değiştirdik. Konfigürasyon dosyalarımı oluşturduk. Şimdi ise bu konfigürasyon dosyalarının root klasörlerini oluşturacağız. Bunun için 
```bash
cd /var/www 
```
diyerek Apache'nin Web dosyalarının bulunduğu klasöre geliyoruz. Burada bulunan *html* klasörünü silmek istiyoruz. Bunun için 
```bash
sudo rm -rf html 
```
komutunu kullanıyoruz ve kaldırıyoruz. Kaldırdıktan sonra yeni root dosyalarını oluşturacağız.
- **Klasör oluşturur**
```bash
sudo mkdir bugday.org
sudo mkdir 2025ozgur.com && sudo mkdir 2025ozgur.com/yonetim
```
klasörlerini oluşturuyoruz. *bugday.org* ve *buğday.org* aynı Wordpress'e bağlanacağı için extra bir klasör oluşturmadık. Apache'nin konfigürasyon dosyalarını ve Web klasörlerini oluşturduk.
- **Apache'nin bu konfigürasyon dosyalarının yapılandırmasını sağlamak için**
```bash
sudo a2ensite bugday.org.conf
sudo a2ensite 2025ozgur.com.conf
sudo a2ensite xn--buday-l1a.org.conf
```
komutlarını kullanıyoruz.
- **Apache servisini tekrar başlatalım.**
```bash
sudo systemctl restart apache2
```
- Artık Apache'yi hem konfigüre ettik ve yapılandırdık. Şimdi veri tabanı kurulumuna geçelim.
## Veri Tabanı Kurulumu
- Öncelikle yapacağımız işlem yeni bir sanal makine daha oluşturmaktır. Bu sanal makinenin kurulumunu yukarıdak anlatıldığı şekilde yapalım. [Virtual Box Konfigürasyonları](#virtualbox) kısmında belirtilen konfigürasyonları da yapmalıyız fakat SSH bağlantısı için herhangi bir ayar yapmamıza gerek yok. Çünkü bu makineye SSH ile bir bağlantı sağlamayacağız. Yani extra SSH kurulumu yapmamıza gerek yoktur.
- Bu sanal makinemizde veri tabanı oluşturacağız. Bu veri tabanını Wordpress için kullanacağız. Kullanacağımız veri tabanı MySQL olacaktır. MySQL ilişkisel bir veri tabanı yönetim sistemidir.
- **MySQL kurulumunu yapmak için** 
```bash
sudo apt install mysql-server -y
```
komutunu kullanıyoruz ve mysql-server'i indiriyoruz.
- **Sistemde sürekli çalıştırmak için**
```bash
sudo systemctl enable mysql
```
komutunu kullanıyoruz ve çalıştırmaya başlıyoruz.
## MySQL Konfigürasyonları
- Şimdi MySQL'e dışarıdan bağlantı sağlamak için dışarıdan bağlantıyı açacağız ve diğer sunucudan bağlantısı için konfigürasyonunu yapacağız.
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
komutunu kullanarak mysql için konfigüre edeceğimiz dosyayı açıyoruz. Bu dosya içinde aramak yapmak için **CTRL+W** kombinasyonunu kullanarak *bind-address* 'i bularak değeri aşağıda belirtildiği şekilde değiştiriyoruz. Normalde buluna değer sadece o sanal makine içinde bağlanmamızı sağlarken bu değiştirdiğimiz değer MySQL serverine sadece host sunucudan değil diğer sanal makineden de bağlanmamızı sağlayacak.
```plain text
bind-address = 0.0.0.0
```
- Bu işlemi de yaptıktan sonra MySQL sunucusunu yeniden başlatacağız.
- **MySQL sunucusunu yeniden başlatmak için**
```bash
sudo systemctl restart mysql
```
komutunu çalıştırıyoruz. MySQL konfigürasyonlarını da yapmış bulunmaktayız. Şimdi MySQL'in genel olarak kullanıldığı 3306 portunu açacağız. Daha önce yaptığımız gibi FireWall yazılımını açalım.
```bash
sudo ufw enable
sudo ufw allow 3306
sudo reboot
```
komutlarını kullanarak FireWall yazılımını açtık. 3306 portuna izin verdik ve bilgisayarımızı yeniden başlattık. Yeniden başladıktan sonra *new_user* kullanıcısıa geçiş yapalım.
## MySQL Database Oluşturma
- Şuanki bölümde MySQL içinde Wordpress için bir database oluşturacağız. Öncelikle oluşturmak için MySQL' giriş yapmalıyız.
```bash
sudo mysql
```
komutunu kullanarak artık MySQL terminaline giriş yaptık. Bu terminalde önce Database oluşturacağız.
```bash
CREATE DATABASE wordpress_db DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```
komutunu kullanıyoruz. Bu komut *wordpress_db* adında bir veri tabanı oluşturur. Sonrasında bir kullanıcı oluşturacağız.
- **Kullanıcı oluşturmak için**
```bash
CREATE USER 'wp_user'(Kullanmak istediğiniz kullanıcı adı)@'%'(Bütün her yerden erişim için) IDENTIFIED BY 'password'(koyacağınız şifre);
```
**NOT = GİRECEĞİNİZ KULLANICI ADI VE ŞİFRENİZİ UNUTMAYIN BU KULLANICI ADI VE ŞİFREYİ KULLANACAĞIZ**
- **Bu veri tabanı üzerinde bu kullanıcıya yetki vermek için**
```bash
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;
```
komutlarını kullanıyoruz. Bu MySQL ile artık işimiz kalmadı. Artık MySQL terminalinden çıkış yapalım. 
- **Terminalden çıkış yapmak için**
```bash
exit;
```
komutunu kullanıyoruz.
- Bunları yaptıktan sonra artık Wordpress kurulumuna geçebiliriz.
## Wordpress Kurulumu
- Az önce kurduğumuz veri tabanı farklı bir sanal makinedeydi. Şuan kuracağımız paketleri yine en başta Apache Server kurduğumuz sanal makineye kuracağız.
- Öncelikle Wordpress kurulumu için *php* paketi gerekiyor.
- **php paketini kurmak için**
```bash
sudo apt update
sudo apt install php libapache2-mod-php php-mysql php-xml php-mbstring php-curl -y
```
komutlarını kullanarak *php* için gerekli olan en basit paketleri indiriyoruz.
- Şimdi php paketlerinin Apache ile daha düzgün çalışmasını sağlamamız gerekiyor.
- **php paketlerini Apache ile uyumlu yapmak için**
```bash
sudo a2enmod php
sudo systemctl restart apache2
```
komutlarını kullanarak php ile Apache Web Serverini daha düzgün çalışmasını sağladık ve Apache Web Serveri yeniden başlattık. **Eğer bu komut hata verirse**
```bash
php -v 
```
komutunu kullanarak versiyonunu öğrenelim. Bu dökümantasyon yazılırken php versiyonu 8.3.6'dır. Bu versiyon için
```bash
sudo a2enmod php8.3
```
komutunu kullanarak Apache'nin php için uyumlu çalışmasını sağlayabiliriz.
- Şimdi ise Wordpress'in kurulumuna geçelim. Kurulum yapacağımız klasöre giriş yapacağız ve bu klasörün içine Wordpress kurulumunu yapacağız. Bu Wordpress kurulumu yapacağımız klasör bugday.org olarak oluşturduğumuz klasör olacak. Bu klasöre geçiş yapmak için; 
```bash
cd /var/www/bugday.org && sudo mkdir wordpress && sudo chown -R www:data:www:data wordpress/
```
komutunu kullanıyoruz. Bu komut /bugday.org klasörü içinde wordpress adlı bir klasör oluşturur ve izinlerini ayarlar. Web sunucuları www-data kullanıcısını kullanır. Bu izin bu dosyalara bu Web sunucusunda ayar yapabilecek yetkideki kullanıcılara yetki verir. -R parametresi altındaki bütün dizinler için bunu geçerli hale getirir. Şimdi *wp-cli* uygulamasını indireceğiz.
- Şimdi ise *wp-cli*'yi indireceğiz ve artık bütün işlemleri terminal üzerinden gerçekleştireceğiz. *wp-cli*'yi indirmek için (Bu indirmeyi wordpress klasörü içinde yaparsak daha garanti olur);
```bash
cd /var/www/bugday.org/wordpress && sudo curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
``` 
komutunu kullanıyoruz. Bu *wp-cli* komutlarını her klasör içinde kullanabilmek için ;
```bash
sudo chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```
komutlarını kullanıyoruz. Bu komutların biri wp-cli.phar dosyasını çalıştırılabilir hale getirir ve /usr/local/bin klasörü içinde *wp* adlı bir dosyaya yazar. Şimdi Wordpress'in *wp-cli* ile kurulumunu sağlayacağız.
## Wordpress Konfigürasyonları
- Şimdi *wp-cli* ile kuracağız. Bunun için ;
```bash
wp core download
```
komutunu kullanıyoruz. Bu komut Wordpress dosyalarını indirir. Şimdi konfigürasyon dosyasını oluşturacağız. Bunun için ;
```bash
wp config create --dbname=wordpress_db --dbuser=wp_user --dbpass=sifre --dbhost=Veri_tabanı_IP 
```
komutunu kullanarak uygun yerleri kendimiz dolduruyoruz. Konfigürasyon dosyasını oluşturduktan sonra artık Wordpress'i kuracağız. Bu kurulum için;
```bash
wp core install --url="bugday.org" --title="bugday.org" --admin_user="admin" --admin_password="admin_şifresi" --admin_email="mgk@bugday.org"
```
- Artık Wordpress'imizin kurulumu sağlandı. Apache'yi tekrardan başlatalım.
```bash
sudo systemctl restart apache2
```
- Denemek için verdiğiniz domain name adresine girebilirsiniz.
## wp-cli Kullanarak Post Atma
- Öncelikle *wordpress* klasörünün içine girelim. Klasörün içine girmek için ; 
```bash
cd /var/www/bugday.org/wordpress
```
komutunu kullanalım. Sonrasında buraya atacağımız gönderiler için bir klasör oluşturalım. 
```bash
sudo mkdir wp-content/uploads/txt-files
```
komutunu kullanalım ve *txt-files* içerisine bir *.txt* dosyası oluşturalım ve içine yazılarımızı yazalım.
```bash
sudo nano wp-content/uploads/txt-files/post1.txt
```
komutunu kullanalım ve içerisine paylaşacağımız yazıyı yazalım. Sonrasında kaydedip çıkalım. Şimdi *wp-cli* kullanarak post oluşturacağız.
- **Post oluşturmak için**
```bash
wp post create --post_title="Wordpress Kurulumu" --post_content="$(cat /var/www/bugday.org/posts/post1.txt) --post_status=publish --post_author=1 --post_name="wordpress-kurulumu"
```
komutunu kullanarak başlığı Wordpress Kurulumu, içeriği post1.txt dosyasında yazdığımız yazı, yayınlanmış, kullanıcısı 1'numaralı kullanıcı olan ve linkinin SEO uyumlu olması için konuyla alakalı olan bir post oluşturmuş olduk. Siz bunları değiştirerek kullanabilirsiniz. Artık postumuzu attık. Kontrol etmek için domain namemize girebiliriz. Postumuz ana sayfada gözükecek.
- Postumuzu da oluşturduğumuza göre artık 2025ozgur.com adlı domain isminin ayarlamalarına geçebiliriz.
## 2025ozgur.com Sitesinin Oluşturulması
- Daha 2025ozgur.com için klasörü önce oluşturmuştuk fakat herhangi bir site oluşturmamıştık. Siteyi oluşturmak için öncelikle o klasöre gitmemiz gerekiyor.
```bash
cd /var/www/2025ozgur.com
```
yaparak o dizine gittik burada bir yönetim klasörü var ve buraya ulaşım kullanıcı adı ve şifre ile olmalıdır. Bu klasöre şifre koymak için öncelikle Apache'nin temel kimlik doğrulamasını etkinleştirmemiz gerekiyor.
- **Apache'nin kimlik doğrulamasını etkinleştirmek için**
```bash
sudo a2enmod auth_basic && sudo systemctl restart apache2
``` 
komutlarını kullanıyoruz ve Apache kimlik doğrulamasını açtık ve Apache web serveri tekrar başlattık. Şimdi bunun için ilk kullanıcımızı ekleyeceğiz.
- **Apache'nin kimlik doğrulaması için kullanıcı eklemek için**
```bash
sudo htpasswd -c /etc/apache2/.htpasswd admin
```
komutunu kullanıyoruz. *.htpasswd* dosyası Apache Web sunucusu için yetkilendirilecek kişilerin kullanıcı adı ve şifre bilgilerinin barındırıldığı dosyadır. Bu dosyaya bir *admin* adında bir kullanıcı ekledik. Bu komutu kullandıktan sonra bizden şifre isteyecek. *admin* kullanıcısı için şifremizi belirleyelim ve girelim.
- Şimdi ise bu */yonetim* adlı klasöre erişim için *.htaccess* dosyası oluşturup düzenleyeceğiz. *.htaccess* dosyası herhangi bir web sunucusunun kullanıcı isteklerine nasıl yanıt vereceğini kontrol altına almaya yarayan bir dosyadır.
- Şimdi bu dosyayı */yonetim* adlı klasörün içine oluşturalım ve oraya bir kimlik doğrulaması ekleyelim.
- *htaccess* dosyasını oluşturalım.
```bash
sudo nano /var/www/2025ozgur.com/yonetim/.htaccess
```
komutunu kullanalım ve içine 
```plaintext
AuthType Basic                //Doğruluma türünü belirler.
AuthName "yonetim"            //Doğrulama ismini belirtir.
AuthUserFile /etc/apache2/.htpasswd     //.paswd dosyasının yolunu belirledik. Biz buraya oluşturmuştuk.
Require valid-user                    // Kullanıcı adı ve şifre ile giriş olacağını söyledik.
```
textini ekleyelim. Bu ekleme işlemi */yonetim* klasörüne artık sadece .htpasswd içindeki kullanıcıların şifreleriyle birlikte girmesini sağlayacak. Bunu oluşturduktan sonra bu dosya içine bir *index.html* dosyası ekleyelim.
- ***index.html* dosyasını oluşturur.**
```bash
sudo nano /var/www/2025ozgur.com/yonetim/index.html
```
sonrasında içine küçük bir *html* kodu ekleyelim.
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basit HTML Sayfası</title>
</head>
<body>
        <h1>Hoş Geldiniz Yonetim</h1>
        <p>Bu basit bir HTML sayfası örneğidir.</p>
</body>
</html>
```
kodunu yapıştıralım. Bu kod basit bir *html* sayfası örneğidir. Bu sayfaya sadece yonetimde olan insanlar erişebilir olacak.
- Şimdi kullanıcılar için bir *html* dosyası oluşturalım. Bu *html* dosyasına herkes ulaşabilir olacak. Bu *html* içinde 100 kere **Kullanıcılarımın kişisel verilerini toplamayacağım.** yazacak.
- Öncelikle *html* dosyasını oluşturalım. Bu *html* dosyasını */yonetim* klasörü altına oluşturmayacağız.
```bash
sudo nano /var/www/2025ozgur.com/index.html
```
komutunu kullanalım ve içine 
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Veri Politikası</title>
</head>
<body>
    <h1>Veri Politikası</h1>
    <div>
        <!-- 100 kere yazdırma -->
        <script>
            for (let i = 1; i <= 100; i++) {
                document.write(`<p>${i}. Kullanıcılarımın kişisel verilerini toplamayacağım.</p>`);
            }
        </script>
    </div>
</body>
</html>
```
kodunu yazalım. Bu koddaki *script* *div* etiketi içine 100 tane *p* etiketi oluştururken bu *p* etiketlerinin içine **Kullanıcılarımın kişisel verilerini toplamayacağım.** yazacak ve bu i değişkeniyle hepsini 1'den 100'e kadar sıralayacak.
- Şimdi Apache'mizi tekrar başlatalım.
```bash
sudo systemctl restart apacahe2
```
- Apache serverini tekrar başlatarak verilen bütün görevleri başarıyla gerçekleştirmiş olduk.Bu dökümantasyonda; Sanal makine kurulumu, herhangi bir sunucuya SSH bağlantısı sağlanarak nasıl ulaşacağımızı, SSH key ile güvenli bağlantıyı, Apache Web Server kurulumunu, bu Apache'yi nasıl konfigüre edeceğimizi, Apache Web Server'ine Wordpress kurulumunu, basit *wp-cli* komutlarını ve yönetim için olan url'lere nasıl kullanıcı adı ve şifre ile bağlanacağımızı öğrenmiş olduk. 
