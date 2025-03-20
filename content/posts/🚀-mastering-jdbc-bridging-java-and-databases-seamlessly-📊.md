---
title: "🚀 Mastering JDBC: Bridging Java and Databases Seamlessly 📊"
date: 2025-01-03
categories: 
  - "general"
---

## JDBC IN JAVA

> JDBC (Java Database Connectivity) is a key API in Advanced Java that allows Java applications to interact with databases. It provides a standard way to connect to a database, execute SQL queries, and retrieve results. Below is a comprehensive overview:

## Key Components of JDBC

1. DriverManager:
    
    > Manages a list of database drivers and establishes a connection between the application and the database.
    
2. Connection:
    
    > Represents the session between the Java application and the database.
    
3. Statement:
    
    > Executes static SQL queries against the database.
    
4. PreparedStatement:
    
    > Used to execute parameterized SQL queries, improving security and performance.
    
5. CallableStatement:
    
    > Used for executing stored procedures in the database.
    
6. ResultSet:
    
    > Represents the results of a query, allowing navigation through rows of data.
    

## Steps to Use JDBC

1. Load the Driver:

```
Class.forName("com.mysql.cj.jdbc.Driver"); // Load MySQL Driver
```

1. Establish a Connection:

```
Connection con = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/yourDatabaseName", "username", "password");
```

1. Create a Statement:

```
Statement stmt = con.createStatement();
```

1. Execute a Query:

```
ResultSet rs = stmt.executeQuery("SELECT * FROM yourTableName");
while (rs.next()) {
    System.out.println(rs.getString("columnName"));
}
```

1. Close the Connection:

```
rs.close();
stmt.close();
con.close();
```

## Advantages of JDBC

- Platform Independence: Works on any platform with a supported Java environment.
- Supports Multiple Databases: Compatible with various databases like MySQL, Oracle, PostgreSQL, etc.
- Dynamic SQL Execution: Provides flexibility to execute both static and dynamic SQL.
- Database Metadata: Allows retrieval of database and table structure.

## Types of JDBC Drivers

1. Type 1: JDBC-ODBC Bridge Driver (Deprecated)
2. Type 2: Native-API Driver
3. Type 3: Network Protocol Driver
4. Type 4: Thin Driver (Pure Java driver, widely used)

## Common JDBC Operations

1. Insert Data:

```
String query = "INSERT INTO employees (id, name) VALUES (?, ?)";
PreparedStatement pstmt = con.prepareStatement(query);
pstmt.setInt(1, 101);
pstmt.setString(2, "John Doe");
pstmt.executeUpdate();
```

1. Update Data:

```
String query = "UPDATE employees SET name = ? WHERE id = ?";
PreparedStatement pstmt = con.prepareStatement(query);
pstmt.setString(1, "Jane Doe");
pstmt.setInt(2, 101);
pstmt.executeUpdate();
```

1. Delete Data:

```
String query = "DELETE FROM employees WHERE id = ?";
PreparedStatement pstmt = con.prepareStatement(query);
pstmt.setInt(1, 101);
pstmt.executeUpdate();

```

## Example Program

```
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/yourDatabase";
        String user = "root";
        String password = "password";

        try (Connection con = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = con.prepareStatement("SELECT * FROM yourTable")) {

            ResultSet rs = pstmt.executeQuery();
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("id") + ", Name: " + rs.getString("name"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## ✨ More update to connect:

GitHub  
Linkedin

Go to Source
