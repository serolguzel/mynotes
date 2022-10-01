# **C# Notları**

### **C# Static Olmak, Non-Static Olmak**

"Static" sıkça karşılaştığımız bir anahtar sözcüktür(keyword). C#'da kullandığımız bütün sınıflar(class), ve üyeleri(metot, field, property, delegate, event, constructor) için olmazsa olmaz bir kavramdır. Bir sınıf veya üye tanımlanırken static anahtar sözcüğü yazılırsa, o sınıfa veya üyeye, static'tir deriz. Eğer yazılmamışsa non-static'tir deriz. (Burada constant; -diğer bir adı ile değişmezler veya sabitler- istisnai bir duruma sahiptir. Constant üyelere static anahtar sözcüğünü yazamayız. Çünkü default olarak static tanımlanırlar. Tanımlarken static yazmaya zorlarsak derleme zamanı hatası alırız.)

**Bir Sınıfın Static Olması**

Bir sınıf static olarak tanımlanıyorsa, içerisindeki bütün üyeler static olarak tanımlanmak zorundadır. Amaç tüm üyeleri static olmaya zorlamaktır. Static sınıfların özellikleri aşağıdaki gibidir:

* C# 2.0 ile birlikte static anahtar sözcüğü class'lara da uygulanabilir bir hale geldi.
* Bir class'a static anahtar sözcüğü uygulandığından içindeki tüm üyeleri static olmaya zorlamış oluruz...
* Tek bir özel nesne örneği, sınıfın kullanıldığı anda ram'in Static bölgesine çıkar...
* Static sınıflar new ile örneklenemezler. Dolayısıyla heap'te nesne örnekleri bulunmaz.

**Üyelerin Static Olması**

C#’da bir sınıfa ait üyeler; metotlar, alanlar(field), özellikler(property), olaylar(event), delegeler(delegate), constructor(yapıcı) metot olarak sıralanabilir. Bu makalede metotlar ve alanlar üzerinde duracağız. Bir üyenin static olması demek; o üyeye erişimin, SınıfAdi.UyeAdi olarak yapılabilmesi anlamına gelmektedir. Bir üyenin non-static olması demek; o üyeye ancak ve ancak, tanımlandığı sınıfa ait bir nesne örneği üzerinden erişebilmemiz anlamına gelir.

> **ÖNEMLİ NOT:** Static, bir erişim belirleyici değildir!

Bir üye neden static olmalıdır? Aynı şekilde, bir üye neden non-static olmalıdır? Şimdi bu sorulara örnek seneryolar aracılığı ile cevap bulmaya çalışalım.

**Static Field(Alan)**

Öncelikle, yeni bir console uygulaması açalım. Daha sonra uygulamaya, adı “Urun” olan yeni bir sınıf ekleyelim. Bu sıfın içerisinde de Id, Ad ve Fiyat bilgilerini tutalım. Seneryomuza göre, oluşan **Product** nesneleri için **Tax** bilgisini de tutmak istiyoruz. Dolayısıyla oluşturduğumuz sınıfa **Tax** alanını da ekleyelim. Benim oluşturduğum Urun sınıfının kodları aşağıdaki gibidir;

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public double Price { get; set; }
    public double Tax { get; set; }
}
```

### Linkler ve Referanslar

[markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)

[Markdown Theme Kit](https://marketplace.visualstudio.com/items?itemName=ms-vscode.Theme-MarkdownKit)

[Markdown TOC](https://marketplace.visualstudio.com/items?itemName=AlanWalk.markdown-toc)

## **Override (Geçersiz Kılma)**

Override methodlar nesne yönelim mimarisinin olmazsa olmazlarından biridir ve haliyle virtual methodlarda tabi ki. Method ezmeye yarayan override yapısı sayesinde miras aldığımız yani base sınıfımız olan bir sınıftaki methodun eksikliğini giderebiliriz.
Override konusunu iyi bir şekilde kavramak için iki sınıf düşünelim. 1. Sınıfımız Person sınıfı olacak ve name ve surname üye değişkenlerine sahip olacak. Aynı zamanda bu sınıfımızda ToString() gibi bir methodumuz olacak ve bu method o kişinin bilgilerini yeniden formatlayacak.
2.Sınıfımız ise Student sinifi olup Person sinifindan miras alacaktır. Farklı olarak StudentNo üye değişkenine sahip olan bu sınıfımız için ise override bir method yazacağız. Ama öncelikle yazmamamız durumunda ne ile karşılaşacağımızı bir incelemek istiyorum. Şimdi sınıflarımızı görelim,

```csharp
public class Person
{
    public string Name { get; set; }
    public string Surname { get; set; }
    public virtual string ToString()
    {
        return string.Format("{0} {1}", Name, Surname);
    }
}
```

Şimdi ise Person sınıfından miras alacak olan Student sınıfımızı inceleyelim;

Unit test yazıp bir Person ve Student türetelim ve ekrana yazdırma işlemini gerçekleştirelim:

```csharp
public class Student : Person
{
    public int StudentNo { get; set; }
    public virtual string ToString()
    {
        return string.Format("{0} {1} {2}", Name, Surname, StudentNo);
    }
}

[Fact]
public void Person_Overvide()
{
    var person = new Person { Name = "Name", Surname = "Surname" };
    var student = new Student { Name = "Name", Surname = "Surname", StudentNo = 123 };
    
    Assert.Equal("Name Surname", person.ToString());
    Assert.Equal("Name Surname 123", student.ToString());
}
```

Çıktısı;

`Name Surname`

`Name Surname 123`

## Indexers (İndekleyiciler)

Indexers (indeksleyiciler) yazım biçimi olarak properties (özelliklere) benzemektedirler. Indexers bir nesnenin dizi gibi indekslenmesini sağlar. Tıpkı bir array (dizi) tanımladığımız da bu array içinde hangi indeksteki elemanı görmek istiyorsak çağırdığımız gibi, indexer’lar da Class’ımızın bir array gibi olmasını sağlamaktadır. Örneğin class içindeki 7. Elemanı getir dendiğinde bu elemanı rahatlıkla getirebiliriz. Tanımlanması properties gibi sadece fazladan bir geri dönüş tipi ve köşeli parantezle aldığı parametreyi söylüyoruz. Indexers aynı methods gibi overload edilebilmektedir.

* İndeksleyiciler dizi ile ilişkilendirildikleri için ve dizilerde birden fazla boyuta sahip olabildikleri için indeksleyiciler parametre alabilirler.
* Aynı özellikler gibi hem get() hem de set() yordamlarını içermek zorunda değildir. Sadece get() yordamı olursa bu read-only (sadece okunabilir) , sadece set() yordamı olursa bu write-only (sadece yazılabilir) indeksleyici olur.
* İndeksleyiciler, özelliklerden farklı olarak yeniden yüklenebilir yani overload edilebilir ancak statik olarak tanımlamazlar.

```csharp
public class Indexers
{
    string[] people = new string[] { "Elma", "Armut", "Kavun", "Karpuz", "Çilek" };
    public string this[int indexs]
    {
        get
        {
            return people[indexs];
        }
    }
    
    public int Length
    {
        get
        {
            return people.Length;
        }
    }
```

Generic ornek;

```csharp
public class SampleCollection<T>
{
    // Veri öğelerini depolamak için bir dizi bildirin.
    private T[] arr = new T[100];
    int nextIndex = 0;
    // İstemci kodunun [] gösterimini kullanmasına izin vermek için dizin oluşturucuyu tanımlayın.
    public T this[int i] => arr[i];

    public void Add(T value)
    {
        if (nextIndex >= arr.Length)
            throw new IndexOutOfRangeException($"Koleksiyon sadece {arr.Length} element tutabilir.");
        arr[nextIndex++] = value;
    }
}

[Test]
public void Indexer_Test()
{
    var indexers = new Indexers();
    Console.WriteLine(indexers[2]);

    var persons = new SampleCollection<Person>();
    persons.Add(new Person { Name = "Name" });
}

public class Person
{
    public string Name { get; set; }
}
```

### Hashtable

İçerisinde key-value çiftlerini barındıran, verilen anahtar kelimeye göre değerini geri döndüren collection tipi. İsminden de anlaşıldığı gibi anahtar kelimeye göre değeri döndürmek için hashing kullanılır.
Hashtable'in güzelliği; eklediğiniz anahtar kelimeyle ilişkili değeri eliyle koymuş gibi (hızlı) bulabilmesidir. Hashtable'da amaç da budur zaten; 0, 1, 2,.. gibi anlamsız indislerle veriye erişmek yerine veriye bir isim (anahtar kelime; key) verip o isimle erişmektir. Böylece sayısal indis kullanıyormuşçasına hızlı ve fakat daha anlaşılır indisler ile verinize erişirsiniz.

Sonuç itibariyle siz her ne kadar anahtar kelime de kullansanız, verileriniz bir dizi üzerinde tutulur ve sayısal indis ile erişilir. Sizin belirttiğiniz anahtar kelime hashing algoritmaları yardımıyla sayısal indekse dönüştürülür. Olayın karanlık iç yüzü budur. Bu konu biraz akademik seviyede olduğu için bu konuda atıp tutmayı ayıp sayarım.

Hashing de en onemli sey key generatordır. oyle bir key generator yapılmali ki collision olasılığı az olmalıdır. Ornek: basit bir hashing için key olarak verilen karakterlerin ascii karsiliklarinin sayisal degerlerini toplamak ve bunu da hashing icin onceden belirledigimiz toplam kayıt sayisina gore modunu almaktir. eger elde edilen degere baska bir kayit daha once atanmissa kayit overflow alanina eklenir.
.Net'te serialize edilemeyen veri tipi. İlerde serialize edebileceğiniz sınıfları tasarlarken uzak durmak gerekiyor.
