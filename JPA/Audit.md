## JPA 에서의 Audit

---

JPA 를 사용하여 데이터베이스와의 테이블에 객체를 매핑할 때 공통적으로 사용되는 필드와 컬럼들이 존재 한다 . ( EX : 생서일자 , 수정일자 , 식별자 와 같은 필드 ) 

우리가 실제 운영하는 서비스에서는 테이블 상에 **누가 , 언제** 와 같은 기록을 잘 남겨 두어야 한다 . 때문에 공통적으로 자주 사용되는 이 부분에서 JPA 에서는 **Audit** 이라는 기능을 제공 하고 있다 . 



### JPA 에서의 Audit 기능을 사용할 시에는

도메인을 영속성 컨텍스트에 저장하거나 조회를 수행한 후에 update를 하는 경우 매번 시간 데이터를 입력하여 주어야 하는데, audit을 이용하면 자동으로 시간을 매핑하여 데이터베이스의 테이블에 넣어주게 됩니다.

기능 사용을 위해서는 의존성을 추가해주어야 하며 .

```java
dependencies {
compile('org.springframework.boot:spring-boot-starter-web')
compile('org.projectlombok:lombok')
compile('org.springframework.boot:spring-boot-starter-data-jpa')
}
```

@EnableJpaAuditing 추가해주어야 한다 . 

```java
@EnableJpaAuditing
@SpringBootApplication
public class BigtynoApplication {

    public static void main(String[] args) {
        SpringApplication.run(BigtynoApplication.class, args);
    }

}
```

### **@EntityListeners**

우선 Auditing을 적용할 엔티티 클래스에 **`@EntityListeners`** 어노테이션을 적용해야한다. 해당 어노테이션은 엔티티의 변화를 감지하여 엔티티와 매핑된 테이블의 데이터를 조작한다.

이 어노테이션의 파라미터에 이벤트 리스너를 넣어줘야하는데, 여기에 **`AuditingEntityListener`** 클래스를 넣어준다. 이 클래스는 Spring Data JPA에서 제공하는 이벤트 리스너로 엔티티의 영속, 수정 이벤트를 감지하는 역할을 한다.

### **@CreateDate**

생성일을 기록하기 위해 **`LocalDateTime`** 타입의 필드에 **`@CreateDate`** 를 적용한다. 또한 생성일자는 수정되어서는 안되므로 **`@Column(updatable = false)`** 를 적용한다. 이렇게 적용하면, 엔티티가 생성됨을 감지하고 그 시점을 **`createdAt`** 필드에 기록한다.

### **@LastModifiedDate**

수정일을 기록하기 위해 **`LocalDateTime`** 타입의 필드에 **`@LastModifiedDate`** 를 적용한다. 이렇게 적용하면, 엔티티가 수정됨을 감지하고 그 시점을 **`updatedAt`** 필드에 기록한다.

사실 Auditing 적용은 이것이 끝이다. 직접 엔티티를 생성하고, 조회해보면 **`createAt`**과 **`updatedAt`**에 시각이 잘 들어가는 것을 확인할 수 있다.

# **BaseEntity로 분리하기**

사실 생성시각과 수정시각은 대부분의 엔티티에서 사용되는 필드일 것이다. 따라서 이를 별개의 엔티티 클래스로 분리하고, 다른 엔티티에서 상속을 받아 사용하면 많은 중복 코드를 없앨 수 있을 것이다.

```
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class BaseEntity {

    // 파싱 룰 적용
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME)
    @CreatedDate
    @Column(nullable = false)
    private LocalDateTime createAt; //생성일시

    @CreatedBy
    @Column(nullable = false, length = 100, updatable = false)
    private String createBy; // 생성자

    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME)
    @LastModifiedDate
    @Column(nullable = false)
    private LocalDateTime modifiedAt; //수정일시

    @LastModifiedDate @Column(nullable = false, length = 100, updatable = false)
    private String modifiedBy; //수정자

}

```
