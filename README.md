# Abstrack factory(fabrika) tasarım deseni 

Abstrack factory(fabrika) tasarım deseni creational grubuna ait bir tasarım desenidir. Eğer nesnelerin aralarında ilişki varsa  nesneler için somut sınıflar oluşturmadan arayüzler oluşturmaya imkan sağlar.İf else yapısından kurtulmanızı sağlayarak daha sade bir kod yazmanızı sağlar. Bu tasarım desenin oluşturmak için en az türü abstarct, interface ve normal sınır olan bir süper class, en az bir alt sınıf , bir tane  süper abstract class, bir tane üretici ve test sınıfı olmak zorundadır. Gelin bu tasarım desenini bir senaryo üzerinden inceleyelim


![Image of Class](https://raw.githubusercontent.com/caglarozcan/Desing-Patterns/master/DependencyInjection/DependencyInjection/Component/DependencyInjectionClassDiagram.png)

Yukarıda gördüğünüz diagramda ki senaryo yeni bir pc oluşturulmak istendiğini düşünülerek  kurulmuştur fakat tüm yapıyı anlatmak için bütün bilgisayar bileşenleri bir bir  örneklemek yerine Ram ve Hdd  bileşenleri üzerinden  anlatılmıştır. Yeni bir bilgisayar oluşturulmak istendiğinde PcFactory arayüzü üzerinden oluşturulmak istenen bileşenin sınıfına erişiliyor. Bizim örneğimizde bu bileşenler ram ve hdd dir. Ve bu bileşenlerin  diagram üzerinde  kendi içerisinde  iki farklı türü bulunmaktadır. Yani bir bilgisayar  nesnesi oluşturulmak istendiğinde kullanılabilecek ram türü iki tanedir.


RamAbstrack ve HddAbstrack class kodları 

```csharp
public abstract class HddAbstract
{
    public abstract void HddIslem();
    public abstract string HddTur { get; }
}
public abstract class RamAbstract
{
    public abstract void RamIslem();
    public abstract string RamTur { get; }
}


```
Şu  ana RamAbstrack  classını oluşturduğumuz için PcFactory üretici sınıf üzerinden ram türlerini oluşturabiliriz . 

RamAbstrack classından türetilmiş RamConcrete1 ve RamConcrete2 kodları


```public class RamConcrete1 : RamAbstract
{
    public override void RamIslem()
    {
        Console.WriteLine("RamConcrete1 birleştirildi");
    }
    public override string RamTur
    {
        get { return "Bu ram türü: RamConcrete1"; }
    }
}
public class RamConcrete2 : RamAbstract
{
    public override void RamIslem()
    {
        Console.WriteLine("RamConcrete2 birleştirildi");
    }
    public override string RamTur
    {
        get { return "Bu ram türü: RamConcrete2"; }
    }
}

```
Yukarıda anlattığım  durum Hdd içinde geçerlidir.

HddAbstrack classından türetilmiş HddConcrete1 ve HddConcrete2 kodları
```csharp
public class HddConcrete1:HddAbstract
{
    public override void HddIslem()
    {
        Console.WriteLine("HddConcrete1 birleştirildi.");
    }
    public override string HddTur
    {
        get { return "Bu hdd türü: HddConcrete1"; }
    }
}
public class HddConcrete2 : HddAbstract
{
    public override void HddIslem()
    {
        Console.WriteLine("HddConcrete2 birleştirildi");
    }
    public override string HddTur
    {
        get { return "Bu hdd türü: HddConcrete2"; }
    }
}

```
RamAbstract ve HddAbstract sınıflarını oluşturup bunlar üzerinden türetilmiş  her sınıf için iki tane toplamda dört  türde oluşturduğumuza göre artık bunlara hükmedebileceğimiz bir üretici sınıfı oluşturmamızın zamanı geldi. Yani şunu söylemek istiyorum artık yeni bir ram veya hdd üretmak istediğimiz zaman ;örneğin bu üreteceğimiz ram olsun. Bunun türünün RamConcrete1 mi yoksa RamConrete2 mi olacağını belirtmemiz yeterli olacak ve üretici sınıfımız onu üretecektir.
PsFactory kodları
```csharp
public class Factory
{
    private PcFactory _pcFactory;
    private HddAbstract _hddAbstract;
    private RamAbstract _ramAbstract;
    public Factory(PcFactory pcFactory)
    {
        _pcFactory = pcFactory;
        _hddAbstract = _pcFactory.CreateHdd();
        _ramAbstract = _pcFactory.CreateRam();
    }
    public void Birlestir()
    {
        Console.WriteLine(_hddAbstract.HddTur);
        _hddAbstract.HddIslem();
        Console.WriteLine(_ramAbstract.RamTur);
        _ramAbstract.RamIslem();
    }
}

```
Şimdi de oluşturduğumuz bu yapıyı kullanalım.
Factory kodları
```csharp
classProgram
{
    static void Main(string[] args)
    {
        Factory f = new Factory(new Concrete1Factory());
        f.Birlestir();
        Console.WriteLine();
        f = new Factory(new Concrete2Factory());
        f.Birlestir();
        Console.ReadKey();
    }
}
 
