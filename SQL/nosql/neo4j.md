# NEO4J

## 图数据库

> 节点和关系组成的数据模型，典型应用场景 社交网络、推荐系统、意向图、消费图、兴趣图、关系图谱
>
> 图计算

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
CREATE OR REPLACE (<node-name>:<label-name>)
// 已知节点 创建关系
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>)
CREATE  
	(<node1-label-name>)-[<relationship-label-name>:<relationship-name>]->(<node2-label-name>)
RETURN <relationship-label-name>
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
 // null 查询不到时返回空
 OPTIONAL MATCH
```



### WHERE

> 根据属性条件查询

```CQL
MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
RETURN emp
// 字典顺序大于Abc
MATCH (emp:Employee) 
WHERE emp.name > 'Abc'
RETURN emp
// 查询 null
MATCH (e:Employee) 
WHERE e.id IS NULL
RETURN e.id,e.name,e.sal,e.deptno
// 查询 in
MATCH (e:Employee) 
WHERE e.id IN [123,124]
RETURN e.id,e.name,e.sal,e.deptno
// 模糊查询 正则
match(emp) where emp.name =~'.*haha.*' return emp
// (?i) 大小写不敏感
match(emp) where emp.name =~'(?h)aha.*' return emp
// id查询
MATCH (n) WHERE id(n)=251 RETURN n
// starts with
MATCH(n) where n.name STARTS WITH 'AA' RETURN n
// not 
MATCH(n) where NOT n.name STARTS WITH 'AA' RETURN n

// 节点条件
// 不带类型 直接关联 
MATCH p=(a:Person{name:'Johan'}) -- (b:Person{name:'Ian'})
// 不带类型关联
MATCH p=(a:Person{name:'Johan'})-[*]-(b:Person{name:'Ian'})
// 长度1-2
MATCH p=(a:Person{name:'Johan'})-[:KNOWS*1..2]-(b:Person{name:'Ian'})
```

### DELETE

> 用于删除节点和关联关系。

```CQL
MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
DELETE emp

// 删除节点及关系
DETACH DELETE
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
  RETURN ENDNODE(movie) AS smovie,ID(movie),TYPE(movie)
  ```

- WITH... AS

  ```CQL
  //    WITH语句将分段的查询部分连接在一起，查询结果从一部分以管道形式传递给另外一部分作为开始点
  MATCH (david { name: 'Tom Hanks' })--()--(otherPerson)
  WITH otherPerson, count(*) AS foaf
  WHERE foaf > 1
  RETURN otherPerson
  ```

- COLLECT(n.title)

  ```CQL
  MATCH (am:Movie)<-[ai:ACTED_IN]-(p:Person)-[d:DIRECTED]->(dm:Movie) return p, collect(ai), collect(d), collect(am), collect(dm)
  ```

- UNWIND

  ```cql
  // 行转列
  UNWIND [1, 2, 3] AS x
  RETURN x
  ```

- CASE

  ```CQL
  MATCH(m)
  RETURN 
  CASE m.eyes
  WHEN 'B'
  THEN 1
  ELSE 2 END AS reult
  ```

- 其他

  | 函数        | 说明               | 函数        | 说明         | 函数      | 说明               | 函数             | 说明         |
  | ----------- | ------------------ | ----------- | ------------ | --------- | ------------------ | ---------------- | ------------ |
  | all()       |                    | any()       |              | none()    |                    | single()         |              |
  | exists()    |                    | size()      | 结果集个数   | length()  |                    | type()           | 关系类型     |
  | id()        | 关系或节点id       | coalesce()  | 第一个非空值 | head()    | 第一个元素         | last()           | 最后一个元素 |
  | timestamp() |                    | startNode() |              | endNode() |                    | properties()     | 实体转map    |
  | toInt()     |                    | toFloat()   |              | nodes()   | 路径所有节点       | relastionships() | 路径所有关系 |
  | labels()    | 节点所有标签       | keys()      | 属性名称     | extract() | 所有节点的某个属性 | filter()         |              |
  | tail()      | 除第一个的所有元素 | range()     |              | reduce()  | 每个元素执行表达式 | abs()            |              |
  | ceil()      |                    | floor()     |              | round()   |                    | sign()           | 数的正负值   |
  | rand()      | 随机数             | log()       |              | log10()   |                    | exp()            |              |
  | e()         |                    | sqrt()      |              | sin()     |                    | cos()            |              |
  | tan()       |                    | cot()       |              | asin()    |                    | acos()           |              |
  | atan()      |                    | atan2()     |              | pi()      |                    | degrees()        | 弧度转度     |
| radians()   | 度转弧度           | haversin()  | 半正矢       | replace() |                    | substring()      |              |
  | left()      | 左边指定长         | right()     |              | ltrim()   | 左除空             | rtrim()          |              |
  | trim()      |                    | lower()     |              | upper()   |                    | split()          |              |
  | reverse()   | 倒叙               | toString()  | 转字符串     |           |                    |                  |              |
  
   
  

## Spring Data JPA

### pom

```xml
    <properties>
		<java.version>8</java.version>
		<maven-deploy-plugin.version>3.0.0-M1</maven-deploy-plugin.version>
		<maven-failsafe-plugin.version>3.0.0-M3</maven-failsafe-plugin.version>
		<maven-install-plugin.version>3.0.0-M1</maven-install-plugin.version>
		<maven-surefire-plugin.version>3.0.0-M3</maven-surefire-plugin.version>
		<neo4j.version>4.1.0</neo4j.version>
		<spring-data-neo4j-rx.version>${revision}${sha1}${changelist}</spring-data-neo4j-rx.version>
		<testcontainers.version>1.13.0</testcontainers.version>
    </properties> 
<dependencies>
<dependency>
			<groupId>org.neo4j.springframework.data</groupId>
			<artifactId>spring-data-neo4j-rx-spring-boot-starter</artifactId>
			<version>${spring-data-neo4j-rx.version}</version>
		</dependency>
		<dependency>
			<groupId>org.neo4j.springframework.data</groupId>
			<artifactId>spring-data-neo4j-rx-spring-boot-test-autoconfigure</artifactId>
			<version>${spring-data-neo4j-rx.version}</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.neo4j.test</groupId>
			<artifactId>neo4j-harness</artifactId>
			<version>${neo4j.version}</version>
			<scope>test</scope>
		</dependency>
    </dependencies>
<profiles>
		<profile>
			<id>revisionMissing</id>
			<activation>
				<property>
					<name>!revision</name>
				</property>
			</activation>
			<properties>
				<revision>1.1.1</revision>
			</properties>
		</profile>
		<profile>
			<id>sha1Missing</id>
			<activation>
				<property>
					<name>!sha</name>
				</property>
			</activation>
			<properties>
				<sha1></sha1>
			</properties>
		</profile>
		<profile>
			<id>changelistMissing</id>
			<activation>
				<property>
					<name>!changelist</name>
				</property>
			</activation>
			<properties>
				<changelist></changelist>
			</properties>
		</profile>
	</profiles>
```

### application conf

```properties
#org.springframework.data.rest.level=DEBUG
#debug: true
org.neo4j.driver.uri=bolt://192.168.0.61:7687
org.neo4j.driver.authentication.username=neo4j
org.neo4j.driver.authentication.password=password
org.neo4j.driver.config.encrypted=false
org.neo4j.data.database=neo4j

```

### support

```java
@Component
public class MovieModule extends SimpleModule {

	public MovieModule() {

		setMixInAnnotation(MovieEntity.class, MovieEntityMixin.class);
		setMixInAnnotation(PersonEntity.class, PersonEntityMixin.class);

		addDeserializer(Roles.class, new RoleDeserializer());
	}

	@JsonInclude(JsonInclude.Include.NON_EMPTY)
	abstract static class MovieEntityMixin {
		@JsonCreator MovieEntityMixin(@JsonProperty("title") final String title,
			@JsonProperty("description") final String description) {
		}

		@JsonDeserialize(keyUsing = PersonEntityAsKeyDeSerializer.class)
		@JsonSerialize(keyUsing = PersonEntityAsKeySerializer.class, contentUsing = RolesAsContentSerializer.class)
		abstract Map<PersonEntity, Roles> getActorsAndRoles();

	}

	@JsonInclude(JsonInclude.Include.NON_EMPTY)
	abstract static class PersonEntityMixin {
		@JsonCreator PersonEntityMixin(@JsonProperty("name") final String name, @JsonProperty("born") final Long born) {
		}
	}

	static class RoleDeserializer extends JsonDeserializer<Roles> {

		@Override
		public Roles deserialize(JsonParser jsonParser,
			DeserializationContext deserializationContext) throws IOException {

			return new Roles(jsonParser.readValueAs(new TypeReference<List<String>>() {
			}));
		}
	}

	static class PersonEntityAsKeyDeSerializer extends KeyDeserializer {

		@Override
		public Object deserializeKey(String key, DeserializationContext ctxt) {
			return new PersonEntity(null, key);
		}
	}

	static class PersonEntityAsKeySerializer extends JsonSerializer<PersonEntity> {

		@Override
		public void serialize(PersonEntity personEntity, JsonGenerator jsonGenerator,
			SerializerProvider serializerProvider) throws IOException {
			jsonGenerator.writeFieldName(personEntity.getName());
		}
	}

	static class RolesAsContentSerializer extends JsonSerializer<Roles> {
		@Override
		public void serialize(Roles roles, JsonGenerator jsonGenerator,
			SerializerProvider serializerProvider) throws IOException {
			jsonGenerator.writeObject(roles.getRoles());
		}
	}
}
```

### pojo

- node

```java
@Node("Movie")
public class MovieEntity {

	@Id
	private final String title;

	@Property("tagline")
	private final String description;

	@Relationship(type = "ACTED_IN", direction = INCOMING)
	private Map<PersonEntity, Roles> actorsAndRoles = new HashMap<>();

	@Relationship(type = "DIRECTED", direction = INCOMING)
	private List<PersonEntity> directors = new ArrayList<>();

	public MovieEntity(String title, String description) {
		this.title = title;
		this.description = description;
	}
}
```

```java
@Node("Person")
public class PersonEntity {

	@Id
	private final String name;

	private Integer born;

	public PersonEntity(Integer born, String name) {
		this.born = born;
		this.name = name;
	}
}
```

- relationship

```java
@RelationshipProperties
public class Roles {

	private final List<String> roles;

	public Roles(List<String> roles) {
		this.roles = roles;
	}

	public List<String> getRoles() {
		return roles;
	}
}

```

### repository

```java
public interface PersonRepository extends Neo4jRepository<PersonEntity, String> {

	Optional<PersonEntity> findByName(String name);

	@Query("MATCH (am:Movie)<-[ai:ACTED_IN]-(p:Person)-[d:DIRECTED]->(dm:Movie) return p, collect(ai), collect(d), collect(am), collect(dm)")
	List<PersonEntity> getPersonsWhoActAndDirect();
}

```

### service

```java
@GetMapping("/by-title")
MovieEntity byTitle(@RequestParam String title) {
    return movieRepository.findOneByTitle(title)
        .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
}

@PostMapping
String updatePerson(PersonEntity updatedPerson) {

    this.personRepository.findByName(updatedPerson.getName()).ifPresent(p -> {
        p.setBorn(updatedPerson.getBorn());
        this.personRepository.save(p);
    });

    return "redirect:" + MvcUriComponentsBuilder.fromMethodName(
        this.getClass(), "showPersonForm", updatedPerson).build();
}
```

## *Neo4j Graph Data Science library*

## 简介

> neo4j 计算扩展依赖
- 官方文档地址
[英文原文](https://neo4j.com/docs/graph-data-science/current/introduction/)
## 安装

- 版本对应

  | Neo4j Graph Data Science                                     | Neo4j version      |
  | :----------------------------------------------------------- | :----------------- |
  | `1.3.x`                                                      | `4.1.0` - `4.1.2`  |
  | `1.3.x`                                                      | `4.0.0` - `4.0.8`  |
  | `1.2.3` [[a\]](https://neo4j.com/docs/graph-data-science/current/installation/#ftn.deprecated) | `4.0.0` - `4.0.6`  |
  | `1.2.0` - `1.2.2` [[a\]](https://neo4j.com/docs/graph-data-science/current/installation/#ftn.deprecated) | `4.0.0` - `4.0.4`  |
  | `1.1.x`                                                      | `3.5.9` - `3.5.22` |
  | `1.0.x` [[a\]](https://neo4j.com/docs/graph-data-science/current/installation/#ftn.deprecated) | `3.5.9` - `3.5.18` |
  | [[a\] ](https://neo4j.com/docs/graph-data-science/current/installation/#deprecated)This version series is end-of-life and will not receive further patches. Please use a later version. |                    |

- 下载安装包
	
	[地址](https://neo4j.com/download-center/#community)
	
- 修改配置文件

  neo4j.conf

  ```conf
  dbms.security.procedures.unrestricted=gds.*
  dbms.security.procedures.whitelist=gds.*
  ```

- 验证是否安装成功

  ```CQL
  RETURN gds.version()
  CALL gds.list()
  ```

  

## cypher 使用

```CQL
// 创建图 格式
CALL gds.graph.create(
    graphName: String,
    nodeProjection: String, List or Map,
    relationshipProjection: String, List or Map,
    configuration: Map
)
// 示例 map
CALL gds.graph.create(
    'my-graph', {
        Person: { label: 'Person' },
        City: { label: 'City' }
    },
    '*'
)
YIELD graphName, nodeCount, relationshipCount;
// 示例 list
CALL gds.graph.create('my-graph', ['Person', 'City'], '*')
YIELD graphName, nodeCount, relationshipCount;
```

## 集群搭建

https://blog.csdn.net/thinkercode/article/details/46472209

- 注：仅供企业版使用