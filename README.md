### Gradle Dependency
----


##依赖声明

```
dependencies {
    %依赖配置% : %依赖包信息%
}
```

其中，依赖配置包括以下几类：
- compile
- runtime
- testCompile
- testRuntime

例如：

```
apply plugin: 'java'

repositories {
    mavenCentral()
}

dependencies {
    compile 'org.hibernate:hibernate-core:3.6.7.Final'
    testCompile group: 'junit', name: 'junit', version: '4.+'
}
```

声明的是当前```project```的包依赖

##依赖种类

主要包括 包依赖、子项目依赖、脚本依赖和任务依赖 四类。

- 包依赖

  在```repositories```下载的```jar```包会放置在```~/.gradle/caches/modules-2/files-2.1/```下面，通过定向```classpath```使其能够被访问。
  
  若```repositories```中未包含某个```jar```包，也可以使用方法指定：
  
  ```
  dependencies{
      compile fileTree(dir: "${rootProject.projectDir}/lib", include: "*.jar")
  }
  ```

- 子项目依赖

  ```
  dependencies {
      compile project(':core')
  }
  ```

- 构建脚本的自我依赖

  脚本依赖采用```buildscript```块定义，用于脚本本身执行。
  
  例如：
  
  ```
  buildscript {
      repositories {
          mavenCentral()
      }
  
  
      dependencies {
          classpath "com.googlecode.flyway:flyway-gradle-plugin:2.3.1"
          classpath "mysql:mysql-connector-java:5.1.36"
      }
  }
  ```

  需要注意，脚本依赖需要显示指明```repositories```。

- 任务依赖

  任务在执行中若依赖于其他任务可定义如下：
  ```
  task taskA ( dependsOn : taskB, taskC ) << {
      ...
  }
  ```
  或
  ```
  taskA.dependsOn taskB, taskC
  ```
  
  其中```task <<```是```task.doLast```的简写。
  
  
  ----
  
  ## 关于```jar{}```块 和```flyway{}``` 块的位置问题
  
  首先 ```jar``` 和 ```flyway``` 都是插件中定义的任务名称，这两个块的存在本质是用于修改相应任务的相关属性的,例如
  
  ```
  jar {
    baseName = 'user-management'
    version = '0.1.0'
  }
  ```
  
  甚至可以修改任务的```name```属性。
  
  所以这两个块都应与相应的 ```apply plugin: ...``` 同级，即同属于一个对象，如```project```或```buildscript```。因此，这样写是有效的：
  
  ```
  buildscript{
      apply plugin: 'java'
      jar{
        name = 'haha'
      }
  }
  ```
  
  
  
