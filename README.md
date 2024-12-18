# slf4j-demo

### 踩坑相关踩坑

1. 引用调用SLF4J API 运行报错

   ```none
   SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
   SLF4J: Defaulting to no-operation (NOP) logger implementation
   SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
   ```

   答：SLF4J只是接口层，未找到实现则默认的空实现(nop)【1.6 开始】，并打印警告

   可引入 SLF4J 实现/绑定层，如：

   ```none
   <dependency>
   	<groupId>ch.qos.logback</groupId>
   	<artifactId>logback-classic</artifactId>
   	<version>1.2.3</version>
   	<scop>test</scop>
   </dependency>
   ```
   这个版本一定要选1.2.3，不要升级到1.4.12，否则报错，而且不输出日志
2. 在多个参数情况下，若想打印 Throwable 堆栈信息，需注意Throwable必须放在最后一个
   例：logger.error("错误消息：{}",e.getMessage(),e);

![image.png](README/1732498539244-edf49d7a-578c-4e50-826a-0b8e5ad57b16.png)



### 最佳实践

#### 源码

```java
package com.coderdream;

import lombok.extern.slf4j.Slf4j;

//TIP To <b>Run</b> code, press <shortcut actionId="Run"/> or
// click the <icon src="AllIcons.Actions.Execute"/> icon in the gutter.
@Slf4j
public class Main {

    public static void main(String[] args) {
        //TIP Press <shortcut actionId="ShowIntentionActions"/> with your caret at the highlighted text
        // to see how IntelliJ IDEA suggests fixing it.
        System.out.printf("Hello and welcome!");

        for (int i = 1; i <= 5; i++) {
            //TIP Press <shortcut actionId="Debug"/> to start debugging your code. We have set one <icon src="AllIcons.Debugger.Db_set_breakpoint"/> breakpoint
            // for you, but you can always add more by pressing <shortcut actionId="ToggleLineBreakpoint"/>.
//            System.out.println("i = " + i);

            log.info("i =  {}", i);
        }

        try {
            int a = 0;
            int b = 2 / a;
            System.out.println("b = " + b);
        } catch (Exception e) {
//            log.error(e.getMessage(), e);
            log.error("错误消息：{}", e.getMessage(), e);
        }
        log.info("Hello and welcome!");

    }
}

```

#### 输出

![image-20241125095101197](README/image-20241125095101197.png)