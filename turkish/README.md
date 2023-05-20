## İçindekiler

- [Başlangıç](#başlangıç)
- [Ön Hazırlık](#ön-hazırlık)
  - [Selam, map!](#selam-map)
  - [Anonim Fonksiyonlar (i.e. lambda)](#)
- [Bölüm 1: Sayıları Ayrıştırma](#)
- [Bölüm 2: Zor Kısım](#)
  - [Özyinelemeli bir fonksiyonun anatomisi](#)
- [Bölüm 3: Daha Kolay Bölüm](#)

Bu belge, yeni başlayanlar için fonksiyonel programlama paradigmasına _yumuşak_ bir giriş niteliğindedir. Bazı zorlu kodlar ile karşılaşacaksınız, kemerinizi bağlayın ve sıkı durun.

Az önce de söylediğim gibi, zor kodlarla karşılaşacaksınız. Lütfen sıradan bir programcının farklı, yabancı bir paradigmada bir kod gördüğünde yapacağı şeyi yapmayın; çığlık atıp kaçmak. Kod ne kadar zor görünürse görünsün, burada kalın ve anlamaya çalışın. Kendinize bir kağıt ve kalem alın. Fonksiyonun çalışma şeklini çizmeye çalışın. İnanın bana, bu harika bir alıştırma olacak.

Eğer her şey yolunda gider ve bu dokümanı bahsettiğim her şeyi anlayarak bitirirseniz, Tamamen Fonksiyonel Programlama Dilleri yolculuğunuz için hazır olacaksınız, yani Haskell!

Başlamadan önce, lütfen yeterli seviyede Python bilgisine sahip olduğunuzdan emin olun.

Bu tip dokumanlarin basinda insanlar genellikle fibonacci veya bunun gibi eski moda saçmalıklar gösterirler ama hadi başka bir şey deneyelim - basit bir hesap makinesi programlıyoruz. İşte soyle görünecek:

```
$ python program.py
Lutfen sayilari girin: 1 34 22
Lutfen yapmak istediginiz islemi secin [*, /, +, -]: +

Sonuc: 57
```

Kolay duruyor, değil mi? Şimdi neden bunu Python'da, fonksiyonel programlama veya başka bir şey olmadan yapmaya çalışmıyorsunuz? Basitçe normalde yapacağınız şeyi yapın. Çalışan bir şey oluşturun ve **Fonksiyonel Programlama** kullanarak nasıl programlayacağınızı görmek için buraya geri gelin. (Yapmak zorunda değilsiniz, isterseniz devam etmekten çekinmeyin).

# Başlangıç

Kullanıcıdan girdi almakla başlayalım:

```python
def main():
    numbers   = input("Lutfen sayilari girin: ")
    operator  = input("Lutfen yapmak istediginiz islemi secin [*, /, +, -]: ")

    result = solution(numbers, operator)
    print(result)

def solution(numbers, operator):
    ...

# Bu kalıptan haberdar değilseniz, lütfen şuna bir göz atın:
# https://realpython.com/if-name-main-python/
if __name__ == "__main__":
    main()
```

Sade. Programı çalıştıran bir ``main`` fonksiyonu oluşturduk. Ayrıca sayıları ve operatörü alan ve sonucu hesaplayan bir ``solution`` fonksiyonu oluşturduk. Basitçe şu şekilde de yazabilirdik:

```python
numbers   = input("Lutfen sayilari girin: ")
operator  = input("Lutfen yapmak istediginiz islemi secin [*, /, +, -]: ")

# Burada sonucu hesapliyoruz...

print(result)
```

İşte iki ayrı fonksiyonlar kullanmanın bazı faydaları:

- ``__name__ == "__main__"`` kalıbını kullandık, böylece Python dosyasını içe aktarmak programımızın çalışmasına neden olmayacak. Bunun yerine, ``main`` ve ``solution`` fonksiyonlarına erişimimiz olacak, ancak biz onları çalıştırmadıkça çalışmayacaklar. İsterseniz ``__name__ == "__main__"`` kalıbını test edebilirsiniz.
- ``solution`` ve ``main`` fonksiyonlarını ayırmak, ``solution`` fonksiyonunu başka bir Python programında kullanmamızı kolaylastirir. Örneğin, tkinter ile bir kullanıcı arayüzü oluşturursak ve arayuzden sayıları ve operatörü alırsak, ``solution`` üzerinde herhangi bir değişiklik yapmak zorunda kalmayacağız. Basitçe içe aktarabilir ve kullanabiliriz. Girdiyi kullanıcı arayüzünden alacağız ve doğrudan ``solution``a vereceğiz.

Bu bizim kodumuzun temelidir. Şu andan itibaren ``solution`` üzerinde çalışacağız.

Sayıları ayrıştırmakla başlayacağız. Bu ``input``tan elde ettiğimiz şeydir:

```
"1 34 22"
```

Ancak istedigimiz sey değil, sayılara bu biçimde ihtiyacımız var:

```
[1, 34, 22]
```

Bu yüzden ilk yapacağımız şey bu olacak. Ancak öncesinde fonksiyonel programlamanın bazı temel kavramlarını bildiğinizden emin olmamız gerekiyor. Eğer zaten çok iyi biliyorsanız bunları atlayabilirsiniz.

## Ön Hazırlık

### Selam, map!

Teknik olarak konuşmak gerekirse, ``map`` bir [yüksek dereceli fonksiyon](https://en.wikipedia.org/wiki/Higher-order_function)'dur. Oldukça basittir. Bir koleksiyonun (örneğin liste, string) her bir elemanına belirli bir fonksiyon uygular.

İşte yaptığı şey:

```python
mynumbers = [1, 2, 3]

def multiply_with_2(number): # Aldigi sayiyi ikiyle carpan bir fonksiyon
    return number * 2

print(
    list(map(multiply_with_2, mynumbers))
)
# 2, 4, 6
```

Map basitce budur. ``map`` bir fonksiyon ve bir "koleksiyon" alır, fonksiyonu her bir elemana uygular ve uygulanan koleksiyonu döndürür.

Python`da ``map`` bir ``map`` nesnesi döndürdüğü için ``map`` nesnesini bir listeye dönüştürmek zorundaydık. Bu, koleksiyonu herhangi bir biçimde, str, tuple veya liste olarak almamızı sağlar.

Devam etmeden önce, lütfen ``map``'in programlamada yaygın bir konsept olduğunu unutmayın. Sadece Python'da degil, programlama dillerinin çoğunda mevcuttur.

### Anonim Fonksiyonlar (i.e. lambda)

Iste bir ornek:

```python
def multiply_with_2(number):
    return number * 2
```

Bu çok uzun, 2 satır. Bu kadar basit bir şeyi, neden sadece bir satırda yazmıyoruz?

```python
multiply_with_2 = lambda number: number * 2
```

``lambda`` ifadesi anonim olarak (bir isme sahip olmadan) çalışabilir, hadi bunu yukarıdaki kodla birleştirelim.

```python
mynumbers = [1, 2, 3]

print(
    list(map(lambda number: number * 2, mynumbers))
)
# 2, 4, 6
```

Sayı listesi için de bir değişkene ihtiyacımız yok:

```python
print(
    list(map(lambda number: number * 2, [1, 2, 3]))
)
```

Tanım, matematikteki fonksiyonlara oldukça yakındır:

```
lambda n: n + 2

f(x) = x + 2
```
