# DAO란?

Data Access Object의 약자로, 실제 DB에 접근하는 역할을 수행하는 객체이다.
DB에 접근하는 로직과 비지니스 로직을 분리하기 위해 사용되는 객체이다.

매번 DB에 접근하여 데이터를 불러오거나 조작하기위해 커넥션 객체를 생성하는것이 아닌 전담객체를 두고 필요할때마다 호출하여 사용한 후 반납하는 것

웹서버는 DB와 연결하기 위해 매번 커넥션 객체를 생성하는데, 이를 해결하기 위해 Connection Pool 을사용한다.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDao {

    public void add(TestDto dto) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test", "root", "root");

        PreparedStatement preparedStatement = connection.prepareStatement("insert into users(id,name,password) value(?,?,?)");

        preparedStatement.setString(1, dto.getName());
        preparedStatement.setInt(2, dto.getValue());
        preparedStatement.setString(3, dto.getData());
        preparedStatement.executeUpdate();
        preparedStatement.close();
        
        connection.close();

    }
}
```

위 코드는 DAO의 예시 중 하나이다. `Connection` 객체를 생성 및 초기화된 상태로 가지고 있기 때문에 DB에 접근할 때마다 새로 접근할 필요가 없어진다.

# DTO란?

Data Transfer Object의 약자로, 데이터를 운반하는 객체이다.
계층간 데이터 교환을 위해 존재하며 로직은 존재하지 않고 getter와 setter만 가진다.

```java
public class PersonDTO {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

