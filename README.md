# Paymybuddy-Kubernetes

kubectl apply -f paymybuddy-namespace.yaml # Pour créer le namespace
Résultat => namespace/paymybuddy created

kubectl apply -f . -n paymybuddy # Appliquer tous les manifestes
Résultat =>
configmap/db-init-scripts-configmap created
deployment.apps/mysql created
service/mysql created
deployment.apps/paymybuddy-app created
namespace/paymybuddy unchanged
secret/mysql-secret created
service/paymybuddy-service created
service/paymybuddy-webapp created

kubectl get pod -n paymybuddy # Pour lister tous les pods dans le namespace paymybuddy
Résultat =>
NAME                              READY   STATUS    RESTARTS   AGE
mysql-774fcfcf76-8blxb            1/1     Running   0          27s
paymybuddy-app-7f4867f967-dtfb2   1/1     Running   0          27s

kubectl get svc -n paymybuddy # Pour lister tous les services dans le namespace paymybuddy
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
mysql                ClusterIP   10.105.28.130    <none>        3306/TCP       4m28s
paymybuddy-service   ClusterIP   10.110.73.162    <none>        8080/TCP       4m28s
paymybuddy-webapp    NodePort    10.109.152.102   <none>        80:30081/TCP   4m28s

kubectl logs mysql-774fcfcf76-8blxb -n paymybuddy # Regarder les logs de la base de données pour s'assurer qu'elle a bien bien démarré
Résultat => 
2025-06-11 20:54:47+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.42-1.el9 started.
2025-06-11 20:54:48+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2025-06-11 20:54:48+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.42-1.el9 started.
'/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
2025-06-11T20:54:48.685178Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
2025-06-11T20:54:48.723744Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.42) starting as process 1
2025-06-11T20:54:48.733176Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2025-06-11T20:54:49.055673Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2025-06-11T20:54:49.282811Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2025-06-11T20:54:49.282846Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2025-06-11T20:54:49.285132Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2025-06-11T20:54:49.348013Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
2025-06-11T20:54:49.348034Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.42'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.

kubectl logs paymybuddy-app-7f4867f967-dtfb2 -n paymybuddy # Regarder les logs de l'application pour s'assurer qu'elle a bien bien démarré

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.1)

2025-06-11 20:54:49.267  INFO 7 --- [           main] c.p.paymybuddy.PayMyBuddyApplication     : Starting PayMyBuddyApplication v0.0.1-SNAPSHOT using Java 17.0.2 on paymybuddy-app-7f4867f967-dtfb2 with PID 7 (/app/paymybuddy.jar started by root in /app)
2025-06-11 20:54:49.269  INFO 7 --- [           main] c.p.paymybuddy.PayMyBuddyApplication     : No active profile set, falling back to 1 default profile: "default"
2025-06-11 20:54:49.931  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2025-06-11 20:54:49.971  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 34 ms. Found 4 JPA repository interfaces.
2025-06-11 20:54:50.517  INFO 7 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8081 (http)
2025-06-11 20:54:50.526  INFO 7 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2025-06-11 20:54:50.526  INFO 7 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.64]
2025-06-11 20:54:50.586  INFO 7 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2025-06-11 20:54:50.586  INFO 7 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1245 ms
2025-06-11 20:54:50.980  INFO 7 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2025-06-11 20:54:51.329  INFO 7 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2025-06-11 20:54:51.379  INFO 7 --- [           main] liquibase                                : Clearing database change log checksums
2025-06-11 20:54:51.670  INFO 7 --- [           main] liquibase.lockservice                    : Successfully acquired change log lock
2025-06-11 20:54:51.867  INFO 7 --- [           main] liquibase.lockservice                    : Successfully released change log lock
2025-06-11 20:54:51.929  INFO 7 --- [           main] liquibase.lockservice                    : Successfully acquired change log lock
2025-06-11 20:54:52.163  INFO 7 --- [           main] liquibase.changelog                      : Reading from db_paymybuddy.DATABASECHANGELOG
2025-06-11 20:54:52.201  INFO 7 --- [           main] liquibase.changelog                      : Reading from db_paymybuddy.DATABASECHANGELOG
2025-06-11 20:54:52.242  INFO 7 --- [           main] liquibase.lockservice                    : Successfully released change log lock
2025-06-11 20:54:52.323  INFO 7 --- [           main] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
2025-06-11 20:54:52.368  INFO 7 --- [           main] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 5.6.9.Final
2025-06-11 20:54:52.512  INFO 7 --- [           main] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.1.2.Final}
2025-06-11 20:54:52.630  INFO 7 --- [           main] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.MySQL8Dialect
2025-06-11 20:54:53.201  INFO 7 --- [           main] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
2025-06-11 20:54:53.207  INFO 7 --- [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2025-06-11 20:54:53.811  WARN 7 --- [           main] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning
2025-06-11 20:54:54.047  INFO 7 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will secure any request with [org.springframework.security.web.session.DisableEncodeUrlFilter@6f31df32, org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@7c90b7b7, org.springframework.security.web.context.SecurityContextPersistenceFilter@2213639b, org.springframework.security.web.header.HeaderWriterFilter@46fc522d, org.springframework.security.web.csrf.CsrfFilter@1376883, org.springframework.security.web.authentication.logout.LogoutFilter@50b93353, org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter@8f40022, org.springframework.security.web.savedrequest.RequestCacheAwareFilter@6f25bf88, org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@7b5cc918, org.springframework.security.web.authentication.rememberme.RememberMeAuthenticationFilter@6c8fe7a4, org.springframework.security.web.authentication.AnonymousAuthenticationFilter@5d7911d5, org.springframework.security.web.session.SessionManagementFilter@5f0a2638, org.springframework.security.web.access.ExceptionTranslationFilter@3e046e39, org.springframework.security.web.access.intercept.AuthorizationFilter@75d366c2]
2025-06-11 20:54:54.051  WARN 7 --- [           main] o.s.s.c.a.web.builders.WebSecurity       : You are asking Spring Security to ignore Ant [pattern='/images/**']. This is not recommended -- please use permitAll via HttpSecurity#authorizeHttpRequests instead.
2025-06-11 20:54:54.051  INFO 7 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will not secure Ant [pattern='/images/**']
2025-06-11 20:54:54.051  WARN 7 --- [           main] o.s.s.c.a.web.builders.WebSecurity       : You are asking Spring Security to ignore Ant [pattern='/js/**']. This is not recommended -- please use permitAll via HttpSecurity#authorizeHttpRequests instead.
2025-06-11 20:54:54.051  INFO 7 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will not secure Ant [pattern='/js/**']
2025-06-11 20:54:54.051  WARN 7 --- [           main] o.s.s.c.a.web.builders.WebSecurity       : You are asking Spring Security to ignore Ant [pattern='/webjars/**']. This is not recommended -- please use permitAll via HttpSecurity#authorizeHttpRequests instead.
2025-06-11 20:54:54.051  INFO 7 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will not secure Ant [pattern='/webjars/**']
2025-06-11 20:54:54.290  INFO 7 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8081 (http) with context path ''
2025-06-11 20:54:54.332  INFO 7 --- [           main] c.p.paymybuddy.PayMyBuddyApplication     : Started PayMyBuddyApplication in 5.685 seconds (JVM running for 6.417)

On voit l'application a bien démarré et le port interne du conateneur est 8081

kubectl exec -it -n paymybuddy mysql-774fcfcf76-8blxb -- /bin/bash

bash-5.1# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 28
Server version: 8.0.42 MySQL Community Server - GPL

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db_paymybuddy      |
| information_schema |
| mysql              |
| paymybuddy         |
| performance_schema |
| sys                |
+--------------------+
6 rows in set (0.03 sec)

mysql> USE db_paymybuddy;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+-------------------------+
| Tables_in_db_paymybuddy |
+-------------------------+
| DATABASECHANGELOG       |
| DATABASECHANGELOGLOCK   |
| bank_account            |
| connection              |
| transaction             |
| user                    |
+-------------------------+
6 rows in set (0.00 sec)

mysql> SELECT * FROM user;
+---------+---------------------+--------------------------------------------------------------+-----------+-----------+---------+
| user_id | email               | password                                                     | firstname | lastname  | balance |
+---------+---------------------+--------------------------------------------------------------+-----------+-----------+---------+
|       1 | security@mail.com   | $2a$10$vpDkNfBtWg.ebbkL8VwaG.BrmlIlqRCd0RqoyOIb6hgRZRMfJ51xa | Security  | User      |    0.00 |
|       2 | hayley@mymail.com   | $2a$10$1NDocQWD9pl52dv/cY7mmOuCYbIVTzCd6ahb5EUDQxwkDMkg1Q54y | Hayley    | James     |   10.00 |
|       3 | clara@mail.com      | $2a$10$41nUyaddehEi9Slu/4kFWeedO3YrLnGCu5nZqYySX3CH7uyHMrclu | Clara     | Tarazi    |  133.56 |
|       4 | smith@mail.com      | $2a$10$3TU.lRztZJgEueboxsP2b.AV6TeBsKK.qyyCYGYJXKeozeahFVTuu | Smith     | Sam       |    8.00 |
|       5 | lambda@mail.com     | $2a$10$prOZuMO22K.itqO3CKrEGuVf2KUxdWOB9fGQh8DvWHPHWIiiR6iZy | Lambda    | User      |   96.91 |
|       6 | test@gmail.com      | $2a$10$6eEU1XTArLhMFsVgptrEZusjno7UyEfomo/yDZJBisF4QkE2YMGKy | test      | test      |    0.00 |
|       7 | test1@gmail.com     | $2a$10$niNaAqNBl7IE4HQSLPvUEeCMS04FrhnK6J1FkbdH/7SQJVviyeEKq | test1     | test1     |    0.00 |
|       8 | test-user@gmail.com | $2a$10$1ihjmHWEbanm5g3yeoyUGeOQVCpL5jFlXFb4WKs4Rh0gzM4dgS3LG | test-user | test-user |    0.00 |
+---------+---------------------+--------------------------------------------------------------+-----------+-----------+---------+
8 rows in set (0.02 sec)

bash-5.1# ls -l /data
total 100756
-rw-r----- 1 mysql mysql   196608 Jun 11 21:25 '#ib_16384_0.dblwr'
-rw-r----- 1 mysql mysql  8585216 Jun  5 23:56 '#ib_16384_1.dblwr'
drwxr-x--- 2 mysql mysql     4096 Jun 11 20:54 '#innodb_redo'
drwxr-x--- 2 mysql mysql     4096 Jun 11 20:54 '#innodb_temp'
-rw-r----- 1 mysql mysql       56 Jun  5 23:56  auto.cnf
-rw-r----- 1 mysql mysql  2984808 Jun  5 23:56  binlog.000001
-rw-r----- 1 mysql mysql      180 Jun  5 23:57  binlog.000002
-rw-r----- 1 mysql mysql      180 Jun  5 23:57  binlog.000003
-rw-r----- 1 mysql mysql      180 Jun  6 00:13  binlog.000004
-rw-r----- 1 mysql mysql      180 Jun  6 00:24  binlog.000005
-rw-r----- 1 mysql mysql      180 Jun  6 14:09  binlog.000006
-rw-r----- 1 mysql mysql      180 Jun  6 14:16  binlog.000007
-rw-r----- 1 mysql mysql      180 Jun  6 14:18  binlog.000008
-rw-r----- 1 mysql mysql      180 Jun  6 14:20  binlog.000009
-rw-r----- 1 mysql mysql      180 Jun  6 14:25  binlog.000010
-rw-r----- 1 mysql mysql      180 Jun  6 15:19  binlog.000011
-rw-r----- 1 mysql mysql     5353 Jun  6 17:56  binlog.000012
-rw-r----- 1 mysql mysql     6912 Jun  6 23:23  binlog.000013
-rw-r----- 1 mysql mysql      157 Jun  6 23:58  binlog.000014
-rw-r----- 1 mysql mysql      180 Jun  6 23:58  binlog.000015
-rw-r----- 1 mysql mysql      180 Jun  7 00:00  binlog.000016
-rw-r----- 1 mysql mysql     8536 Jun 10 20:56  binlog.000017
-rw-r----- 1 mysql mysql      974 Jun 10 21:10  binlog.000018
-rw-r----- 1 mysql mysql      974 Jun 10 21:12  binlog.000019
-rw-r----- 1 mysql mysql     3016 Jun 11 20:03  binlog.000020
-rw-r----- 1 mysql mysql     3016 Jun 11 20:52  binlog.000021
-rw-r----- 1 mysql mysql     3416 Jun 11 21:18  binlog.000022
-rw-r----- 1 mysql mysql      336 Jun 11 20:54  binlog.index
-rw------- 1 mysql mysql     1705 Jun  5 23:56  ca-key.pem
-rw-r--r-- 1 mysql mysql     1112 Jun  5 23:56  ca.pem
-rw-r--r-- 1 mysql mysql     1112 Jun  5 23:56  client-cert.pem
-rw------- 1 mysql mysql     1705 Jun  5 23:56  client-key.pem
drwxr-x--- 2 mysql mysql     4096 Jun 10 20:55  db_paymybuddy <=====
-rw-r----- 1 mysql mysql     3822 Jun 11 20:52  ib_buffer_pool
-rw-r----- 1 mysql mysql 12582912 Jun 11 21:25  ibdata1
-rw-r----- 1 mysql mysql 12582912 Jun 11 20:54  ibtmp1
drwxr-x--- 2 mysql mysql     4096 Jun  5 23:56  mysql
-rw-r----- 1 mysql mysql 32505856 Jun 11 21:18  mysql.ibd
lrwxrwxrwx 1 mysql mysql       27 Jun  6 17:56  mysql.sock -> /var/run/mysqld/mysqld.sock
drwxr-x--- 2 mysql mysql     4096 Jun 10 20:55  paymybuddy
drwxr-x--- 2 mysql mysql     4096 Jun  5 23:56  performance_schema
-rw------- 1 mysql mysql     1705 Jun  5 23:56  private_key.pem
-rw-r--r-- 1 mysql mysql      452 Jun  5 23:56  public_key.pem
-rw-r--r-- 1 mysql mysql     1112 Jun  5 23:56  server-cert.pem
-rw------- 1 mysql mysql     1705 Jun  5 23:56  server-key.pem
drwxr-x--- 2 mysql mysql     4096 Jun  5 23:56  sys
-rw-r----- 1 mysql mysql 16777216 Jun 11 20:56  undo_001
-rw-r----- 1 mysql mysql 16777216 Jun 11 21:18  undo_002
Footer
