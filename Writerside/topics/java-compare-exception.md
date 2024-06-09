# 자바 Exception  

## 자바 예외 처리 메커니즘

자바 예외(Java Exception)는 프로그램 실행 중에 발생할 수 있는 오류의 일종으로, 프로그램의 정상적인 흐름을 방해하거나 중단시킬 수 있는 예외적인 상황을 의미합니다. 자바에서는 Exception 클래스를 상속받은 클래스들을 통해 예외 상황을 처리하고, `try-catch` 블록을 통해 예외를 처리할 수 있습니다.

## 자바 예외의 주요 개념

1. **예외 클래스 계층 구조**
    - 자바의 모든 예외 클래스는 `java.lang.Throwable` 클래스를 상속합니다.
    - `Throwable` 클래스의 두 가지 주요 서브클래스가 있습니다:
        - `Exception`: 대부분의 예외가 이 클래스를 상속합니다.
        - `Error`: 주로 JVM에서 발생하는 심각한 오류를 나타내며, 일반적으로 프로그램에서 처리하지 않습니다.

2. **체크 예외(Checked Exception)**
    - 컴파일 시점에 체크되는 예외입니다.
    - 반드시 예외 처리를 해야 하며, 그렇지 않으면 컴파일 오류가 발생합니다.
    - 예: `IOException`, `SQLException`.

3. **언체크 예외(Unchecked Exception)**
    - 런타임 시점에 발생하는 예외로, 컴파일러가 체크하지 않습니다.
    - 예외 처리가 강제되지 않으며, 주로 프로그래머의 실수에 의해 발생합니다.
    - 예: `NullPointerException`, `ArrayIndexOutOfBoundsException`.

4. **에러(Error)**
    - 심각한 시스템 레벨의 문제를 나타내며, 일반적으로 프로그램에서 처리하지 않습니다.
    - 예: `OutOfMemoryError`, `StackOverflowError`.

## 예외 처리 방법

자바에서는 예외를 처리하기 위해 `try`, `catch`, `finally` 블록을 사용합니다.

1. **try 블록**
    - 예외가 발생할 가능성이 있는 코드를 포함합니다.

2. **catch 블록**
    - `try` 블록에서 예외가 발생하면 해당 예외를 처리합니다.
    - 여러 개의 `catch` 블록을 사용할 수 있으며, 각 블록은 다른 유형의 예외를 처리할 수 있습니다.

3. **finally 블록**
    - 예외 발생 여부와 관계없이 항상 실행되는 코드를 포함합니다.
    - 주로 자원 해제(파일 닫기, 데이터베이스 연결 종료 등)에 사용됩니다.

```java
try {
    // 예외가 발생할 수 있는 코드
} catch (ExceptionType1 e1) {
    // ExceptionType1 예외 처리
} catch (ExceptionType2 e2) {
    // ExceptionType2 예외 처리
} finally {
    // 항상 실행되는 코드
}
```

## 예외 던지기

예외를 명시적으로 발생시키기 위해 `throw` 키워드를 사용합니다. 또한, 메서드 시그니처에 `throws` 키워드를 사용하여 메서드가 특정 예외를 던질 수 있음을 선언할 수 있습니다.

```java
public void exampleMethod() throws IOException {
    if (/* some condition */) {
        throw new IOException("예외 메시지");
    }
}
```

## 사용자 정의 예외

사용자 정의 예외를 만들기 위해서는 `Exception` 클래스를 상속하면 됩니다.

```java
public class MyCustomException extends Exception {
    public MyCustomException(String message) {
        super(message);
    }
}
```

## Checked Exception과 Unchecked Exception의 차이점

### 체크

1. **정의 및 특징**:
    - Checked Exception은 컴파일 시점에 체크되는 예외입니다.
    - 반드시 예외 처리를 해야 하며, 그렇지 않으면 컴파일 오류가 발생합니다.

2. **주요 예**:
    - `IOException`, `SQLException`, `FileNotFoundException`.

3. **용도**:
    - 주로 외부 자원 접근(파일 입출력, 네트워크 통신, 데이터베이스 작업 등)과 관련된 예외 상황에서 사용됩니다.

4. **처리 방법**:
    - 메서드 내부에서 `try-catch` 블록으로 처리하거나, 메서드 선언부에서 `throws` 키워드를 사용하여 호출자에게 예외를 던집니다.

### 언체크

1. **정의 및 특징**:
    - Unchecked Exception은 런타임 시점에 발생하는 예외로, 컴파일러가 체크하지 않습니다.
    - 예외 처리가 강제되지 않으며, 주로 프로그래머의 실수에 의해 발생합니다.

2. **주요 예**:
    - `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`.

3. **용도**:
    - 주로 프로그래머의 실수나 논리 오류를 나타냅니다.

4. **처리 방법**:
    - 반드시 처리할 필요는 없지만, 필요에 따라 `try-catch` 블록을 사용하여 처리할 수 있습니다.

## finally 블록의 역할

`finally` 블록은 예외 처리와 관계없이 항상 실행되는 코드를 포함하는 블록입니다. 예외가 발생하든 안 하든 상관없이 실행되므로 주로 자원을 해제하거나 정리하는 코드를 포함합니다.

### 주요 역할

1. **자원 해제**:
    - 파일 닫기, 데이터베이스 연결 종료, 네트워크 소켓 닫기 등 자원을 명시적으로 해제하는 데 사용됩니다.

2. **정리 작업**:
    - 예외가 발생하든 안 하든 상관없이 반드시 실행해야 하는 정리 작업을 포함합니다.

3. **안정성 확보**:
    - 프로그램이 비정상 종료되더라도, 중요한 정리 작업이 수행되도록 보장합니다.

```java
public void exampleMethod() {
    FileReader reader = null;
    try {
        reader = new FileReader("example.txt");
        // 파일 읽기 작업
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (reader != null) {
            try {
                reader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 체크 예외 사용 예

Checked Exception은 외부 자원 접근이나 중요한 작업에서 발생할 수 있는 예외를 처리하는 데 주로 사용됩니다. 이러한 예외를 처리하지 않으면 컴파일 오류가 발생하므로, 개발자는 반드시 예외 처리를 해야 합니다. 주요 사용 예를 들어보겠습니다:

1. **파일 입출력**
    - 파일을 읽거나 쓸 때 발생할 수 있는 예외를 처리합니다.
    - 예: `FileNotFoundException`, `IOException`.

   ```java
   import java.io.*;

   public class FileReadExample {
       public void readFile(String fileName) throws IOException {
           FileReader reader = null;
           try {
               reader = new FileReader(fileName);
               int data;
               while ((data = reader.read()) != -1) {
                   System.out.print((char) data);
               }
           } catch (FileNotFoundException e) {
               System.out.println("파일을 찾을 수 없습니다: " + fileName);
               // 다른 파일을 읽거나 기본값 사용
               useDefaultFile();
           } catch (IOException e) {
               System.out.println("파일을 읽는 도중 오류가 발생했습니다: " + e.getMessage());
           } finally {
               if (reader != null) {
                   try {
                       reader.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           }
       }

       private void useDefaultFile() {
           // 기본값을 사용하거나 대체 파일을 읽는 로직
           System.out.println("기본값을 사용합니다.");
       }

       public static void main(String[] args) {
           FileReadExample example = new FileReadExample();
           example.readFile("nonexistentfile.txt");
       }
   }
   ```

2. **데이터베이스 접근**
    - 데이터베이스에 접근하고 쿼리를 실행할 때 발생할 수 있는 예외를 처리합니다.
    - 예: `SQLException`.

   ```java
   import java.sql.*;

   public class DatabaseExample {
       public void connectDatabase(String url, String user, String password) throws SQLException {
           Connection conn = null;
           try {
               conn = DriverManager.getConnection(url, user, password);
               // 데이터베이스 작업
           } catch (SQLException e) {
               System.out.println("데이터베이스 연결 실패: " + e.getMessage());
           } finally {
               if (conn != null) {
                   conn.close();
               }
           }
       }
   }
   ```

3. **네트워크 통신**
    - 소켓을 통해 네트워크 통신을 할 때 발생할 수 있는 예외를 처리합니다.
    - 예: `UnknownHostException`, `IOException`.

   ```java
   import java.io.*;
   import java.net.*;

   public class NetworkExample {
       public void connectToServer(String host, int port) throws IOException {
           Socket socket = null;
           try {
               socket = new Socket(host, port);
               PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
               out.println("Hello Server");
           } catch (UnknownHostException e) {
               System.out.println("호스트를 찾을 수 없습니다: " + host);
           } finally {
               if (socket != null) {
                   socket.close

();
}
}
}
}
   ```

## Unchecked Exception 사용 예

Unchecked Exception은 런타임 시점에 발생하며, 주로 프로그래머의 실수나 논리 오류를 나타냅니다. 이러한 예외는 컴파일러가 강제하지 않으며, 필요에 따라 예외 처리를 할 수 있습니다. 주요 사용 예를 들어보겠습니다:

1. **`NullPointerException`**
   - 객체 참조가 null인 상태에서 메서드나 속성에 접근하려 할 때 발생합니다.

   ```java
   public class NullPointerExample {
       public void printLength(String str) {
           if (str != null) {
               System.out.println("문자열 길이: " + str.length());
           } else {
               System.out.println("문자열이 null입니다.");
           }
       }
   }
   ```

2. **`ArrayIndexOutOfBoundsException`**
    - 배열의 인덱스가 유효 범위를 벗어날 때 발생합니다.

   ```java
   public class ArrayExample {
       public void printArrayElement(int[] array, int index) {
           if (index >= 0 && index < array.length) {
               System.out.println("배열 요소: " + array[index]);
           } else {
               System.out.println("인덱스가 범위를 벗어났습니다.");
           }
       }
   }
   ```

3. **`ArithmeticException`**
    - 잘못된 산술 연산을 수행할 때 발생합니다. 예: 0으로 나누기.

   ```java
   public class ArithmeticExample {
       public void divide(int a, int b) {
           try {
               int result = a / b;
               System.out.println("결과: " + result);
           } catch (ArithmeticException e) {
               System.out.println("0으로 나눌 수 없습니다.");
           }
       }
   }
   ```

## Checked Exception 처리 및 로직 조정

Checked Exception을 처리할 때는 단순히 예외를 잡아서 무시하는 것이 아니라, 상황에 맞게 프로그램의 로직을 조정해야 할 때가 많습니다. 이를 통해 프로그램이 예외 상황에서도 정상적으로 동작하거나, 사용자가 인지할 수 있는 방식으로 문제를 해결할 수 있도록 합니다.

### 예제: 파일 읽기 로직 조정

파일을 읽는 도중 `FileNotFoundException`이 발생하면, 사용자에게 파일이 없다는 메시지를 표시하고 다른 파일을 읽거나 기본값을 사용할 수 있습니다.

```java
import java.io.*;

public class FileReadExample {
    public void readFile(String fileName) {
        FileReader reader = null;
        try {
            reader = new FileReader(fileName);
            int data;
            while ((data = reader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (FileNotFoundException e) {
            System.out.println("파일을 찾을 수 없습니다: " + fileName);
            // 다른 파일을 읽거나 기본값 사용
            useDefaultFile();
        } catch (IOException e) {
            System.out.println("파일을 읽는 도중 오류가 발생했습니다: " + e.getMessage());
            // 로그 작성
            logError(e);
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    System.out.println("파일 닫는 도중 오류가 발생했습니다: " + e.getMessage());
                    // 또 다른 예외 처리
                    logError(e);
                }
            }
        }
    }

    private void useDefaultFile() {
        // 기본값을 사용하거나 대체 파일을 읽는 로직
        System.out.println("기본값을 사용합니다.");
    }

    private void logError(Exception e) {
        // 예외 정보를 로그에 기록
        System.err.println("에러 로그: " + e.getMessage());
    }

    public static void main(String[] args) {
        FileReadExample example = new FileReadExample();
        example.readFile("nonexistentfile.txt");
    }
}
```

이 예제에서는 파일을 찾지 못했을 때 사용자에게 알리고, `useDefaultFile` 메서드를 호출하여 기본값을 사용하도록 합니다.

## Unchecked Exception 처리 및 적절한 종료

Unchecked Exception은 런타임 시 발생하는 예외로, 발생 시 프로그램이 예기치 않게 종료될 수 있습니다. 이를 방지하기 위해 프로그램은 Unchecked Exception이 발생하더라도 적절하게 종료될 수 있도록 예외 처리를 해야 합니다.

### 예제: 글로벌 예외 핸들러

전역적으로 Unchecked Exception을 처리하여 프로그램이 비정상 종료되지 않도록 할 수 있습니다. 예를 들어, `Thread.setDefaultUncaughtExceptionHandler`를 사용하여 모든 스레드에서 발생하는 Unchecked Exception을 처리할 수 있습니다.

```java
public class GlobalExceptionHandlerExample {
    public static void main(String[] args) {
        // 전역 예외 핸들러 설정
        Thread.setDefaultUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
            @Override
            public void uncaughtException(Thread t, Throwable e) {
                System.out.println("비정상 종료 방지: " + e.getMessage());
                // 로그 작성 및 자원 해제
                cleanup();
            }
        });

        // 예외 발생 코드
        divideByZero();
    }

    private static void divideByZero() {
        int a = 1;
        int b = 0;
        System.out.println(a / b); // ArithmeticException 발생
    }

    private static void cleanup() {
        // 자원 해제 로직
        System.out.println("자원 해제 완료.");
    }
}
```

이 예제에서는 글로벌 예외 핸들러를 설정하여 모든 스레드에서 발생하는 Unchecked Exception을 처리합니다. 예외가 발생하면 메시지를 출력하고, `cleanup` 메서드를 호출하여 자원을 해제합니다. 이를 통해 프로그램이 예외 발생 후에도 적절하게 종료될 수 있습니다.

## 요약

- **Checked Exception 처리**:
    - 예외가 발생하면 단순히 예외를 처리하는 것에 그치지 않고, 상황에 맞게 로직을 조정하여 프로그램이 정상적으로 동작하도록 합니다.
    - 예를 들어, 파일을 읽을 때 파일이 없으면 다른 파일을 읽거나 기본값을 사용하는 등의 로직을 추가합니다.

- **Unchecked Exception 처리**:
    - Unchecked Exception이 발생하더라도 프로그램이 적절하게 종료될 수 있도록 글로벌 예외 핸들러를 설정합니다.
    - 예외가 발생하면 사용자에게 알리고, 자원을 해제하며, 로그를 남기는 등의 처리를 통해 프로그램의 안정성을 확보합니다.

## 최상위 예외 처리기 설정 시 고려사항

1. **안정성 보장**:
    - 예외 처리기 자체가 예외를 발생시키지 않도록 주의합니다.

2. **자원 해제**:
    - 예외 발생 시 데이터베이스 연결, 파일 핸들, 네트워크 소켓 등 모든 자원을 안전하게 해제합니다.

3. **로깅**:
    - 예외의 스택 트레이스와 관련된 컨텍스트 정보를 로그로 남깁니다.

4. **경고 및 알림**:
    - 치명적인 예외 발생 시 관리자나 모니터링 시스템에 알림을 보냅니다.

5. **시스템 상태 저장**:
    - 예외 발생 시 애플리케이션의 현재 상태를 저장하여 재시작 시 상태를 복구할 수 있도록 합니다.

6. **페일 세이프 모드**:
    - 일부 기능이 중지되더라도 최소한의 기능을 제공할 수 있는 페일 세이프 모드를 구현합니다.

## 최상위 예외 처리기에서 재귀적인 예외 발생 방지

최상위 예외 처리기에서 예외가 발생할 경우, 이를 처리하는 과정에서 다시 예외가 발생하지 않도록 주의해야 합니다. 이를 위해 다음과 같은 방안을 구현할 수 있습니다.

1. **단순한 로깅 구현**:
    - 최상위 예외 처리기에서는 가능한 한 단순한 로깅 메커니즘을 사용합니다. 예를 들어, 표준 출력 또는 파일에 로그를 남기는 방식으로 구현합니다.

2. **로깅 라이브러리의 예외 처리**:
    - 로깅 라이브러리가 예외를 발생시키지 않도록 설정하거나, 로깅 라이브러리 자체의 예외 처리 기능을 활용합니다. 예를 들어, Log4j나 SLF4J와 같은 라이브러리는 내부적으로 발생하는 예외를 처리할 수 있는 설정을 제공합니다.

3. **보조 로깅 메커니즘**:
    - 기본 로깅 메커니즘이 실패할 경우를 대비하여 보조 로깅 메커니즘을 구현합니다. 예를 들어, 기본 로그 파일에 기록할 수 없을 때 표준 출력으로 로그를 남깁니다.

4. **중복 로그 방지 플래그**:
    - 재귀적인 예외 발생을 방지하기 위해 플래그를 사용하여 이미 예외 처리가 진행 중인지를 확인합니다.

```java
public class MainApplication {
    private static AtomicBoolean isHandlingException = new AtomicBoolean(false);

    public static void main(String[] args) {
        // 전역 예외 핸들러 설정
        Thread.setDefaultUncaughtExceptionHandler(new Thread.Un

caughtExceptionHandler() {
            @Override
            public void uncaughtException(Thread t, Throwable e) {
                handleException(e);
            }
        });

        // 예외 발생 코드
        causeException();
    }

    private static void handleException(Throwable e) {
        if (isHandlingException.getAndSet(true)) {
            System.err.println("예외 처리 중에 또 다른 예외가 발생했습니다: " + e.getMessage());
            return;
        }

        try {
            // 예외 처리 로직
            System.err.println("치명적인 오류 발생: " + e.getMessage());
            logError(e);
            cleanup();
            notifyUser(e);
            restartApplication();
        } catch (Exception handlerException) {
            System.err.println("예외 처리기 내에서 예외 발생: " + handlerException.getMessage());
        } finally {
            isHandlingException.set(false);
        }
    }

    private static void causeException() {
        throw new RuntimeException("테스트용 예외");
    }

    private static void cleanup() {
        System.out.println("자원 해제 완료.");
    }

    private static void logError(Throwable e) {
        System.err.println("에러 로그: " + e.getMessage());
    }

    private static void notifyUser(Throwable e) {
        System.out.println("예외가 발생했습니다: " + e.getMessage());
    }

    private static void restartApplication() {
        System.out.println("애플리케이션을 재시작합니다.");
        // 시스템 재시작 로직 (예: 스크립트 호출)
        // Runtime.getRuntime().exec("reboot-script.sh");
    }
}
```

## 요약

- **Checked Exception**은 외부 자원 접근이나 중요한 작업에서 발생할 수 있는 예외를 처리하며, 컴파일러가 예외 처리를 강제합니다.
- **Unchecked Exception**은 주로 프로그래머의 실수나 논리 오류로 인해 발생하며, 컴파일러가 예외 처리를 강제하지 않지만 필요에 따라 처리할 수 있습니다.
- **finally 블록**은 예외 발생 여부와 관계없이 항상 실행되는 코드를 포함하여 자원 해제나 정리 작업을 보장합니다.
- **최상위 예외 처리기**는 애플리케이션 전체에서 발생하는 예외를 포착하고 처리하며, 안정성 보장, 자원 해제, 로깅, 경고 및 알림 등을 통해 시스템의 안정성을 유지합니다.
- **AtomicBoolean**은 멀티스레드 환경에서 boolean 값을 안전하게 읽고 쓸 수 있도록 원자적 연산을 제공하는 클래스입니다.

## 참고사항
- 반복 횟수를 설정하는 두 가지 방식(외부 파라미터와 지역 변수)의 장단점을 고려하여 상황에 맞는 방법을 선택합니다.
- 글로벌 예외 처리기를 설정하여 특정 예외를 포착하지 않도록 하거나, 발생한 예외에 대해 적절한 로그와 알림을 통해 시스템의 안정성을 보장합니다.
- 최상위 예외 처리기에서 예외가 발생했을 때 안전한 종료와 추가적인 대책을 마련하여 시스템의 신뢰성을 유지합니다.

이와 같이 다양한 예외 처리 방법과 상황에 따른 대처 방안을 통해 자바 애플리케이션의 안정성을 높이고, 예외 상황에서도 신뢰할 수 있는 시스템을 구축할 수 있습니다.