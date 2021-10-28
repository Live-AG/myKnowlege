
## Параметры подключения

http://10.78.77.62:9000/
login: `admin`
pass: `1qaz@WSX`

Мой пароль:
`AGusarov - 123`


## Настройка SonarQube

1.	Распокаовать SonarQube в папку
2.	Устанвлен PostgreSQL `postgresql-13.3-2-windows-x64`
```
Создаем пользователя: sonar_user с паролем admin
Создаем базу: sonar_db
```
3.	Установить Java 11 OpenJDK  ==(ВАЖНО! версия должна быть 8 или 11)==
4.	Настроить файл `wrapper.conf`
```
# указать путь к исполняемому файлу java
wrapper.java.comand=C:/jdk-11.0.2/bin/java
```
5.	Настроить файл `sonar.properties`
```
# User credentials.
# Permissions to create tables, indices and triggers must be granted to JDBC user.
# The schema must be created first.

sonar.jdbc.username=sonar_user
sonar.jdbc.password=admin

-------------------------------------------------------------

#----- PostgreSQL 9.6 or greater
# By default the schema named "public" is used. It can be overridden with the parameter "currentSchema".

sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonar_db

-------------------------------------------------------------

# WEB SERVER
...
sonar.web.javaOpts=-Xmx4g -Xms4g -XX:+HeapDumpOnOutOfMemoryError

-------------------------------------------------------------

# COMPUTE ENGINE
...
sonar.ce.javaOpts=-Xmx4g -Xms4g -XX:+HeapDumpOnOutOfMemoryError

-------------------------------------------------------------

# ELASTICSEARCH
...
# JVM options of Elasticsearch process
sonar.search.javaOpts=-Xmx4g -Xms4g -XX:MaxDirectMemorySize=1792m -XX:+HeapDumpOnOutOfMemoryError


```
5.	Устанавливаем новый пароль: `Логин: admin Пароль: 1qaz@WSX`
6.	При необходимости генерируем Токен: ` http://localhost:9000/account/security/`
7.	Добавляем нового пользователя `http://localhost:9000/admin/users` : `SonarUs / 1qaz@WSX`
8.	Скачиваем и распаковываем плагин:
```
Скачиваем и копируем плагин для SonarQube в каталог C:\sonarqube\extensions\downloads\

https://github.com/1c-syntax/sonar-bsl-plugin-community/releases/
```
9. Добавить проект FirstProject
10. Установить Sonar-scanner
11. Клонируем репозиторий с bat-ником `git clone https://github.com/otymko/article-sonar-for-infostart.git`
12. Настркаивем bat-ник
```
C:/Sonar-scanner/bin/sonar-scanner.bat -D"sonar.projectKey=FirstProject" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.login=9cbb77e3a1c204194aac91c03883ee83ce746507"
```
13. Настраиваем sonar-project.properties
```
sonar.bsl.skipVendorSupportedObjects=true
```

14. Запускаем `check.bat`. ==ВАЖНО! запуск должен производиться из каталога в котором bat-файл.==

### Troubleshooting
https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/

````
**Java heap space error or java.lang.OutOfMemoryError**  
Increase the memory via the `SONAR_SCANNER_OPTS` environment variable when running the scanner from a zip file:

```
export SONAR_SCANNER_OPTS="-Xmx512m"
```

In Windows environments, avoid the double-quotes, since they get misinterpreted and combine the two parameters into a single one.

```
set SONAR_SCANNER_OPTS=-Xmx512m
```

**Unsupported major.minor version**  
Upgrade the version of Java being used for analysis or use one of the native package (that embed its own Java runtime).

**Property missing: `sonar.cs.analyzer.projectOutPaths'. No protobuf files will be loaded for this project.**  
Scanner CLI is not able to analyze .NET projects. Please, use the SonarScanner for .NET. If you are running the SonarScanner for .NET, ensure that you are not hitting a known limitation.
````