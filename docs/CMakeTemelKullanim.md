
<div align="center">
    <h1>CMake'in Temel Kullanımı</h1>
    <img src="../images/CMakeTemelKullanim/start.svg" alt="İçerik Başlığının Üst Görseli" style="width:40%; height:40%;"/>
    <p><a href="https://storyset.com/illustration/launching/amico">İllüstrasyon Kaynak Bağlantısı</a></p>
</div>

## Ön Hazırlık

İlerleyen süreçte gerçekleştirilecek tüm örnekler Ubuntu 22.04 dağıtımına sahip Linux işletim sistemi üzerinde yapılmıştır. Ancak benzer adımları küçük bir araştırmayla istediğiniz işletim sisteminde yürütebilirsiniz.

Kullanım örneğine geçmeden önce, C++ kodlarını derlemek ve çalıştırabilmek için GCC derleyicisini içeren **build-essential** debian paketi (farklı bir derleyici (Clang vb.) kullanmak isterseniz o derleyiciye ait paket), CMake ile çalışabilmek için ise **cmake** debian paketine ihtiyacımız var.

    sudo apt install build-essential
    sudo apt install cmake

## Örnek

Temel CMake kullanımını görmek için basit bir C++ kodu yazarak süreci ilerleteceğiz. Herhangi bir metin editörü veya terminal üzerinden aşağıda yer alan dizin yapısını oluşturun.

<div align="center">
    <img src="../images/CMakeTemelKullanim/FirstExampleDirectory.png" alt="İlk Örneğin Dizin Yapısı" style="width:40%; height:40%;"/>
</div>

"main.cpp" dosyasına aşağıdaki kodu ekleyin:

    #include <iostream>

    int main() {
        std::cout << "Hello World!" << std::endl;
        return 0;
    }

Kök dizininde önceden oluşturduğumuz **"CMakeList.txt"** dosyasını düzenleyerek CMake direktiflerini belirtin:

    cmake_minimum_required(VERSION 3.22.1)

    project(
        CMakeLearn
        VERSION 0.0.1
        DESCRIPTION "CMake Learn Application"
        LANGUAGES CXX
    )

    add_executable(CMakeLearn src/main.cpp)

Projemizin derlenmesi ve kullanılabilmesi için minimum CMake versiyonunun 3.22.1 olması gerektiğini, projemizin adının CMakeLearn ve versiyonun 0.0.1 olduğunu, projeye dair yürütülebilir dosyanın src altında yer alan main.cpp aracılığıyla oluşturularak CMakeLearn adıyla kaydedilmesini belirlemiş olduk.

İlk kısımda bahsettiğimiz kısıtlama ve bağımlıklar için örnek durum oluşmuş oldu. CMake versiyonunu belirttiğimiz için, başka cihaz veya platformda aynı CMake dosyası kullanılarak proje derlenmeye çalışıldığında versiyon uyumsuzluğu söz konusuysa hata alınacaktır. 

Ayrıca aşağıda yer alan satırların "CMakeLists.txt" dosyasına eklenmesi durumunda projenin C++17 standardını desteklediği, derleyicinin g++ olması ve C++17 standardını desteklemesi gerektiğini belirtebiliriz. Eğer required değeri false olarak set edilirse zorunluluk şartı bulunmayacaktır.

    set(CMAKE_CXX_XOMPILER g++)
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED true)

Şimdilik bu düzenlemeri dışarıda bırakıyoruz. Her şeyimiz hazır. Terminal aracılığıyla build dizine giderek CMake'i çalıştırın. CMake derleme işlemi için gerekli olan dosyaları ve içerikleri belirlediğimiz direktiflere (**CMakeLists.txt**) göre oluşturacaktır.

    cd build/
    cmake ..

<div align="center">
    <img src="../images/CMakeTemelKullanim/buildDirectory.png" alt="Build dizini ve CMake Komutu" style="width:80%; height:80%;"/>
</div>

Bu komutun çıktısını incelediğimizde, CMake'in varsayılan olarak seçili olan Unix Makefiles standartına göre yapılandırma dosyalarını ve içeriklerini GNU/GCC 11.4.0 versiyonuna sahip "/usr/bin/cc" dizininde yer alan derleyicimize uygun olarak **build** dizini içerisine otomatik olarak oluşturduğunu görebilirsiniz.

Seçili olan makefile standardını aşağıda yer alan komut aracılığıyla kolaylıkla kontrol edebilirsiniz.

    cmake --help

<div align="center">
    <img src="../images/CMakeHakkindaGenelBilgiler/CMakeHelpCommand.png" alt="CMake Help Komutu" style="width:80%; height:80%;"/>
</div>

CMake'in oluşturduğu dosya ve içeriklerini inceleyelim.

<div align="center">
    <img src="../images/CMakeTemelKullanim/CMakeOutput.png" alt="CMake Komutu Sonucunda Oluşan Dosyalar" style="width:80%; height:80%;"/>
</div>

<div align="center">
    <img src="../images/CMakeTemelKullanim/buildCMakeFIlesDirectory.png" alt="CMake Komutu Sonucunda Oluşan Dosyalar" style="width:80%; height:80%;"/>
</div>

Oluşan dosyaları incelediğimizde derleyici ve işletim sistemine uygun olarak belirtilen direktiflere sahip nesneler, geçici/ara dosyalar ve derleme işlemini gerçekleştireceğimiz **Makefile** dosyasının elde edildiğini görebiliriz. Bu dosyalar aracılığıyla "make" komutunu kullanarak projeyi derleyebilir ve çalışabilir hale getirebiliriz.

    make

<div align="center">
    <img src="../images/CMakeTemelKullanim/makeCommand.png" alt="Make Komutu" style="width:80%; height:80%;"/>
</div>

Derleme işlemi tamamlandığında **CMakeLists.txt** dosyasında belirtilen dizin içerisinde "CMakeLearn" yürütülebilir dosyası oluştu ve çalıştırdığımızda ise "Hello World!" çıktısını elde etmiş olduk!

## CMake Kullanarak Farklı Derleyiciler İçin Gerekli Dosyaları Oluşturmak

CMake'in bizlere sağladığı kolaylığı daha iyi anlamak için aynı işlemleri Clang derleyicisine uygun dosya ve içerikleri oluşturmak için gerçekleştireceğiz. **Bu işlem için Clang derleyicisine sahip olmanız gerekir.**

    sudo apt-get install clang

İlk olarak build dizinin temizleyelim. Ardından yeniden Makefile dosyasını oluşturmak için CMake'i kullanalım.

    cd build/
    rm -rf *
    cmake -DCMAKE_C_COMPILER=clang ..

<div align="center">
    <img src="../images/CMakeTemelKullanim/ClangBuild.png" alt="CMake Komutuyla Clang Derleme Dosyalarının Oluşturulması" style="width:80%; height:80%;"/>
</div>

Cmake komutunun çıktısının incelediğimizde ilk kullanımımızdan farklı olarak compiler olarak Clang 14.0.0 sürümünün, "/usr/bin/clang" dizininden seçildiğini görebilirsiniz. Şimdi make komutunu çalıştırarak, yürütülebilir olarak oluşacak olan "CMakeLearn" dosyamızı elde edelim. Ardından dosyamızı çalıştırarak "Hello, World!" çıktısını alalım.

    make

<div align="center">
    <img src="../images/CMakeTemelKullanim/ClanCompile.png" alt="Clang İçin Make Komutu" style="width:80%; height:80%;"/>
</div>


Farklı derleyiciler için işlem yapmak istediğinizde komut satırı üzerinden parametre geçmek zorunda değilsiniz. Bunun yerine "CMakeLists.txt" dosyasını düzenleyebilir veya terminal üzerinde "CMAKE_GENERATOR" parametresini set edebilirsiniz. Clang için örnek düzenlemeye [bu bağlantıdan](https://stackoverflow.com/questions/7031126/switching-between-gcc-and-clang-llvm-using-cmake) ulaşabilirsiniz.

# CMake Kullanarak Farklı Yapı Sistemi Kurallarına Uygun Dosyaları Oluşturmak

Şu ana kadar kullandığımız tüm örneklerde make komutuna uygun derleme dosyaları oluşturarak kullandık, buna alternatif olarak gömülü vb. projelerde kullanılan "Ninja" için derleme dosyalarını da CMake ile oluşturabilirz.

Bu işlem için Ninja'nın sisteminizde kurulu olması gerekir.

    sudo apt-get install ninja-build

İlk olarak build dizinini temizleyelim ve yeniden dosyaları oluşturmak için CMake'i kullanalım.

    cd build/
    rm -rf *
    cmake -GNinja ..

<div align="center">
    <img src="../images/CMakeTemelKullanim/NinjaBuild.png" alt="Ninja İçin CMake Komutu" style="width:80%; height:80%;"/>
</div>

Oluşan dosya çıktılarını incelediğimizde yukarıda yer alan çıktılardan farklı olarak ninja için belirlenmiş dosyalarında oluştuğunu görebiliyoruz. Derleme işlemini ninja komutuyla gerçekleştirelim ve yürütülebilir dosyayı çalıştırarak "Hello, World!" çıktısına alalım.

    ninja

<div align="center">
    <img src="../images/CMakeTemelKullanim/NinjaCompile.png" alt="Ninja İçin Compile Komutu" style="width:80%; height:80%;"/>
</div>

# Not

Derleme işlemlerinde make ve ninja kullanacağımızın bilgisine sahiptik. Fakat dışarıdan bilmediğiniz bir projeyi derlemek için aşağıdaki yöntemle derleme yapabilirsiniz. Elbette bu işlemi yapabilmeniz için **"cmake .."** komutunu kullanarak gerekli dosyaları oluşturmuş olmanız gerekiyor.

    cd build/
    cmake --build .

Örnekler içerisinde birçok kez **rm -rf * komutu** kullanılıyor. Fakat make ve ninja için **help** aracılığıyla gerekli komutları öğrenerek **cmake --build .** ile birlikte kullanabilirsiniz. Örneğin Unix System Files ve make kullanacaksınız;

    cmake --build . --target clean
    cmake --build . --target all

# Kaynakça

* ChatGPT 3.5'ten yardım alınmıştır.
* [CMake 2.8.12 Documentation](https://cmake.org/cmake/help/v2.8.12/cmake.html)
* [How to CMake Good - 0b - Running CMake](https://www.youtube.com/watch?v=lI2nwZSMvlE)
* [CMake Tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)
* [Switching between GCC and Clang/LLVM using CMake](https://stackoverflow.com/questions/7031126/switching-between-gcc-and-clang-llvm-using-cmake)
* [How to Use Ninja](https://gist.github.com/jrhemstad/811d1e41af9685670241fa472170ef32)
* [What is Ninja vs CMake](https://www.quora.com/What-is-Ninja-vs-CMake)
* [Difference between invoking `ninja` directly vs through `cmake --build`](https://stackoverflow.com/questions/70855120/difference-between-invoking-ninja-directly-vs-through-cmake-build)
* [Can you help me understand what the difference of Make, CMake, and Ninja?](https://www.reddit.com/r/embedded/comments/sbhg3q/can_you_help_me_understand_what_the_difference_of/)
* [ENCSS - CMake hands-on workshop](https://enccs.github.io/cmake-workshop/)
* [Cmake Temel (Zero to Level0.1)](https://www.youtube.com/watch?v=4rHP7FGYpMY)
* [Learn CMake's Scripting Language in 15 Minutes](https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/)

<div align="center">
    <a href="CMakeHakkindaGenelBilgiler.md"> < Önceki Sayfaya Dön</a>
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 
    <a href="CMakeKutuphaneEklemek.md"> Sonraki Sayfaya İlerle ></a>
</div>
