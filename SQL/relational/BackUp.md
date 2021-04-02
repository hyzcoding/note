# 主从同步

## mysql主从同步配置

- 主服务器

  - `/etc/my.cnf`

    ```bash
    [mysqld]
    server-id=1 # 服务数据库id
    log-bin=master-bin
    log-bin-index=master-bin.index
    binlog-do-db=stk # 同步的数据库
    ```

    

  - mysql shell

- 从服务器

  - `/etc/my.cnf`

    ```bash
    [mysqld]
    server-id=2
    binlog-do-db=stk
    ```

  - mysql shell

    ```bash
    change master to master_host='主xxx.xxx.xxx.xx',master_port=3306,master_user='repl',master_password='mysql',master_log_file='master-bin.000001',master_log_pos=0;
    start slave;
    show slave status \G; 
    stop slave;
    ```

    