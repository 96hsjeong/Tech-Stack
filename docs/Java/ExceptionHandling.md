# 예외처리 (Exception Handling)
*written by sohyeon, hyemin 💡*

<br>

## 1. 프로그램 오류

프로그램 실행 중 어떤 원인에 의해 오작동하거나 비정상적으로 종료되는 경우가 있는데  
이런 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다. 

발생시점에 따라 다음과 같이 두종류로 구분한다.

* 컴파일 에러 (compile error) : 컴파일 시에 발생하는 에러
* 런타임 에러 (runtime error) : 실행 시에 발생하는 에러
* 논리적 에러 (logical error) : 실행은 되지만 의도와 다르게 동작하는 것

자바에서는 실행 시에(run time) 발생하는 프로그램 오류를 다음과 같이 구분한다

* 에러(error): 메모리 부족이나 스택오버플로(OutOfMemoryError, StackOverflowError)와 같이 수습될 수 없는 심각한 오류
* 예외(exception): 코드에 의해 수습될 수 있는 다소 미약한 오류

`프로그래머가 예외에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다.`  

## 2. 예외 클래스의 계층구조

모든 클래스의 조상은 Object클래스 이므로 Exception과 Error클래스 역시 Object클래스의 자손들이다.

```
                  Object
                    |
                Throwable
                  /  \
      Exception          Error
        / \               /  \
IOException RuntimeEx..  ..  OutOfMemoryError
```

모든 예외의 최고 조상은 Exception class이고 다음과 같이 표현할 수 있다.
```
Exception
    |
    |--- IOException
    |--- ClassNotFoundException
    |--- ...
    |--- RuntimeException
            |--- ArithmeticException
            |--- ClassCastException
            |--- NullPointerException
            |--- ...
            |--- IndexOutOfBoundsException
```
두 그룹으로 나누어진다.  
* `Exception 클래스들`  
    : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
* `RuntimeException 클래스들`  
    : 프로그래머의 실수로 발생하는 예외

## 3. 예외처리하기

### 3-1. 예외처리란?

    - 정의: 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
    - 목적: 프로그램의 비정상 종료를 막고 정상적인 실행상태를 유지하는 것

예외를 처리하기 위해 `try-catch문`을 사용한다.

#### ex) 예제
```Java
class ExceptionEx1{
    public static void main(String args[]) {
        try {
            try{ } catch(Exception e){ }
        }catch (Exception e){
            try{ } catch(Exception e){ }  // --> error 'e' 가 중복 선언
        }
    }
}
```

### 3-2. 특징

* try블럭 또는 catch블럭에 또 다른 try-catch문이 포함될 수 있다.  
* catch블럭 내에 또 하나의 try-catch문이 포함된 경우, 같은 이름의 참조변수를 사용해서는 안된다.  

## 4. try-catch문의 흐름

* try블럭 내에서 예외가 발생한 경우  

    1. 발생한 예외와 일치하는 catch블럭이 있는지 확인  

    2. 일치하는 catch블럭을 찾게되면, 그 catch블럭 내의 문장들을 수행하고  
       전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행  
       만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.  

* try블럭 내에서 예외가 발생하지 않은 경우  

    1. catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.  

## 5. 예외의 발생과 catch블럭

### 5-1. 예외의 발생과 처리

* catch블럭은 `괄호()`와 `블럭{}` 두 부분으로 나눠져 있는데,  
  `괄호()`내에는 처리하고자 하는 예외와 같은 타입의 참조변수 하나를 선언해야 한다.

* 예외가 발생하면 해당 클래스의 인스턴스가 만들어진다.  

* 예외가 발생한 문장이 try-catch문의 try블럭에 있다면, 이 예외를 처리할 수 있는 catch블럭을 찾게된다.  

* 첫번째 catch 블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 
  생성된 예외클래스의 instance에 instanceof 연산자를 이용해 검사하고 결과가 true이면 예외처리한다.  

* 모든 예외 클래스는 Exception클래스의 자손이므로 Exception클래스 타입의 참조변수를 선언해 놓으면 
  어떤 종류의 예외가 발생해도 이 catch블럭에 의해 처리된다.  

#### ex) 예제
```Java
class ExceptionEx {
     public static void main(String args[]) {
         System.out.println(1);
         System.out.println(2);
         try {
             System.out.println(3);
             System.out.println(0/0); // 0으로 나눠 ArithmeticException발생
             System.out.println(4);  // 실행되지 않는다
         }catch (ArithmeticException ae){
             if (ae instanceof ArithmeticException)
                System.out.println("true");
             System.out.println("ArithmeticException");
         }catch (Exception e){  //ArithmeticException을 제외한 모든 예외
             System.out.println("Exception");
         }
         System.out.println(6);
     }
 }
 ```
#### 실행결과
```
1
2
3
true
ArithmeticException
6
```

### 5-2. 예외에 대한 method

예외가 발생했을 때 생성되는 예외클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨져 있으며  
다음 method들을 통해 정보를 얻을 수 있다.  

    - printStackTrace(): 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력  
    - getMessage(): 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.  

### 5-3. 멀티 catch블럭

JDK1.7부터 여러 catch블럭을 '|' 기호를 이용해서 하나의 catch블럭으로 합칠 수 있게 되었다.  
중복된 코드를 줄일 수 있고 연결할 수 있는 예외 클래스의 개수에 제한이 없다.  

> 두 예외 클래스가 조상과 자손 관계라면 컴파일 에러가 발생한다.  
> 조상클래스만 써주는 것과 동일하기 때문에 불필요한 코드를 제거하라는 의미의 에러다.

#### ex) 적용 전
```Java
try {
    ...
}catch (ExceptionA e){
    e.printStackTrace();
}catch (ExceptionB e2){
    e2.printStackTrace();
}
```
#### ex) 적용 후
```Java
try {
    ...
}catch (ExceptionA | ExceptionB e){
    e.printStackTrace();
}
```

## 6. finally블럭

finally블럭은 try-catch문과 함께 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.
try-catch문 끝에 선택적으로 덧붙여 사용할 수 있다.

#### ex) 예제
```Java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
    ...
} catch (Exception e) {
    // 예외처리를 위한 문장을 적는다.
    ...
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
}
```
예외가 발생한 경우에는 `try->catch->finally`순으로 실행되고,  
예외가 발생하지 않은 경우 `try->finally`순으로 실행된다.

