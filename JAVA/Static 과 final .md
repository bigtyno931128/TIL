### **< Static  >**

먼저 Static 이란 “ 고정된 “ 이라는 의미 입니다 . 

—> 객체 생성 없이 사용할 수 있는 필드와 메소드를 생성하고자 할때 활용되고 있습니다 .

**공용 데이터에 해당하거나 인스턴스 필드를 포함하지 않는 메소드를 선언하고자 할때 사용되고 있습니다 .**  

반대로 필드나 메소드를 객체마다 다르게 가져야 한다면 인스턴스로 사용합니다.

---

ex ) 라면을 예로 들어보자면 

라면의 경우 스프와 면 자체는 공용할 수 있는 필드로 생각하면 될 것 같습니다 .

하지만 이름의 경우는 삼양라면, 너구리 , 짜파게티등의 여러 라면이 존재하기에 필드로 생성하면 될 것 같습니다 .

```java
public class Ramen() {
	
	static String soup = "스푸";
	static String noodle = "면";
	
	String name = "삼양라면";
}
```

// 위 와 같이 라면 클래스를 설계할 경우 

```java
public class Main {

    public static void main(String[] args) {
        String noodle = Ramen.noodle;
        String soup = Ramen.soup;
        System.out.println("noodle = " + noodle);
        System.out.println("soup = " + soup);
    }
}
```

// 인스턴스를 생성하지 않고도 바로 필드를 사용할 수 있습니다 . 
// 하지만 name 의 경우 static 필드가 아니기에 위와 같은 방법으로는 
// 사용할 수 없으며 인스턴스화를 해줘야 사용할 수있습니다.

```java
public class Main {

    public static void main(String[] args) {

        Ramen ramen = new Ramen();
        System.out.println("noodle = " + ramen.noodle);
        System.out.println("soup = " + ramen.soup);

        System.out.println("name = " + ramen.name);

    }
}
```

// 위 코드와 같이 name 을 사용하기 위해서는 인스턴스화를 진행해 주어야하며 , 누드와 스프에 경우에도
// 같은 방법을 사용할 수 있지만 추천하고 있지는 않습니다 .

---

### **< final  >**

final 은 최종적인 이라는 의미로 써 ,  상수 . 변하지 않는 최종값에 사용을 한다.  이는 수정이 불가능하다 .

```java
public class Final {

    final String soup = "스프";
    final String ramen;

    public Final(String ramen) {
        this.ramen = ramen;
    }

}
```

선언 방법에는 위 코드와 같이 선언과 동시에 값을 할당하는 방법 .

선언을 한 뒤 생성자를 값을 받는 방법이 있다 . 

---

### **< Static final >**

그렇다면 static 과 final 을 동시에 사용하는 것은 무엇일까 ?  

위에 static 과 final 에 의미대로라면 “고정된  + 최종적인” 의  의미를 가질 수 있다 . 

=== > 상수에 사용 . 

저의 경우에는 프로젝트 당시에 파일 경로등에 자주 사용했던것 같습니다 .
