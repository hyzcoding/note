# NEO4J

## 基础

- 节点

- 标签

- 关系

  ```CQL
  // 左至右
  CREATE (<node1-details>)-[<relationship-details>]->(<node2-details>)
  CREATE (n1:Node1)-[r1:Relationship]->(n2:Node2)
  ```

  

## 数据库

- 复制数据库

  ```bash
  ## 仅支持企业版
  $neo4j-home> bin/neo4j-admin copy --from-database=neo4j --to-database=copy --delete-nodes-with-labels="Cat,Dog"
  
  ```

  

- 创建用户

## 浏览器

- 用户

## CQL语句

### CREATE

```CQL
// 一个节点，一个标签
CREATE (<node-name>:<label-name>)
// 一个节点，多个标签
CREATE (<node-name>:<label-name1>:<label-name2>.....:<label-namen>)
// 节点带属性键值对
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
// 多个节点带关系
CREATE (<node1-name>:<label1-name>)-
	[(<relationship-name>:<relationship-label-name>)]
	->(<node2-name>:<label2-name>)
// 如果不存在则创建
CREATE (<node-name>:<label-name>) IF NOT EXISTS
CREATE OR REPLACE (<node-name>:<label-name>)
```

| 语法元素                                | 描述                                            |
| --------------------------------------- | ----------------------------------------------- |
| `<node-name>`                           | 它是我们将要创建的节点名称。                    |
| `<label-name>`                          | 它是一个节点标签名称                            |
| `<Property1-name>...<Propertyn-name>`   | 属性是键值对。 定义将分配给创建节点的属性的名称 |
| `<Property1-value>...<Propertyn-value>` | 属性是键值对。 定义将分配给创建节点的属性的值   |

> ```CQL
> CREATE (dept:Dept { deptno:10,dname:"Accounting",location:"Hyderabad" })
> 
> CREATE (p1:Profile1)-[r1:LIKES]->(p2:Profile2)
> ```



### MATCH&RETURN

> match 和return 不能单独使用
>
> match 与 update或return 组合使用
>
> return 与create 或match 组合使用

```CQL
MATCH
(
   <node-name>:<label-name>
)
RETURN
 <node-name>.<property1-name>,
   ...
 <node-name>.<propertyn-name>
```

| 语法元素                              | 描述                                            |
| ------------------------------------- | ----------------------------------------------- |
| `<node-name>`                         | 它是我们将要创建的节点名称。                    |
| `<label-name>`                        | 它是一个节点标签名称                            |
| `<Property1-name>...<Propertyn-name>` | 属性是键值对。 定义将分配给创建节点的属性的名称 |

```CQL
// 查询节点
MATCH (dept: Dept)
 RETURN dept.deptno,dept.dname
// 查询关系
MATCH ( cc: CreditCard)-[r]-()
 RETURN r
```



### WHERE

> 根据属性条件查询

```CQL
MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
RETURN emp
// 查询 null
MATCH (e:Employee) 
WHERE e.id IS NULL
RETURN e.id,e.name,e.sal,e.deptno
// 查询 in
MATCH (e:Employee) 
WHERE e.id IN [123,124]
RETURN e.id,e.name,e.sal,e.deptno
// 模糊查询
match(emp) where emp.name =~'.*haha.*' return emp
```

### DELETE

> 用于删除节点和关联关系。

```CQL
MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
DELETE emp
```

## REMOVE

> 用于删除标签和属性。

```CQL
// 移除标签
MATCH (m:Movie) 
REMOVE m:Picture
// 移除属性
MATCH (dc:DebitCard) 
REMOVE dc.cvv
RETURN dc
```

## SET

> 设置属性

```CQL
MATCH (book:Book)
SET book.title = 'superstar'
RETURN book
```

## 排序与联结

```CQL
// ORDER BY (DESC)
    MATCH (emp:Employee)
    RETURN emp.empid,emp.name,emp.salary,emp.deptno
    ORDER BY emp.name DESC
// UNION & UNION ALL
    MATCH (cc:CreditCard) RETURN cc.id,cc.number
    UNION
    MATCH (dc:DebitCard) RETURN dc.id,dc.number
    // -- //
    MATCH (cc:CreditCard)
    RETURN cc.id as id,cc.number as number,cc.name as name,
       cc.valid_from as valid_from,cc.valid_to as valid_to
    UNION
    MATCH (dc:DebitCard)
    RETURN dc.id as id,dc.number as number,dc.name as name,
       dc.valid_from as valid_from,dc.valid_to as valid_to
    /** ---- **/
       MATCH (cc:CreditCard)
RETURN cc.id as id,cc.number as number,cc.name as name,
   cc.valid_from as valid_from,cc.valid_to as valid_to
UNION ALL
MATCH (dc:DebitCard)
RETURN dc.id as id,dc.number as number,dc.name as name,
   dc.valid_from as valid_from,dc.valid_to as valid_to
```

## LIMIT & SKIP

```CQL
// limit 返回前n条记录
MATCH (emp:Employee) 
RETURN emp
LIMIT 2
// skip 跳过前n条记录
MATCH (emp:Employee) 
RETURN emp
SKIP 2
```

## MERGE

> 相当于replace函数，存在则覆盖，不存在则创建

```CQL
MERGE (gp2:GoogleProfile2{ Id: 201402,Name:"Nokia"})
```



## 索引与约束

```CQL
// 索引
CREATE INDEX ON :<label_name> (<property_name>)
	CREATE INDEX ON :Customer (name)
DROP INDEX ON :<label_name> (<property_name>)
	DROP INDEX ON :Customer (name)
// 唯一约束语法
CREATE CONSTRAINT ON (<label_name>)
ASSERT <property_name> IS UNIQUE
    CREATE CONSTRAINT ON (cc:CreditCard)
    ASSERT cc.number IS UNIQUE
DROP CONSTRAINT ON (<label_name>)
ASSERT <property_name> IS UNIQUE
    DROP CONSTRAINT ON (cc:CreditCard)
    ASSERT cc.number IS UNIQUE
```

## 函数

- LOWER（） 小写输出

  ```CQL
  MATCH (e:Employee) 
  RETURN e.id,LOWER(e.name),e.sal,e.deptno
  ```

- SUBSTRING

  ```CQL
  MATCH (e:Employee) 
  RETURN e.id,SUBSTRING(e.name,0,2),e.sal,e.deptno
  ```

- COUNT

  ```CQL
  MATCH (e:Employee) RETURN COUNT(*)
  ```

- MAX/MIN

  ```CQL
  MATCH (e:Employee) 
  RETURN MAX(e.sal),MIN(e.sal)
  ```

- AVG/SUM

  ```CQL
  MATCH (e:Employee) 
  RETURN SUM(e.sal),AVG(e.sal)
  ```

- STARTNODE/ENDNODE

  ```CQL
  MATCH (a)-[movie:ACTION_MOVIES]->(b) 
  RETURN STARTNODE(movie),ID(movie),TYPE(movie)
  MATCH (a)-[movie:ACTION_MOVIES]->(b) 
  RETURN ENDNODE(movie),ID(movie),TYPE(movie)
  ```

  

- 

## Spring Data JPA

### pom

```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
		<neo4j.version>3.5.21</neo4j.version>
    </properties> 
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-neo4j</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>
        <dependency>
            <groupId>org.neo4j.test</groupId>
            <artifactId>neo4j-harness</artifactId>
            <version>${neo4j.version}</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-nop</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

```

### application conf

```properties
#org.springframework.data.rest.level=DEBUG
#debug: true
spring.data.neo4j.uri=bolt://localhost
spring.data.neo4j.username=neo4j
spring.data.neo4j.password=password

```

### application run

```java
@SpringBootApplication
@EnableNeo4jRepositories("movies.spring.data.neo4j.repositories")
public class SampleMovieApplication {

    public static void main(String[] args) {
        SpringApplication.run(SampleMovieApplication.class, args);
    }
}
```

### pojo

- node

```java
@NodeEntity
public class Movie {

	@Id
	@GeneratedValue
	private Long id;
	private String title;
	private int released;
	private String tagline;

	@JsonIgnoreProperties("movie")
	@Relationship(type = "ACTED_IN", direction = Relationship.INCOMING)
	private List<Role> roles;

	public void addRole(Role role) {
		if (this.roles == null) {
			this.roles = new ArrayList<>();
		}
		this.roles.add(role);
	}
}
```

```java
@NodeEntity
public class Person {

    @Id
    @GeneratedValue
	private Long id;
	private String name;
	private int born;

	@Relationship(type = "ACTED_IN")
	private List<Movie> movies = new ArrayList<>();

	public Person() {
	}

	public Person(String name, int born) {
		this.name = name;
		this.born = born;
	}
}
```

- relationship

```java
@RelationshipEntity(type = "ACTED_IN")
public class Role {

    @Id
    @GeneratedValue
	private Long id;
	private List<String> roles = new ArrayList<>();

	@StartNode
	private Person person;

	@EndNode
	private Movie movie;

	public Role() {
	}
    public Role(Movie movie, Person actor) {
		this.movie = movie;
		this.person = actor;
	}
}
```

### repository

```java
@RepositoryRestResource(collectionResourceRel = "movies", path = "movies")
public interface MovieRepository extends Neo4jRepository<Movie, Long> {

	Movie findByTitle(@Param("title") String title);

	Collection<Movie> findByTitleLike(@Param("title") String title);

    @Query("MATCH (m:Movie)<-[r:ACTED_IN]-(a:Person) RETURN m,r,a LIMIT $limit")
	Collection<Movie> findAllLimitBy(@Param("limit") int limit);
}
```

