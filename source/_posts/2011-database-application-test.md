title: 2011级《数据库应用技术》期末考试试题（A卷）答案与题目
date: 2015-12-14 21:39:45
categories:
- 吉林大学
tags:
- 考试
- 数据库应用
---

## 2011级《数据库应用技术》期末考试试题（A卷）答案

### 判断题

- 1、错，NULL不会被索引
- 2、错，创建主键的时候会自动生成同名的唯一索引，创建唯一键的时候也会自动创建同名唯一索引，但是一张表最多只能有一个主键，但可以有多个唯一键。
- 3、对
- 4、对
- 5、对

### 简单题A

- 1、当前主流的数据库模型是：关系型数据库
    - Mysql
    - Oracle
    - SQL server
- 2、答案如下：
    - “虚表”与“实表”的区别在于：
        - “虚表”（view）不存储数据，使用时只是对原有数据重新组织，而“实表”真实存储数据
        - “虚表”（view）只是一个逻辑结构，不占存储空间
        - DML在复杂“虚表”上不一定能执行，而DML完全可以在“实表”上运行

    - 适用于“虚表”而不适合于“实表”的情况：对于低等级的用户隐藏用户表中的部分信息，这时候可以只赋予低等级用户读“虚表”的权限，而“虚表”此时已经隐藏了用户表中的部分信息，并且禁止低等级用户读用户表。

- 3、答案如下：
    - 常见的并发异常有：
        - 脏读
        - 脏写
        - 不可重复读
        - 幻影读

    - 常见的并发隔离级别由低到高分别是：
        - Read uncommitted
        - Read committed
        - Repeatable read
        - Serializable


- 4、角色是一组权限的集合，用户可以属于不同的角色，从而拥有不同的权限。角色可以降低权限、用户管理工作，实现动态的权限管理。
- 5、使用通用的数据库接口增强了程序的通用性、灵活性、可维护性，降低程序员的学习难度和减少了开发周期。
    - 常见了通用数据库接口有：
        - ODBC
        - JDBC
        - DAO


- 6、不同的分类：
    - 分类一：
        - 逻辑备份：将数据库的内容备份到一个文件中，这个文件的物理结构与数据库并不相同
        - 物理备份：拷贝构成数据库的文件，而不管其中的逻辑内容
    - 分类二：
        - 冷备份：在数据库脱机时进行备份
        - 热备份：在数据库联机时进行备份
    - 分类三：
        - 全量备份：备份所有
        - 增量备份：只备份上次以来的变化部分

### 应用题A

- 1、
```
CREATE TABLE BOOKS
(
BOOK_ID NUMBER(10) NOT NULL,
BNAME VARCHAR2(200) NOT NULL，
PUBLISHER VARCHAR2(100) NULL,
AUTHORS VARCHAR2(200) NULL,
PRICE NUMBER(6,2) NOT NULL,
DATE_PUBLISH DATE NULL,
PRIMARY KEY(BOOK_ID),
CHECK(PRICE > 0)
)
```
- 2、
```
CREATE UNIQUE INDEX BOOK_ID
ON BOOKS (BOOK_ID)
```
- 3、
```
CREATE SEQUENCE SEQ_BOOKS
INSERT INTO BOOKS
(BOOK_ID, BNAME, PUBLISHER, AUTHORS, PRICE, DATE_PUBLISH)
VALUES
(SEQ_BOOKS.CURRVAL, '测试', '测试', '测试', 1， SYSDATE);
```
- 4、不可理之处在于：`BOOK_ORDERS`的`COURSE_ID`与`STUD_NO`应该分别设置为指向`COURSES`表和`STUDENTS`表的外键，保证数据库的完整性。


### 应用题B
- 1、
```
SELECT * FROM BOOKS
WHERE AUTHORS = '王磊' AND PRICE <= 30;
```
- 2、
```
SELECT S.SNAME
FROM BOOK_ORDERS BO, COURSES C, BOOKS B, STUDENTS S
WHERE BO.COURSE_ID = C.COURSE_ID AND C.BOOK_ID = B.BOOK_ID AND BO.STUD_NO = S.STUD_NO AND B.BNAME = '计算机网络' AND C.SCHOOL = '计算机'
ORDER BY S.GRADE, S.STUD_NO;
```
- 3、
```
SELECT COUNT, PRICE
FROM (SELECT COUNT(BO.IS_BOOK) AS COUNT
FROM BOOK_ORDERS BO, COURSES C, BOOKS B, STUDENTS S
WHERE BO.COURSE_ID = C.COURSE_ID AND C.BOOK_ID = B.BOOK_ID AND BO.STUD_NO = S.STUD_NO AND S.STUD_NO = '53080101'),
(SELECT SUM(B.PRICE) AS PRICE
FROM BOOK_ORDERS BO, COURSES C, BOOKS B, STUDENTS S
WHERE BO.COURSE_ID = C.COURSE_ID AND C.BOOK_ID = B.BOOK_ID AND BO.STUD_NO = S.STUD_NO AND S.STUD_NO = '53080101' AND BO.IS_PAID = 'Y')
```
- 4、
```
SELECT S.SNAME, S.STUD_NO
FROM STUDENTS S
WHERE S.STUD_NO NOT IN
(
	SELECT DISTINCT BO.STUD_NO
	FROM BOOK_ORDERS BO
	WHERE BO.IS_BOOK IS NULL
)
```
- 5、
```
SELECT B1.BNAME, B1.BOOK_ID
FROM BOOKS B1
WHERE
(
	SELECT COUNT(*)
	FROM BOOKS B2, COURSES C
	WHERE C.BOOK_ID = B2.BOOK_ID AND B1.BOOK_ID = B2.BOOK_ID
) > 1
```
- 6、
```
SELECT *
FROM
(
	SELECT *
	FROM BOOKS B
	ORDER BY B.PRICE DESC
)
WHERE ROWNUM <= 5
```

### 应用题C

```
CREATE OR REPLACE
FUNCTION "CALL_BILL" (V_PUBLISHER IN VARCHAR2)
RETURN NUMBER
IS
	V_SUM NUMBER DEFAULT 0;
	V_SUM_TEMP NUMBER DEFAULT 0;
	V_COUNT NUMBER DEFAULT 0;
	V_NUM NUMBER DEFAULT 0;
	CURSOR CUR(PUBLISHER VARCHAR2) IS
		SELECT SUM(B.PRICE)
		FROM BOOKS B, BOOK_ORDERS BO, COURSES C
		WHERE C.COURSE_ID = BO.COURSE_ID AND C.BOOK_ID = B.BOOK_ID AND B.PUBLISHER = '123'
		GROUP BY B.BOOK_ID
		ORDER BY COUNT(*) DESC;

BEGIN
	OPEN CUR(V_PUBLISHER);

	LOOP
		FETCH CUR INTO V_NUM;
		EXIT WHEN CUR%NOTFOUND;
		V_COUNT := V_COUNT + 1;
		V_SUM_TEMP := V_SUM_TEMP + V_NUM;
		IF V_COUNT = 10 THEN
			V_SUM_TEMP := V_SUM_TEMP * 0.7;
			V_SUM := V_SUM_TEMP;
			V_SUM_TEMP := 0;
		END IF;
	END LOOP;
	CLOSE CUR;

	IF V_COUNT = 0 THEN
		RAISE_APPLICATION_ERROR(-20010, 'NO DATA');
	END IF;

	V_SUM := V_SUM + V_SUM_TEMP * 0.8;

	RETURN V_SUM;
END;
```

### 简答题B

关注数据库应用系统的安全，我们可以从以下方面入手：
- 硬件的安全：服务器、工作站和网络的安全防范
- 数据库安全：定期做审查与备份，保证数据完整可靠；设计数据库时候应该完善约束条件，保证数据的完整和一致性
- 软件安全：系统、DBMS和开发的应用程序

## 2011级《数据库应用技术》期末考试试题（A卷）题目

本地下载地址：{% asset_link Test.zip 考试题目 %}