

## Connecting to a MySQL Server
- Connect to MySQL server with a username and password:
  ```
  mysql -u [username] -p
  ```

## Creating and Displaying Databases
- Create a database:
  ```
  CREATE DATABASE zoo;
  ```
- List all databases on the server:
  ```
  SHOW DATABASES;
  ```
- Use a specified database:
  ```
  USE zoo;
  ```
- Delete a specified database:
  ```
  DROP DATABASE zoo;
  ```

## Creating Tables
- Create a table:
  ```
  CREATE TABLE animals (id INT, name VARCHAR(50));
  ```

## Modifying Tables
- List all tables in the database:
  ```
  SHOW TABLES;
  ```
- Get information about a specified table:
  ```
  DESCRIBE animal;
  ```

## Querying Data
- Select data from a table:
  ```
  SELECT * FROM animals;
  ```

## Inserting Data
- Insert data into a table:
  ```
  INSERT INTO animals (id, name) VALUES (1, 'Lion');
  ```

## Updating Data
- Update data in a table:
  ```
  UPDATE animals SET name = 'Tiger' WHERE id = 1;
  ```

## Deleting Data
- Delete data from a table:
  ```
  DELETE FROM animals WHERE id = 1;
  ```

## Text Functions
- Concatenate strings:
  ```
  SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
  ```

## Numeric Functions
- Calculate average:
  ```
  SELECT AVG(salary) FROM employees;
  ```

## Date and Time Functions
- Extract parts of dates:
  ```
  SELECT EXTRACT(YEAR FROM hire_date) AS hire_year FROM employees;
  ```

## BY ME  

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

**Show validate password settings**  

```sql  
SHOW VARIABLES LIKE 'validate_password%';
``` 

**How to edit "validate_password" settings"**  

```sql  
SET GLOBAL validate_password.length = 6;
SET GLOBAL validate_password.number_count = 0;
SET GLOBAL validate_password.policy = LOW;

```
