# operator-연산자

기본 자료형을 제외한 참조자료형중 String만 + 연산이 가능하다.  
이유는 컴파일 시점에 + 연산자가 자바 버전에 따라 String 연산을 하는 것으로 변환되기 때문이다.

## Arithmetic Operators
+ 우선순위[3] **`*`** (곱셈)
    ```java
    int result = 5 * 3; // result는 15
    ```
+ 우선순위[3] **`/`** (나눗셈)
    ```java
    int result = 10 / 2; // result는 5
    ```
+ 우선순위[3] **`%`** (나머지)
    ```java
    int result = 10 % 3; // result는 1
    ```
+ 우선순위[4] **`+`** (덧셈)
    ```java
    int result = 5 + 3; // result는 8
    ```
+ 우선순위[4] **`-`** (뺄셈)
    ```java
    int result = 5 - 3; // result는 2
    ```

## Comparison Operators
+ 우선순위[6] **`>`** (크다)
    ```java
    boolean result = 5 > 3; // result는 true
    ```
+ 우선순위[6] **`<`** (작다)
    ```java
    boolean result = 5 < 3; // result는 false
    ```
+ 우선순위[6] **`>=`** (크거나 같다)
    ```java
    boolean result = 5 >= 3; // result는 true
    ```
+ 우선순위[6] **`<=`** (작거나 같다)
    ```java
    boolean result = 5 <= 3; // result는 false
    ```
+ 우선순위[7] **`==`** (같음)
    ```java
    boolean result = 5 == 5; // result는 true
    ```
+ 우선순위[7] **`!=`** (같지 않음)
    ```java
    boolean result = 5 != 3; // result는 true
    ```

## Logical Operators
+ 우선순위[2] **`!`** (논리 NOT)
    ```java
    boolean result = !(5 > 3); // result는 false
    ```
+ 우선순위[11] **`&&`** (논리 AND)
    ```java
    boolean result = (5 > 3) && (3 > 1); // result는 true
    ```
+ 우선순위[12] **`||`** (논리 OR)
    ```java
    boolean result = (5 > 3) || (3 < 1); // result는 true
    ```

## Bitwise Operators
+ 우선순위[2] **`~`** (비트 NOT)
    ```java
    int result = ~5; // result는 -6
    ```
+ 우선순위[5] **`<<`** (왼쪽 시프트)
    ```java
    int result = 5 << 1; // result는 10
    ```
+ 우선순위[5] **`>>`** (오른쪽 시프트)
    ```java
    int result = 5 >> 1; // result는 2
    ```
+ 우선순위[5] **`>>>`** (논리 오른쪽 시프트)
    ```java
    int result = -5 >>> 1; // result는 2147483645
    ```
+ 우선순위[8] **`&`** (비트 AND)
    ```java
    int result = 5 & 3; // result는 1
    ```
+ 우선순위[9] **`^`** (비트 XOR)
    ```java
    int result = 5 ^ 3; // result는 6
    ```
+ 우선순위[10] **`|`** (비트 OR)
    ```java
    int result = 5 | 3; // result는 7
    ```

## Assignment Operators
+ 우선순위[13] **`=`** (할당)
    ```java
    int num = 5;
    ```
+ 우선순위[13] **`+=`** (덧셈 후 할당)
    ```java
    num += 3; // num은 8
    ```
+ 우선순위[13] **`-=`** (뺄셈 후 할당)
    ```java
    num -= 3; // num은 2
    ```
+ 우선순위[13] **`*=`** (곱셈 후 할당)
    ```java
    num *= 3; // num은 15
    ```
+ 우선순위[13] **`/=`** (나눗셈 후 할당)
    ```java
    num /= 5; // num은 1
    ```
+ 우선순위[13] **`%=`** (나머지 후 할당)
    ```java
    num %= 5; // num은 0
    ```