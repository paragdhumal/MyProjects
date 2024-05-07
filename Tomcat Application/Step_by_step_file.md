Host a Tomcat server in an instance and attach MariaDB to it (2 tier application).

1. **Launch an EC2 instance in AWS with general configurations. Open port 22, 3306 and 8080 in Security group.**
2. **Take access of your instance via. ssh through Putty or MobaXterm.**
3. **Install Java on the system.**

```bash
yum install java -y
```
4. **Now go to Google and type 'Tomcat 8 Download', go to the first link and copy the tar.gz link from Core. Now, in your system put the command and press enter.**

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.100/bin/apache-tomcat-8.5.100.tar.gz
```
5. **The file is zipped. Unzip by command (tar -xzvf <file name>). Now, go inside the unzipped folder named "apache-tomcat-8.5.97".**

6. **Now upload your development file eg. .war file and sql connector file .jar file to your instance through MobaXterm or scp command.**

7. **Then move .war file from the uploaded location to webapps/ folder in "apache-tomcat-8.5.97". Similarly, move .jar file to lib/ folder in "apache-tomcat-8.5.97".**

8. **Now go to folder bin/ folder and run the catalina.sh script.**

```bash
./catalina.sh start
```

9. **Now copy your public IP address of instance and paste it on browser with port 8080. eg. 10.0.0.0:8080. Your Tomcat application should be running. Make sure you have added the 8080 port in Security group Inbound rules.**

10. **Now that the Tomcat is running, we need to attach MariaDB database. So, go to your terminal and install MariaDB.**

```bash
yum install mariadb105-server -y
```

11. **Then start the MariaDB service.**

```bash
systemctl start mariadb
systemctl enable mariadb
```
 
12. **Now put the command 'mysql_secure_installation'. Configure new root password and put 'y' for other options.**

13. **Now logon to MariaDB, put command 'mysql -h localhost -u root -p' then put password after executing this command.**
 
14. **Now once you go into MariaDB, put command 'show databases;' and then create database 'create database <database name>'. this will create a new database for your project.**
 
15. **Then execute the command 'use <database name>;' to enter the database.**

16. **Create a table from the following query (Query is for reference purpose only).**

```bash
CREATE TABLE if not exists students (studentid INT NOT NULL AUTO_INCREMENT,
    student_name VARCHAR(100) NOT NULL,
    student_addr VARCHAR(100) NOT NULL,
    student_age VARCHAR(3) NOT NULL,
    student_qual VARCHAR(20) NOT NULL,
    student_percent VARCHAR(10) NOT NULL,
    student_year_passed VARCHAR(10) NOT NULL,
    PRIMARY KEY (student_id)
);

```

17. **Next part is open context.xml file in conf folder and copy the following lines into it at line no 21.**

```bash
 <Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
  maxTotal="100" maxIdle="30" maxWaitMillis="10000"
  username="USERNAME" password="PASSWORD" driverClassName="com.mysql.jdbc.Driver"
  url="jdbc:mysql://DB-ENDPOINT:3306/DATABASE"/>
```


** Note few changes to make are
username=root , password=1234 ,
url="jdbc:mysql://localhost:3306/database name"**
 
** FOR EXAMPLE:-
<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
maxTotal="100" maxIdle="30" maxWaitMillis="10000"
username="root" password="1234" driverClassName="com.mysql.jdbc.Driver"
url="jdbc:mysql://localhost:3306/studentapp"/>**

** Now, to view entries in databases
SHOW DATABASES; -- gives you list of available databases
SHOW TABLES; -- list tables in a specific database
DESCRIBE TABLE_NAME -- Views you schema
SELECT * FROM table_name; -- view data in table
SELECT * FROM table_name WHERE id > 10;
SELECT * FROM table_name LIMIT 10; limiting.**

18. **After creating table and copying the content in context.xml file, go to your application in browser and add the data on Tomcat server and that data should reflect in your database. (See the attached photo for reference).**





