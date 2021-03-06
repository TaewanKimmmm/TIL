# Java Byte Code란
자바는 JVM이라는 자바 가상 머신 위에서 동작하는데, 자바 소스 코드를 JVM이 실행할 수 있도록 변환한 파일이 Java Byte Code이다.

우리가 소스 코드(`.java`)를 작성하면, `Java Compiler(javac.exe)`는 이를 바이트코드(`.class`)로 컴파일한다.

자바 가상 머신(JVM, Java Virtual Machine)을 사용하면, 운영체제와는 독립적으로 동작할 수 있기 때문에 어느 운영체제에서나 같은 형태로 실행될 수 있다.

이러한 점이 수많은 개발자로 하여금 자바를 사용하게 하는 것이다.

자바 프로그래머가 자바 바이트코드를 꼭 인지하거나 이해할 필요는 없다. 

하지만 IBM의 developerWorks journal에서는

> 바이트코드를 이해하고 자바 컴파일러에 의해 바이트코드가 어떻게 생성될 것인지를 이해하는 것은 C나 C++ 프로그래머가 어셈블리어를 이해하는 것과 같다.

라고 제안했다고 한다.

다음과 같은 자바 코드가 있을 때,

```java
for (int i = 2; i < 1000; i++) {
    for (int j = 2; j < i; j++) {
        if (i % j == 0)
            continue outer;
    }
    System.out.println (i);
}
```

자바 컴파일러는 위의 자바 코드를 아래와 같은 바이트 코드로 번역한다:

```assembly
0:   iconst_2
1:   istore_1
2:   iload_1
3:   sipush  1000
6:   if_icmpge       44
9:   iconst_2
10:  istore_2
11:  iload_2
12:  iload_1
13:  if_icmpge       31
16:  iload_1
17:  iload_2
18:  irem
19:  ifne    25
22:  goto    38
25:  iinc    2, 1
28:  goto    11
31:  getstatic       #84; // Field java/lang/System.out:Ljava/io/PrintStream;
34:  iload_1
35:  invokevirtual   #85; // Method java/io/PrintStream.println:(I)V
38:  iinc    1, 1
41:  goto    2
44:  return
```