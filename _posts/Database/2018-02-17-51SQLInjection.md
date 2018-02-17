---
layout: post
title:  "SQL 강좌 51. SQL Injection"
date:   2018-02-17 12:25:00 +0900
categories: database
---

## SQL Injection

- SQL injection은 데이터베이스를 파괴할 수 도 있는 코드 주입 기술이다.  
- 흔한 웹 해킹 기술 중의 하나  
- 웹 페이지 입력을 통해 SQL문에 악의적인 코드를 배치할 수 있다.

## SQL in Web Pages

SQL injection은 흔히 유저에게 username/userId와 같은 input을 요청할 때 발생하며 유저가 이름이나 ID를 입력하지 않고 SQL 구문을 입력함으로써 나도 모르는 사이 데이터베이스에서 작동하게함.


**Example**

```sql
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```

위 예제가 얼마나 잠재적인 위험을 갖고 있는지 설명할 것임

## SQL Injection Based on 1=1 is Always True

위 예제는 주어진 UserID를 통해 유저 정보를 가져오는 구문이다. 하지만 유저가 잘못된 값을 입력할 경우 그것을 방지할 방법은 없다. 

만약에 유저가  

**UserId : 105 OR 1=1** 
 
와 같은 방법으로 입력을 하게된다면  
SQL구문은 다음과 같이 생성될 것이다

```sql
SELECT * FROM Users WHERE UserId = 105 OR 1=1;
```

위 SQL구문에서 1=1은 항상 True이기 때문에 모든 레코드에 대한 정보를 가져오게 될 것이다.

즉, 해커는 단순하게 `105 OR 1=1`를 입력함으로써 모든 유저들의 이름과 비밀번호를 알게되는 것이다.

## SQL Injection Based on ""="" is Always True

웹사이트 로그인 시

**Example**

```sql
uName = getRequestString("username");
uPass = getRequestString("userpassword");

sql = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
```

**uName :** John Doe  
**uPass :** myPass  

를 입력했다고 가정했을 때 결과는 다음과 같을 것이다.

```sql
SELECT * FROM Users WHERE Name ="John Doe" 
AND Pass="myPass"
```

만약에 해커가 `" OR ""="`을 입력한다면 어떻게 될까?

```sql
SELECT * FROM Users WHERE Name ="" or ""="" AND Pass ="" or ""=""
```

`OR ""=""`는 항상 참이므로 모든 레코드들을 반환하는 구문을 생성할 것이다

## SQL Injection Based on Batched SQL Statements 

아래와 같은 예제에서 

```sql
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```

다음과 같은 입력을 받았다고 가정해보자

UserId : 105; DROP TABLE Suppliers

그럼 다음과 같은 SQL구문이 완성될 것이다. 

```sql
SELECT * FROM Users WHERE UserId = 105; 
DROP TABLE Suppliers;
```

모든 정보를 캐낸 후에 테이블을 삭제해버린다.

## Use SQL Parameters for Protection

SQL injection으로부터 보호하기위해 SQL 매개변수들을 사용하면 된다.

SQL 매개변수들은 실행 타임에 SQL쿼리가 추가되는 값들이다.

**ASP.NET Razor Example**

```sql
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = @0";
db.Execute(txtSQL,txtUserId);
```

매개 변수들이 SQL문에서 @표시가 된다

SQL엔진은 각 매개변수들가 해당 column에 대해 올바른지 확인하고 문자 그대로 처리되며 실행될 SQL의 일부가 아닌지 확인한다.

**Another Example**

```sql
txtNam = getRequestString("CustomerName");
txtAdd = getRequestString("Address");
txtCit = getRequestString("City");
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
db.Execute(txtSQL,txtNam,txtAdd,txtCit);
```

## Examples

다음 예들은 일부 공통 웹 언어로 매개 변수화 된 쿼리를 작성하는 방법을 보여줌

**SELECT STATEMENT IN ASP.NET:**

```sql
txtUserId = getRequestString("UserId");
sql = "SELECT * FROM Customers WHERE CustomerId = @0";
command = new SqlCommand(sql);
command.Parameters.AddWithValue("@0",txtUserID);
command.ExecuteReader();
```

**INSERT INTO STATEMENT IN ASP.NET:**

```sql
txtNam = getRequestString("CustomerName");
txtAdd = getRequestString("Address");
txtCit = getRequestString("City");
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
command = new SqlCommand(txtSQL);
command.Parameters.AddWithValue("@0",txtNam);
command.Parameters.AddWithValue("@1",txtAdd);
command.Parameters.AddWithValue("@2",txtCit);
command.ExecuteNonQuery();
```

**INSERT INTO STATEMENT IN PHP:**

```php
$stmt = $dbh->prepare("INSERT INTO Customers (CustomerName,Address,City) 
VALUES (:nam, :add, :cit)");
$stmt->bindParam(':nam', $txtNam);
$stmt->bindParam(':add', $txtAdd);
$stmt->bindParam(':cit', $txtCit);
$stmt->execute();
```