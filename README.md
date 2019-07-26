This project demonstrates the effect of dependency management section in pom.xml.

The dependency of each project is illustrated below:

```
Project A
  +- Project B (dependencyManagement com.google.guava:guava:20.0)
      +- Project C
          + com.google.guava:guava:19.0
```

`mvn dependency:tree` shows the followings:

# Project C receives guava 19.0

When Project C is the root, it picks up guava 19.0 in its direct dependency.

```
suztomo@suxtomo24:~/depmgmt-test$ mvn dependency:tree
[INFO] ------------------------------------------------------------------------
[INFO] Building projectC 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ project-c ---
[INFO] com.google.suztomo:project-c:jar:1.0-SNAPSHOT
[INFO] \- com.google.guava:guava:jar:19.0:compile
[INFO] 
```

# Project B receives guava 20.0

When Project B is the root, the management dependency section controls the version of guava in its
transitive dependencies.

```
[INFO] ------------------------------------------------------------------------
[INFO] Building projectB 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ project-b ---
[INFO] com.google.suztomo:project-b:jar:1.0-SNAPSHOT
[INFO] \- com.google.suztomo:project-c:jar:1.0-SNAPSHOT:compile
[INFO]    \- com.google.guava:guava:jar:20.0:compile
```

# Project A receive guava 19.0

When Project A is the root, it picks up the guava in transitive dependency without the effect of
the dependency management in its dependencies.

```
[INFO] ------------------------------------------------------------------------
[INFO] Building projectA 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ project-a ---
[INFO] com.google.suztomo:project-a:jar:1.0-SNAPSHOT
[INFO] \- com.google.suztomo:project-b:jar:1.0-SNAPSHOT:compile
[INFO]    \- com.google.suztomo:project-c:jar:1.0-SNAPSHOT:compile
[INFO]       \- com.google.guava:guava:jar:19.0:compile
```

