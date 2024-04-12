<div align="center">
    <h1>CMake'in Betik Dili</h1>
    <img src="../images/CMakeHakkindaGenelBilgiler/CMake-Logo.svg" alt="CMake Resmi Logosu" style="width:70%; height:70%;"/>
    <p><a href="https://cmake.org/wp-content/uploads/2023/08/CMake-Logo.svg">SVG Kaynak Bağlantısı</a></p>
    
</div>

## Betik Dili

**CMakeLists.txt** dosyasında dikkate alınması gereken bazı tüyolar;
- Yazılan komutlar tek satırda ele alınır.
- Parametre olarak geçilen değerler bir dizi olarak kabul edilir. Boşluk içeren değerlerde tırnak kullanılmaldır.
- Değişkenlerinizi atama operatörleriyle değil, set fonksiyonuyla belirtin.

        set(name "Kağan Can Şit")

- Değişken değerlerinizi yazdırırken ${} yapısını kullanarak değişken adını içeriğe ekleyin. 

        message("Merhaba, ben ${name}!")

CMake'in betik dili için nitelikli olarak [bağlantıda](https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/) yer alan kaynağı inceleyebilirsiniz.


# Kaynakça

* ChatGPT 3.5'ten yararlanılmıştır.
* [CMake 2.8.12 Documentation](https://cmake.org/cmake/help/v2.8.12/cmake.html)
* [CMake Tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

<div align="center">
    <a href="ProjeyiAltDizinlereAyirmakVeYonetmek.md.md"> < Ana Sayfaya Dön</a>
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 
    <a href="#"> Sonraki Sayfaya İlerle ></a>
</div>