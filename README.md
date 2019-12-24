
##  *Bridge Tasarım Deseni*
Bridge design pattern (köprü); structural grubuna ait nesnelerin modelleme ve uygulanmasını ayrı sınıf hiyerarşilerinde tanımlanmasını düzenleyerek daha esnek bir yapıda olmasını sağlar.
Bridge tasarım deseninde soyutlama ve gerçekleştirme için üst sınıf bulunur. Yani kalıplar ve bu kalıplar üzerinde işlem yapan gerçek işe yarayan sınıflar ayrı olarak tasarlanır. Bridge tasarım deseni de bu yapılar arasında köprü görevi üstlenmiş olur. Böylece kodun genişlemesi kolaylaşır.
## UML Diyagramı
![enter image description here](http://harunozer.com/image/mr/bridge_uml.png)

## Temel olarak 5 yapı bulunmaktadır.

 1. Client
 2. Abstraction
 3. Refined Abstraction
 4. Concrete Implementor
 5. Implementor

Implemantor arayüzü ile operasyonlar tanımlanır ve ConcreteImplemantorlar bu arayüzden türeyerek operasyonları gerçekleştirir. Abstraction abstract sınıfı ise içinde Implemantor arayüzünden referans barındırarak Implemantordaki operasyonları çalıştırır. RefinedAbstraction ise Abstraction u uygulayan gerçek sınıf veya senaryoya göre sınıflardır. Client ise Abstraction ve Implemantor türlerinden nesneleri üreterek yapıyı kullanır.
## Örnek
Bridge tasarım deseni ile ilgili basit bir örnek uygulama. Uygulamada veri tabanı yapısını ele alalım. Uygulama birden fazla veri tabanı desteği veriyor olsun ve execute, connection açma gibi operasyonlar her veri tabanı için farklı olsun. Uygulamanın class diyagramı;
![enter image description here](http://harunozer.com/image/mr/BridgeClassDiagram.png)

## Code
    //Implementor  
    abstract class DbImplementor  
    {  
	    public abstract void Execute(string Sql);  
	    public abstract void OpenCon(string SqlCon);  
    }

    //ConcreteImplementor  
    class SqlServerImplementor : DbImplementor  
    {  
	    public override void Execute(string Sql)  
	    {  
	    Console.WriteLine("\"{0}\" - SqlServer işletildi.", Sql);  
	    }
    
	    public override void OpenCon(string SqlCon)  
	    {  
	    Console.WriteLine("\"{0}\" - Sql Server Con. Açıldı.", SqlCon);  
	    }  
    }
    
    //ConcreteImplementor  
    class OracleImplementor : DbImplementor  
	    {  
	    public override void Execute(string Sql)  
	    {  
	    Console.WriteLine("\"{0}\" - oracle işletildi.", Sql);  
	    }
    
	    public override void OpenCon(string SqlCon)  
	    {  
	    Console.WriteLine("\"{0}\" - oracle Con. Açıldı.", SqlCon);  
	    }  
    }
    
    //Abstraction  
    abstract class DbAbstraction  
    {  
    protected DbImplementor implementor;
    
    public DbAbstraction(DbImplementor imp)  
    {  
    Implementor = imp;  
    }
    
    // Property  
    public DbImplementor Implementor  
    {  
    set { implementor = value; }  
    }
    public abstract void Exec(string Sql);  
    public abstract void ConOpen(string ConStr);  
    }
    //RefinedAbstraction  
    class DbRefinedAbstraction : DbAbstraction  
    {  
	    public DbRefinedAbstraction(DbImplementor imp) : base(imp)  
	    {
	    }  
	    public override void Exec(string Sql)  
	    {  
	    implementor.Execute(Sql);  
	    }
    
	    public override void ConOpen(string ConStr)  
	    {  
	    implementor.OpenCon(ConStr);  
	    }  
    }
    //client  
    class Program  
    {  
	    static void Main(string[] args)  
	    {  
	    DbAbstraction absDb = new DbRefinedAbstraction(new SqlServerImplementor());  
	    absDb.ConOpen("e-ticaret db");  
	    absDb.Exec("select * from Urun");
	    absDb = new DbRefinedAbstraction(new OracleImplementor());  
	    absDb.ConOpen("e-ticaret db");  
	    absDb.Exec("select * from Urun");
	    Console.ReadKey();  
	    }  
    }
