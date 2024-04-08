
<div align="center">
    <h1>CMake'in Temel Kullanımı</h1>
    <img src="../images/CMakeTemelKullanim//start.svg" alt="İçerik Başlığının Üst Görseli" style="width:40%; height:40%;"/>
    <p><a href="https://storyset.com/illustration/launching/amico">İllüstrasyon Kaynak Bağlantısı</a></p>
</div>

## Ön Hazırlık

İlerleyen süre zarfında gerçekleştirilecek tüm örnekler Linux (Ubuntu 22.04) işletim sistemine sahip bir cihaz aracılığıyla yürütülecek, anlatılacaktır. Fakat küçük bir araştırmayla istediğiniz işletim sisteminde benzer süreçleri yürütebilirsiniz.

CMake'in resmi öğretici dokümantasyonlarına ise [buradan](https://cmake.org/cmake/help/latest/guide/tutorial/index.html) ulaşabilirsiniz.

İhtiyacımız olan birkaç bileşen var. Bunlar, C++ kodlarını derleyebilmek ve çalıştırmak için GCC derleyicisini barındıran **build-essential** debian paketi (farklı bir derleyici (CLang, Ninja) kullanmak isterseniz o derleyiciye ait paket), CMake ile çalışabilmek için ise cmake debian paketine ihtiyacımız var.

    sudo apt install build-essential
    sudo apt install cmake

## Örnek

Temel CMake kullanımını görmek için basit bir C++ kodu yazarak süreci ilerleteceğiz. Herhangi bir metin editörü veya terminal üzerinden aşağıda yer alan dosya yapısını oluşturarak başlayalım.

<div align="center">
    <img src="../images/CMakeTemelKullanim/FirstExampleDirectory.png" alt="Ilk Örneğin Dizin Yapısı" style="width:40%; height:40%;"/>
</div>

"main.cpp" dosyamınızın içerisine meşhur örneğimizi yazarak hazır hale getirelim.

    #include <iostream>

    int main() {
        std::cout << "Hello World!" << std::endl;
        return 0;
    }

CMake direktiflerimizi belirtmek için kök dizininde önceden oluşturduğumuz **"CMakeList.txt"** dosyasını düzenleyelim.

    cmake_minimum_required(VERSION 3.22.1)
    project(CMakeLearn VERSION 0.0.1)      
    add_executable(CMakeLearn src/main.cpp)

Projemizin derlenerek kullanılabilmesi için minimum CMake versiyonunun 3.22.1 olması gerektiğini, projemizin adının CMakeLearn ve versiyonun 0.0.1 olduğunu, projeye dair yürütülebilir dosya ekleneceğini ve bu dosyanın kaynak olarak src altında yer alan main.cpp aracılığıyla oluşturularak CMakeLearn adıyla kaydedilmesini belirlemiş olduk.

İlk kısımda bahsettiğimiz kısıtlama ve bağımlıklar için bir örneğimizde olmuş oldu. CMake versiyonunu kısıtlamış olduk. Bu sayede başka bir cihaz veya platformda aynı CMake dosyası kullanılarak proje derlenmeye çalışıldığında versiyon kontrol edilecek ve uyumsuzluk olması durumunda hata alınacaktır. 

Ayrıca ek olarak aşağıda yer alan iki satırın "CMakeLists.txt" dosyasına eklenmesi durumunda projenin C++17 standardını desteklediği ve derleme işlemi için zorunlu olarak C++17 standardını destekleyen bir derleyiciye sahip olunması gerektiğini kabul eder. Eğer required değeri false olarak set edilirse zorunluluk şartı bulunmaz.

    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED true)

Her şeyimiz hazır. Şimdi terminal aracılığıyla build dizine gidelim. Belirlemiş olduğumuz direktifleri (**CMakeLists.txt**) kullanarak CMake'i çalıştıralım. CMake bizler için gerekli dosyaları ve içerikleri oluşturacak, belirlenen direktifleri kontrol edecektir.

    cd build/
    cmake ..

<div align="center">
    <img src="../images/CMakeTemelKullanim/buildDirectory.png" alt="Build dizini ve CMake Komutu" style="width:80%; height:80%;"/>
</div>

Pekala, şimdi bu komut sonucunda kabaca neler oldu? 

<div align="center">
    <img src="../images/CMakeHakkindaGenelBilgiler/CMakeHelpCommand.png" alt="CMake Help Komutu" style="width:80%; height:80%;"/>
</div>

CMake, sistem içerisinde varsayılan olarak derleyicileyi tanımlamış (Unix Makefiles), çeşitli özellikleri kontrol ettikten sonra yapılandırma dosyalarını oluşturarak **build** dizini içerisine yazmıştır.

<div align="center">
    <img src="../images/CMakeTemelKullanim/CMakeOutput.png" alt="CMake Komutu Sonucunda Oluşan Dosyalar" style="width:80%; height:80%;"/>
</div>

<div align="center">
    <img src="../images/CMakeTemelKullanim/buildCMakeFIlesDirectory.png" alt="CMake Komutu Sonucunda Oluşan Dosyalar" style="width:80%; height:80%;"/>
</div>

Oluşan yapılandırma dosyalarında önemli bir dosyayı fark ettiniz mi? 

Derleyici ve işletim sistemine uygun olarak belirttiğimiz direktiflere sahip GCC derleyicisine uygun olan nesne dosyaları, geçici ve ara dosya içerikleriyle birlikte bir **Makefile** dosyası elde ettik. Bu dosya aracılığıyla artık projemizi derleyebilir ve çalışabilir hale getirebiliriz.

    make

Komutu aracılığıyla projemizi derliyoruz.

<div align="center">
    <img src="../images/CMakeTemelKullanim/makeCommand.png" alt="Make Komutu" style="width:80%; height:80%;"/>
</div>

Derleme işlemi tamamlandığında ise CMake dosyasında belirttiğimiz gibi dizin içerisinde "CMakeLearn" adıyla yürütülebilir dosyanın oluştuğunu göreceğiz. Test için çalıştırdığımızda "Hello World!" çıktısını başarıyla görüntülüyoruz.

## CMake Kullanarak Farklı Derleyiciyle Uygun Makefile Oluşturmak

CMake'in bizlere sağladığı kolaylığı daha iyi anlamak için aynı kod içeriğini Ninja derleyicisiyle derleyeceğiz. (**Bu işlem için Ninja derleyicisine sahip olmanız gerekir.**) İlk olarak build dizinimizi temizleyelim ve yeniden Makefile dosyasını oluşturmak için CMake'i kullanalım.

    cd build/
    rm -rf *
    cmake -GNinja ..

<div align="center">
    <img src="../images/CMakeTemelKullanim/NinjaBuild.png" alt="CMake Komutuyla Ninja Derleme Dosyalarının Oluşturulması" style="width:80%; height:80%;"/>
</div>

Oluşan dosya çıktılarını incelediğimizde yukarıda yer alan çıktılardan farklı olduğunu görebilirsiniz. Şimdi hızlıca ninja komutuyla (make yerine ninja kullanılıyor) derleme gerçekleştirelim ve yürütülebilir dosyayı çalıştırarak "Hello, World!" çıktısına alalım.

    ninja

<div align="center">
    <img src="../images/CMakeTemelKullanim/NinjaCompile.png" alt="Ninja Komutu" style="width:80%; height:80%;"/>
</div>


Farklı derleyicilerle derleme yapmak istediğinde komut satırı üzerinden parametre geçmek zorunda değilsiniz. Bunun yerine "CMakeLists.txt" dosyasını düzenleyebilir veya terminal üzerinde "CMAKE_GENERATOR" parametresini de set edebilirsiniz.

# Sonuç

CMake yardımıyla basit bir proje kodunun yönetimini, farklı derleyiciler için nasıl kullanılabileğini, platformlar arası taşınabilirlik ve zaman kazanımı için bizlere yardımcı olma potansiyelini görmüş olduk.


# Kaynakça

* ChatGPT 3.5'ten yardım alınmıştır.
* [CMake 2.8.12 Documentation](https://cmake.org/cmake/help/v2.8.12/cmake.html)
* [How to CMake Good - 0b - Running CMake](https://www.youtube.com/watch?v=lI2nwZSMvlE)
* [CMake Tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)
* [How to Use Ninja](https://gist.github.com/jrhemstad/811d1e41af9685670241fa472170ef32)

<div align="center">
    <a href="CMakeHakkindaGenelBilgiler.md"> < Önceki Sayfaya Dön</a> ---  
    <a href="#"> Sonraki Sayfaya İlerle ></a>
</div>
