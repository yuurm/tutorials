Создание Maven-зависимости и подключение её как библиотеки к другому проекту, а также написание своего Maven-плагина включает несколько шагов. Вот полный туториал:

---

### **1. Создание библиотеки (Maven-зависимости)**

#### **Шаг 1: Создайте проект для библиотеки**
1. Сгенерируйте Maven-проект:
   ```bash
   mvn archetype:generate -DgroupId=com.example.mylib -DartifactId=my-library -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   cd my-library
   ```

2. Добавьте код в библиотеку. Например, создадим класс `MathUtils`:
   ```java
   package com.example.mylib;

   public class MathUtils {
       public static int add(int a, int b) {
           return a + b;
       }
   }
   ```

3. Обновите `pom.xml`, указав версию:
   ```xml
   <groupId>com.example.mylib</groupId>
   <artifactId>my-library</artifactId>
   <version>1.0.0</version>
   ```

4. Упакуйте библиотеку в `.jar`:
   ```bash
   mvn clean install
   ```

   Файл `.jar` будет находиться в папке `target/my-library-1.0.0.jar`.

---

### **2. Использование библиотеки в другом приложении**

#### **Шаг 1: Создайте проект для приложения**
1. Создайте новый Maven-проект:
   ```bash
   mvn archetype:generate -DgroupId=com.example.myapp -DartifactId=my-application -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   cd my-application
   ```

2. Подключите библиотеку в `pom.xml`:
   ```xml
   <dependency>
       <groupId>com.example.mylib</groupId>
       <artifactId>my-library</artifactId>
       <version>1.0.0</version>
   </dependency>
   ```

#### **Шаг 2: Используйте библиотеку**
В классе приложения добавьте вызов метода из библиотеки:
```java
package com.example.myapp;

import com.example.mylib.MathUtils;

public class Application {
    public static void main(String[] args) {
        int result = MathUtils.add(5, 10);
        System.out.println("Result: " + result);
    }
}
```

3. Соберите и запустите приложение:
   ```bash
   mvn clean compile exec:java -Dexec.mainClass="com.example.myapp.Application"
   ```

---

### **3. Написание собственного Maven-плагина**

#### **Шаг 1: Создайте проект для плагина**
1. Генерация Maven-проекта для плагина:
   ```bash
   mvn archetype:generate -DgroupId=com.example.maven -DartifactId=my-maven-plugin -DarchetypeArtifactId=maven-archetype-plugin -DinteractiveMode=false
   cd my-maven-plugin
   ```

2. В файле `pom.xml` укажите информацию о плагине:
   ```xml
   <groupId>com.example.maven</groupId>
   <artifactId>my-maven-plugin</artifactId>
   <version>1.0.0</version>
   ```

#### **Шаг 2: Напишите реализацию плагина**
Создайте класс, реализующий логику:
```java
package com.example.maven;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugins.annotations.Mojo;

@Mojo(name = "sayhello")
public class HelloMojo extends AbstractMojo {
    public void execute() throws MojoExecutionException {
        getLog().info("Hello from my custom Maven plugin!");
    }
}
```

#### **Шаг 3: Упакуйте плагин**
Соберите плагин:
```bash
mvn clean install
```

#### **Шаг 4: Используйте плагин в другом проекте**
1. В `pom.xml` другого проекта добавьте плагин:
   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>com.example.maven</groupId>
               <artifactId>my-maven-plugin</artifactId>
               <version>1.0.0</version>
           </plugin>
       </plugins>
   </build>
   ```

2. Запустите ваш плагин:
   ```bash
   mvn com.example.maven:my-maven-plugin:1.0.0:sayhello
   ```

   Вывод:
   ```
   [INFO] Hello from my custom Maven plugin!
   ```

---

### Результат
- Вы создали библиотеку, добавили её в приложение и использовали.
- Написали собственный Maven-плагин и использовали его в проекте.

Эти шаги помогут вам понять процесс создания и интеграции пользовательских библиотек и плагинов Maven.
