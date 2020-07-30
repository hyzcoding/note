# ElasticSearch

## 简介

## 安装

- docker

  ```bash
  > docker pull elasticsearch:7.1.1
  $ docker network create somenetwork
  $ docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.1.1
  $ docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  -e ELASTICSEARCH_USERNAME="test"  -e ELASTICSEARCH_PASSWORD="123456" elasticsearch:7.1.1
  
  ```
## 配置



## 使用

- 版本

  |                  Spring Data Release Train                   |                  Spring Data Elasticsearch                   | Elasticsearch |                         Spring Boot                          |
  | :----------------------------------------------------------: | :----------------------------------------------------------: | :-----------: | :----------------------------------------------------------: |
  |                            Moore                             |                            3.2.x                             |    6.8.10     |                            2.2.x                             |
  |                           Lovelace                           |                            3.1.x                             |     6.2.2     |                            2.1.x                             |
  | Kay[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] | 3.0.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] |     5.5.0     | 2.0.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] |
  | Ingalls[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] | 2.1.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] |     2.4.0     | 1.5.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#_footnotedef_1)] |

- pom.xml

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
  </dependency>
  ```

- config

  ```java
  @SpringBootApplication
  @EnableElasticsearchRepositories(basePackages = "com.nightriver.jungle.document.repo")
  public class JungleDocumentApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(JungleDocumentApplication.class, args);
      }
  
  }
  ```

  

- java

  -  `CrudRepository` **interface**

  ```java
  public interface CrudRepository<T, ID> extends Repository<T, ID> {
   // Saves the given entity.
    <S extends T> S save(S entity);      
   // 	Returns the entity identified by the given ID.
    Optional<T> findById(ID primaryKey); 
   // 	Returns all entities.
    Iterable<T> findAll();               
   // Returns the number of entities.
    long count();                        
   // Deletes the given entity.
    void delete(T entity);               
   // Indicates whether an entity with the given ID exists.
    boolean existsById(ID primaryKey);   
  
    // … more functionality omitted.
  }
  ```

  - `PagingAndSortingRepository` **interface**

  ```java
  public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {
  
    Iterable<T> findAll(Sort sort);
  
    Page<T> findAll(Pageable pageable);
  }
  ```

  ```java
  PagingAndSortingRepository<User, Long> repository = // … get access to a bean
  Page<User> users = repository.findAll(PageRequest.of(1, 20));
  ```

  - **Query creation from method names**

  ```java
  interface PersonRepository extends Repository<Person, Long> {
  
    List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
  
    // Enables the distinct flag for the query
    List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
    List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
  
    // Enabling ignoring case for an individual property
    List<Person> findByLastnameIgnoreCase(String lastname);
    // Enabling ignoring case for all suitable properties
    List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
  
    // Enabling static ORDER BY for a query
    List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
    List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
  }
  ```

  


[Install Elasticsearch with Docker]: https://www.elastic.co/guide/en/elasticsearch/reference/7.5/docker.html
[Spring Data Elasticsearch - Reference Documentation]: https://docs.spring.io/spring-data/elasticsearch/docs/3.2.9.RELEASE/reference/html/#reference

