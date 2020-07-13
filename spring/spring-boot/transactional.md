# 事务





## 使用

```java
@Service
public class PersonService {
    @Autowired
    private PersonMapper personMapper;

    @Autowired
    private CompanyMapper companyMapper;

    @Transactional(rollbackFor = {RuntimeException.class, Error.class})
    public void saveOne(Person person) {
        Company company = new Company();
        company.setName("aa:" + person.getName());
        companyMapper.insertOne(company);
        personMapper.insertOne(person);
    }
}
```


`rollbackFor`：触发回滚的异常，默认是`RuntimeException`和`Error`


`isolation`: 事务的隔离级别，默认是`Isolation.DEFAULT`也就是数据库自身的默认隔离级别，比如MySQL是`ISOLATION_REPEATABLE_READ`可重复读

