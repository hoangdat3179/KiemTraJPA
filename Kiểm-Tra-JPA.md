# Kiểm tra kiến thức kết nối CSDL-JPA

## Câu 1: Thuộc tính name trong annotation `@Entity` khác với thuộc tính name trong `@Table` như thế nào? Hãy giải thích rõ cần thì minh hoạ.

- Thuộc tính name trong annotation `@Entity`: Cấu hình tên entity. Các lệnh JPQL sử dụng tên entity.
- Thuộn tính name trong annotation `@Table`: Cấu hình tên của CSDL. Dùng cho các lệnh native SQL.

---

## Câu 2: Để debug câu lệnh SQL mà Hibernate sẽ sinh ra trong quá trình thực thi, cần phải bổ sung lệnh nào vào file application.properties?

Để debug câu lệnh SQL mà Hibernate sẽ sinh ra trong quá trình thực thi, cần phải bổ sung lệnh

```
spring.jpa.show-sql=true
```

vào file application.properties để hiển thị ra Hibernate khi thực thi.

---

## Câu 3: Khi sử dụng H2, làm thế nào để xem được cơ sở dữ liệu và viết câu truy vấn?

Để xem được CSDL khi sử dụng H2 ta cần thêm thuộc tính dưới đây vào `application.properties`:

```
spring.h2.console.enabled=true
```

Sau đó truy cập vào đường dẫn:

```
http://localhost:8080/h2-console
```

Sau khi đăng nhập vào CSDL H2 ta có thể xem và bắt đầu viết câu truy vấn.

---

## Câu 4: Khi viết mô tả một model, những thuộc tính chúng ta không muốn lưu xuống CSDL thì cần đánh dấu bằng annotation nào?

Khi viết mô tả một model, những thuộc tính chúng ta không muốn lưu xuống CSDL thì cần đánh dấu bằng annotation `@Formula`.

---

## Câu 5: Annotation @Column dùng để bổ sung tính chất cho cột ứng với một thuộc tính. Tham số nào trong @Column sẽ đổi lại tên cột nếu muốn khác với tên thuộc tính, tham số nào chỉ định yêu cầu duy nhất, không được trùng lặp dữ liệu, tham số nào buộc trường không được null?

Các tham số trong @Column:

- Tham số đổi lại tên cột nếu muốn khác với tên thuộc tính: (name)
- Tham số chỉ định yêu cầu duy nhất và không trùng lặp dữ liệu: (unique)
- Tham số buộc trường không được null: (nullable)

---

## Câu 6: Có 2 sự kiện mà JPA có thể bắt được, viết logic bổ sung

### - Ngay trước khi đối tượng Entity lưu xuống CSDL (ngay trước lệnh INSERT)

### - Ngay trước khi đối tượng Entity cập nhật xuống CSDL (ngay trước lệnh UPDATE)

### Vậy 2 annotation này là gì?

- Ngay trước khi đối tượng Entity lưu xuống CSDL (ngay trước lệnh INSERT): `@PrePersist`

- Ngay trước khi đối tượng Entity cập nhật xuống CSDL (ngay trước lệnh UPDATE):`@PreUpdate`

---

## Câu 7: Tổ hợp các trường thông tin địa chỉ: country, city, county, addressline thường luôn đi cùng nhau và sử dụng lại trong các Entity khác nhau. Nhóm 2 annotation nào dùng để tái sử dụng, nhúng một Entity vào một Entity khác?

Nhóm 2 anootation dùng để tái tạo sử dụng, nhúng một Entity vào một Entity khác là `@Embeddable` và `@Embedded`.

---

## Câu 8: JpaRepository là một interface có sẵn trong thư viện JPA, nó cũng cấp các mẫu hàm thuận tiện cho thao tác dữ liệu. Cụ thể JpaRepository kế thừa từ interface nào?

- `JpaRepository` kế thừ từ interface `PagingAndSortingRepository` và `QueryByExampleExecutor`.

- Trong đó `PagingAndSortingRepository` được kế thừa từ `CrudRepository`.

---

## Câu 9: Hãy viết khai báo một interface repository thao tác với một Entity tên là Post, kiểu dữ liệu trường Identity là long, tuân thủ interface JpaRepository.

```java
@Repository
public interface PostRepository extends JpaRepository<Post, Long>{}
```

---

## Câu 10: Khi đã chọn một cột là Identity dùng @Id để đánh dấu, thì có cần phải dùng xác định unique dùng annotation @Column(unique=true) không?

Khi đã chọn một cột là Identity dùng @Id để đánh dấu, thì không cần phải dùng xác định unique dùng annotation @Column(unique=true).

---

## Câu 11: Khác biệt giữa @Id với @NaturalId là gì?

- `@NaturalID` tạo unique constrain lên một trường không phải PrimaryKey
- Dùng cho những dữ liệu bản chất đã là unique mà không cần hệ thống sinh ví dụ như email, di
  động, mã căn cước, ISBN
- `@Id`, primary cần giữ nguyên không đổi, nhưng `@NaturalId` có thể được phép thay đổi, miễn
  đảm bảo duy nhất.

---

## Câu 12: Có những cột không phải primary key (@Id) hay @NaturalId, dữ liệu có thể trùng lặp (unique không đảm bảo true), nhưng cần đánh chỉ mục (index) để tìm kiếm nhanh hơn vậy phải dùng annotation gì? Hãy viết 1 ví dụ sử dụng annotation đó với index cho 1 column và 1 ví dụ với index cho tổ hợp nhiều column.

---

## Câu 13: Annotation @GeneratedValue dùng để chọn cách tự sinh unique id cho primary key phải là trường kiểu int hoặc long. Nếu trường primary key có kiểu là String, chúng ta không thể dùng @GeneratedValue vậy hãy thử liệt kê các cách đảm bảo sinh ra chuỗi có tính duy nhất?

Cách để đảm bảo sinh ra chuỗi có tính duy nhất là tạo ra Custom ID generator sử dụng Random Id bằng cách tạo ra một chuối String ngẫu nhiên.
Ví dụ:

```java
public class RandomIDGenerator implements IdentifierGenerator {
@Override
public Serializable generate(SharedSessionContractImplementor session, Object obj)
throws HibernateException {
RandomString randomString = new RandomString(10);//Sinh chuỗi ngẫu nhiên 10 ký tự
return randomString.nextString();
}
}
```

```java
@Data
@Entity (name="bar")
@Table (name="bar")
public class Bar {
@GenericGenerator (name = "random_id", strategy =
"vn.techmaster.demojpa.model.id.RandomIDGenerator")
@Id @GeneratedValue (generator="random_id")
private String id;
private String name;
}
```

---

## Câu 14: Giả sử có 1 class Employee với các fields sau {id, emailAddress, firstName, lastName}. Hãy viết các method trong interface EmployeeRespository để :

### - Tìm tất cả các Employee theo emailAddress và lastName

### - Tìm tất cả các Employee khác nhau theo firstName hoặc lastName

### - Tìm tất cả các Employee theo lastName và sắp xếp thứ tự theo firstName tăng dần

### - Tìm tất cả các Employee theo fistName không phân biệt hoa thường

```java
@Repository
public interface EmployeeRespository extends JpaRepository<Employee, Long> {
    List<Employee> findByEmailAddressAndLastName(String emailAddress,String lastName);
    List<Employee> findByFirstName(String firstName);
    List<Employee> findByLastName(String lastName);
    List<Employee> findByLastNameOrderByFirstName(String lastName);
    List<Employee> findByFirstNameAllIgnoreCase(String firstName);
}
```

## Câu 15: Hãy nêu cách sử dụng của @NamedQuery và @Query. Cho ví dụ.

- Cách sử dụng @NamedQuery (Về mặt clean code, thì không khuyến cáo sử dụng @NamedQuery)

```java
@Entity (name ="oto") //tên entity sẽ sử dụng trong câu lệnh JPQL
@Table (name = "car") //tên table sẽ sử dụng để lưu xuống bảng vật lý trong CSDL
@Data //annotation của Lombok
@NamedQuery (name = "Car.findById", query = "SELECT c FROM oto c WHERE c.id=:id")
public class Car {
@Id private long id;
private String model;
private String maker;
private int year;
}
```

- Gọi @NamedQuery:
  Thực tế giá trị @NamedQuery mang lại không nhiều. Do đó hãy ưu tiên khai báo query trong Repository

```java
@NamedQuery (name = "Car.findById", query = "SELECT c FROM oto c WHERE c.id=:id")
```

```java
Query namedQuery = em.createNamedQuery("Car.findById");
namedQuery.setParameter("id", 1L);
Car car = (Car) namedQuery.getSingleResult();
System.out.println(car);
```

- Cách sử dụng @Query:

Khai báo native query trong Repository

```java
@Query(value = "SELECT * FROM car WHERE id=:id", nativeQuery = true)
List<Car> getCarById(@Param("id") long id);
```

Dùng EntityManager tạo native query

```java
Query nativeQuery = em.createNativeQuery("SELECT * FROM car WHERE
id=:id", Car.class); //Không dùng oto mà dùng car
nativeQuery.setParameter("id", 1L);
Car car = (Car) nativeQuery.getSingleResult();
System.out.println(car);
```

---

## Câu 16: Làm thế nào để có thể viết custom method implemetations cho Jpa Repository. Nêu ví dụ

1. Tạo interface repository PersonRepo kế thừa JpaRepository và Custom Repository
   Interface. Chú ý quy tắc đặt tên PersonRepoCustom
2. Hiện thực hoá các phương thức khai báo ở Custom Repository Interface ở
   PersonRepoImpl.

Ví dụ:

Tạo interface `PesonRepo.java`:

```java
@Repository
public interface PersonRepo extends JpaRepository<Person, Long>,
PersonRepoCusto
```

Tạo interface `PersonRepoCustom.java`:

```java
public interface PersonRepoCustom {
TreeMap<String, List<Person>> groupPeopleByOrderCity();
}
```

Tạo class `PersonRepoImpl.java`:

```java
public class PersonRepoImpl implements PersonRepoCustom {
@Autowired @Lazy PersonRepo personRepository;
@PersistenceContext private EntityManager em;
@Override
public TreeMap<String, List<Person>> groupPeopleByOrderCity() {
return personRepository.findAll().stream()
.collect(Collectors.groupingBy(Person::getCity, TreeMap::new,
Collectors.toList()));
```

---

## Câu 17: Hãy nêu 1 ví dụ sử dụng sorting và paging khi query đối tượng Employee ở trên.

---

## Câu 18:

`Product.java`:

```java
@Entity (name="product")
@Table (name="product")
@Data
public class Product {
 @Id
 @GeneratedValue (strategy = GenerationType.AUTO)
 private long id;
 private String name;

    @ManyToOne (fetch = FetchType.LAZY)
    @JsonIgnore
  private Category category;

    @ManyToMany
  @JoinTable(
      name = "product_tag",
      joinColumns = @JoinColumn(name = "product_id"),
      inverseJoinColumns = @JoinColumn(name = "tag_id")
  )
}

```

`Category.java`:

```java
@Entity (name="category")
@Table (name="category")
@Data
public class Category {
 @Id
 @GeneratedValue(strategy = GenerationType.AUTO)
 private long id;
 private String name;

@OneToMany (cascade = CascadeType.PERSIST, orphanRemoval = false)
@JoinColumn (name = "category_id")
private List<Product> products = new ArrayList<>();
}

```

`Tag.java`:

```java
@Entity(name="tag")
@Table(name="tag")
@Data
public class Tag {
 @Id
 @GeneratedValue(strategy = GenerationType.AUTO)
 private long id;
 private String name;

  @JsonBackReference
  @ManyToMany(mappedBy = "tags", fetch = FetchType.LAZY)
  List<Product> produtcs = new ArrayList<>();

}
```

---

## Câu 19:

`Student.java`:

```java
@Entity(name = "student")
@Table(name = "student")
@Data
@NoArgsConstructor
public class Student {
  @Id @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;

  private String name;
  public Student(String name) {
    this.name = name;
  }

  @OneToMany(mappedBy = "student")
  @JsonIgnore
  private List<StudentSubject> studentSubjects = new ArrayList<>();

  @JsonGetter(value = "subjects")
  @Transient
  public Map<String, Float> getSubjects() {
    Map<String, Float> subjectScore = new HashMap<>();
    studentSubjects.stream().forEach( studentSubject -> {
      subjectScore.put(studentSubject.getSubject().getName(), studentSubject.getScore());
      }
    );
    return subjectScore;
  }
  }

```

`Course.java`:

```java
@Entity(name = "course")
@Table(name = "course")
@Data
@NoArgsConstructor
public class Course {
  @Id @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;

  @OneToMany(mappedBy = "course")
  @JsonIgnore
  private List<StudentCourse> studentCourses = new ArrayList<>();

  @JsonGetter(value = "students")
  @Transient
  public Map<String, Float> getStudents() {
    Map<String, Float> studentScore = new HashMap<>();
    studentCourses.stream().forEach( studentCourse -> {
              studentScore.put(studentCourse.getStudent().getName(), studentCourse.getScore());
            }
    );
    return studentScore;
  }

}
```

`StudentCourse.java`:

```java
@Entity (name = "student_course")
@Table (name = "student_course")
@Data
@NoArgsConstructor
public class StudentCourse {
  @Id @GeneratedValue (strategy = GenerationType.AUTO)
  private Long id;

  @ManyToOne (cascade = CascadeType.ALL)
  @JoinColumn (name = "student_id")
  private Student student;

  @ManyToOne (cascade = CascadeType.ALL)
  @JoinColumn (name = "course_id")
  private Course course;

  private int score; //extra column

  public StudentCourse (Student student, Course course, int score) {
    this.student = student;
    this.course = course;
    this.score = score;
  }
}
```

---

## Câu 20:
