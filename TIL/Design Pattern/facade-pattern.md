# Facade Pattern 정리

- 복잡한 클래스와 메서드로 만들어진 기능을 사용자에게 간단하게 보이고 사용할 수 있게 만들어 주는 것
- 복잡한 소프트웨어 바깥쪽의 코드가 라이브러리의 안쪽 코드에 의존하는 일을 감소시켜 주고, 복잡한 소프트웨어를 사용할 수 있게 간단한 인터페이스 제공

예제

Remote_Control.java 

```java
public class Remote_Control {
    
    public void Turn_On()
    {
        System.out.println("TV를 켜다");
    }
    public void Turn_Off()
    {
        System.out.println("TV를 끄다");
    } 
}
```

리모컨을 조작하는 클래스이다.
복잡한 서브 클래스들 중 하나다.



Movie.java 

```
public class Movie {
    
    private String name="";
    
    public Movie(String name)
    {
        this.name = name;
    }
    
    public void Search_Movie()
    {
        System.out.println(name+" 영화를 찾다");
    }
    
    public void Charge_Movie()
    {
        System.out.println("영화를 결제하다");
    }
    public void play_Movie()
    {
        System.out.println("영화 재생");
    } 
}
```

영화 재생과 관련된 클래스이다.
마찬가지로 복잡한 서브 클래스들 중 하나다.



Beverage.java 

```java
public class Beverage {
    
    private String name="";
    
    public Beverage(String name)
    {
        this.name = name;
    }
    
    public void Prepare()
    {
        System.out.println(name+" 음료 준비 완료 ");
    }
}
```

음료를 제공하는 클래스이다.
복잡한 서브 클래스들 중 하나다.



Facade.java

```java
public class Facade {
    
    private String beverage_Name ="";
    private String Movie_Name="";
    
    public Facade(String beverage,String Movie_Name)
    {
        this.beverage_Name=beverage_Name;
        this.Movie_Name=Movie_Name;
    }
    
    public void view_Movie()
    {
        Beverage beverage = new Beverage(beverage_Name);
        Remote_Control remote= new Remote_Control();
        Movie movie = new Movie(Movie_Name);
        
        beverage.Prepare();
        remote.Turn_On();
        movie.Search_Movie();
        movie.Charge_Movie();
        movie.play_Movie();
    }
}
```

다음은 가장 중요한 Facade 클래스이다.
복잡한 서브 클래스들에 대한 인스턴스를 가지며 복잡한 호출 방식에 대하여 view_Movie() 메서드 내에서 구현을 하도록 했다.



Viewer.java

```java
public class Facade {  
    public void view()
    {
        Facade facade = new Facade("콜라","어벤져스");
        facade.view_Movie();
    }
}
```

사용자 입장에서는 이제 서브 클래스에 대해서 알 필요가 없다.
단지 Facde 객체의 view_Movie() 메서드를 호출하면서 서브 클래스들의 복잡한 기능을 수행할 수 있기 때문이다.

---
#### 참고

https://lktprogrammer.tistory.com/42

https://hoony-gunputer.tistory.com/198