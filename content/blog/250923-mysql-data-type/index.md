---
title: "MySQL 데이터 타입"
description: "MySQL의 데이터 타입은 어떤것들이 있는지 기록"
date: "2025-09-23T13:12:24"
thumbnail: "./thumbnail.png"
category: "Database"
tags: ["SQL", "MySQL"]
---

# MySQL 데이터 타입

`MySQL` 에는 다음과 같은 데이터 타입이 있습니다.

| 데이터 타입 | 설명             |
| ----------- | ---------------- |
| CHAR        | 고정 길이 문자열 |
| VARCHAR     | 가변 길이 문자열 |
| ENUM        | 열거형           |
| SET         | 집합             |
| TEXT        | 텍스트           |
| BLOB        | 바이너리 데이터  |
| DATE        | 날짜             |
| INT         | 정수             |
| BOOLEAN     | 불리언           |
| DECIMAL     | 고정 소수점      |
| FLOAT       | 부동소수점       |
| TIMESTAMP   | 날짜와 시간      |
| DATETIME    | 날짜와 시간      |

## CHAR

`CHAR` 는 고정 길이 문자열을 저장하는 데이터 타입입니다.
최대 255자까지 저장할 수 있습니다.

```sql
CREATE TABLE movies (
  title CHAR(10)
);
```

`title` 이 만약 10글자보다 적은 경우에는 공백으로 빈자리를 채웁니다.

## VARCHAR

VARCHAR는 가변 길이 문자열을 저장하는 데이터 타입입니다.

```sql
CREATE TABLE movies (
  contents VARCHAR(100)
);
```

`CHAR` 와 다르게 `contents` 가 100글자보다 적은 경우에는 빈 공간을 공백으로 채우지 않고, `contents` 의 길이만큼만 저장합니다.

## ENUM

`ENUM` 은 열거형을 저장하는 데이터 타입입니다.

```sql
CREATE TABLE movies (
  rating ENUM('G', 'PG', 'R')
);
```

`rating` 은 `G`, `PG`, `R` 중 <span style="color:orange">**하나의 값**</span>만 저장할 수 있습니다.

## SET

`MySQL` 의 `SET` 데이터타입은 미리 정의된 값들의 집합에서 0개 이상의 값을 선택할 수 있는 문자열 객체입니다.

`SET` 은 `ENUM` 과 유사하지만, `ENUM`이 하나의 값만 선택할 수 있는 반면 `SET` 은 여러 개의 값을 동시에 선택할 수 있습니다. 내부적으로는 비트마스크를 사용하여 각 값을 저장합니다.

```sql
CREATE TABLE user_interests (
  hobbies SET('reading', 'sports', 'music', 'travel', 'cooking')
);
```

`hobbies` 는 `reading`, `sports`, `music`, `travel`, `cooking` 중 하나 또는 여러 개의 값을 저장할 수 있습니다.

## TEXT

`TEXT` 는 텍스트를 저장하는 데이터 타입입니다.

`TEXT` 타입은 또 종류가 나뉩니다. 아래와 같이 총 4가지가 있습니다.

| 데이터 타입 | 설명                 |
| ----------- | -------------------- |
| TINYTEXT    | 최대 255자           |
| TEXT        | 최대 65,535자        |
| MEDIUMTEXT  | 최대 16,777,215자    |
| LONGTEXT    | 최대 4,294,967,295자 |

근데 위 종류를 보면 그냥 LONGTEXT를 사용하면 되지 않나? 라는 의문이 들 수 있습니다.

그러나 데이터베이스가 처리하는 방식의 차이때문에 (성능과 관련) 최대한 용도에 맞는 타입 설정이 중요합니다.

## BLOB

`BLOB (Binary Large Object)` 데이터타입은 이진 데이터를 저장하기 위한 데이터타입입니다.

MySQL은 크기에 따라 4가지 BLOB 타입을 제공합니다.

| 데이터 타입 | 최대 길이                 |
| ----------- | ------------------------- |
| TINYBLOB    | 255 bytes                 |
| BLOB        | 65,535 bytes (64KB)       |
| MEDIUMBLOB  | 16,777,215 bytes (16MB)   |
| LONGBLOB    | 4,294,967,295 bytes (4GB) |

## INT

`INT` 는 정수를 저장하는 데이터 타입입니다.

크기에 따라 5가지 정수형 타입을 제공합니다.

| 데이터 타입 | 바이트 | 범위(SIGNED)                                           | 범위(UNSIGNED)                 |
| ----------- | ------ | ------------------------------------------------------ | ------------------------------ |
| TINYINT     | 1      | -128 ~ 127                                             | 0 ~ 255                        |
| SMALLINT    | 2      | -32,768 ~ 32,767                                       | 0 ~ 65,535                     |
| MEDIUMINT   | 3      | -8,388,608 ~ 8,388,607                                 | 0 ~ 16,777,215                 |
| INT         | 4      | -2,147,483,648 ~ 2,147,483,647                         | 0 ~ 4,294,967,295              |
| BIGINT      | 8      | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 | 0 ~ 18,446,744,073,709,551,615 |

```sql
column_name INT

column_name INT UNSIGNED
```

기본적으로 위와 같이 사용하는데, 기본값은 SIGNED 입니다.

## BOOLEAN

MySQL에서 BOOLEAN은 실제로는 TINYINT(1)의 별칭입니다.

진정한 불린 타입이 아닌 정수 타입으로 구현되어 있습니다.

TRUE, FALSE 대신 1, 0을 사용합니다.

```sql
-- 모두 동일한 의미
column_name BOOLEAN
column_name BOOL
column_name TINYINT(1)
```

## DECIMAL

`DECIMAL` 은 고정소수점(Fixed-Point) 숫자를 정확하게 저장하는 데이터타입입니다.

부동소수점과 달리 정확한 계산이 가능하여 금융 데이터나 정밀한 수치 계산에 필수적입니다.

```sql
-- 다양한 DECIMAL 정의
CREATE TABLE decimal_examples (
    id INT PRIMARY KEY,
    price DECIMAL(10,2),        -- 99999999.99까지 (총 10자리, 소수점 2자리)
    percentage DECIMAL(5,4),    -- 9.9999까지 (총 5자리, 소수점 4자리)
    huge_number DECIMAL(65,30), -- 최대 크기
    simple_decimal DECIMAL      -- DECIMAL(10,0)과 동일
);
```

## FLOAT

FLOAT는 부동소수점(Floating-Point) 숫자를 저장하는 데이터타입입니다.

IEEE 754 표준을 따르며, 매우 큰 범위의 수를 근사치로 저장할 수 있지만 정확도에 제한이 있습니다.

```sql
-- 단정밀도 부동소수점
FLOAT           -- 4바이트, 약 7자리 정밀도
FLOAT(p)        -- p는 정밀도 (0-24: FLOAT, 25-53: DOUBLE)

-- 배정밀도 부동소수점
DOUBLE          -- 8바이트, 약 15자리 정밀도
DOUBLE PRECISION -- DOUBLE과 동일
REAL            -- DOUBLE의 별칭 (sql_mode에 따라 FLOAT일 수도 있음)
```

| 데이터 타입 | 바이트 | 범위                                                | 정밀도    |
| ----------- | ------ | --------------------------------------------------- | --------- |
| FLOAT       | 4      | ±1.175494351E-38 ~ ±3.402823466E+38                 | 약 7자리  |
| DOUBLE      | 8      | ±2.2250738585072014E-308 ~ ±1.7976931348623157E+308 | 약 15자리 |

## TIMESTAMP, DATETIME

`TIMESTAMP` 와 `DATETIME` 은 날짜와 시간을 저장하는 데이터 타입입니다.

| 특성          | TIMESTAMP                                     | DATETIME                                  |
| ------------- | --------------------------------------------- | ----------------------------------------- |
| 저장 공간     | 4바이트                                       | 8바이트                                   |
| 범위          | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 UTC | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 |
| 시간대 처리   | UTC로 저장, 조회시 변환                       | 입력값 그대로 저장                        |
| 자동 업데이트 | 지원                                          | 지원 (MySQL 5.6+)                         |

```sql
-- TIMESTAMP 선언
column_name TIMESTAMP [DEFAULT value] [ON UPDATE CURRENT_TIMESTAMP]

-- DATETIME 선언
column_name DATETIME [DEFAULT value] [ON UPDATE CURRENT_TIMESTAMP]

-- 마이크로초 지원 (MySQL 5.6+)
column_name TIMESTAMP(6)  -- 마이크로초까지
column_name DATETIME(3)   -- 밀리초까지

```

그럼 각각을 언제 써야하나요?

- DATETIME : 1000년부터 9999년 사이의 날짜를 저장하고 싶으면 사용
- TIMESTAMP : 시스템 이벤트 저장 시 사용하기 적절 (타임존 고려, 저장공간도 DATETIME에 비하여 덜 차지)

외에도 DATE, TIME, YEAR 도 있습니다.

### DATE, TIME, YEAR

- DATE : 날짜를 저장하고 싶으면 사용
- TIME : 시간을 저장하고 싶으면 사용
- YEAR : 년도를 저장하고 싶으면 사용

```sql
column_name DATE
column_name TIME
column_name YEAR
```

<hr/>

외에도 JSON, GEOMETRY, ENUM, SET 등 다양한 데이터 타입이 있습니다.

자세한 내용은 [MySQL 공식 문서](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)를 참고해주세요.
