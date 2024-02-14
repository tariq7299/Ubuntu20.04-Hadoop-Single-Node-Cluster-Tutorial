**Alter root password**  
```SQL
sudo mysql -u root
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your_new_password';
FLUSH PRIVILEGES; # Reload privileges tables
```  
**Install mysql server**
```bash
sudo apt install mysql-server  
```  

**Check for mysql service**  

```bash
sudo systemctl status mysql.service 
``` 

**start my sql server**  
```bash 
sudo systemctl start mysql.service 
```

**Configure mysql secure installation after installing Mysql**  
```bash  
sudo mysql_secure_installation
```  

**Creat a user in mysql**  
```sql
sudo mysql -u root -exitp
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password12';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
CREATE DATABASE ranger;
```  




