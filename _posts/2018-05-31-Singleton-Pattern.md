---
layout: post
title: Singleton Pattern
date: 2018-05-31
categories: 
  - Software Development
tags: 
  - design pattern
  - java
  - programming
---


## 1. Concept
Wikipedia:
> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object.

## 2. Signleton Implements

### 2.1 Eager Loading Style

Declare a private static singleton object. The singleton object will be created when the class is loading during the compile phrase. Therefore, the memory space will be allocated even though no one acesses the singleton object. Somebody think it is a kind of memory leak.

```
public class Singleton {

    private static Singleton singleton = new Singleton();

    public static Singleton getSingleton() {
        return singleton;
    }
}
```

Advantage: Thread safe

Disadvantage: No lazy loading

Then let's see how to implement a lazy loading singleton pattern.

### 2.2 Single Thread & Lazy Loading Style

Tke idea is to use a private contructor and a public static factory pattern method. In the facotry pattern method, we check whether the singleton has been initialized. If the singleton has not been initailized, then we will initialize it.

```
public class Singleton {

    private static Singleton singleton;

    private Singleton(){}

    public static Singleton getSingleton() {
        if(singleton==null) {
            singleton = new Singleton();
        }
        return singleton;
    }
}
```

Advantage: Lazy loading

Disadvantage: Not thread safety. The singleton may be initialized several times by different threads.

Then let's see how to implement a lazy loading singleton pattern with thread safety.

### 2.3 Naive Multithreading & Lazy Loading Style

To make implement thread safety, the first idea is to lock the singleton class. If only one thread is allowed to access the singleton, then we can implement with the idea of the single thread way.

We use the "synchronized" keyword to lock the singleton when we check whether the singlton has been initialized and the initialization part. In addition. we need to use the "volatile" keyword. 

There are two reasons why we need the "volatile" keyword. 

1. To guarantee the memory visibility of the singlton. 

2. To prohibit instruction reordering and guarantee the happends-before relationship. 

The synchronized block is no visibility guarantee for non-final fields memory visibility. Every thread has its own working memory, and has a copy of variables in the main memory. And all threads have to process the shared variables in their own working memory instead of the main memory. A thread cannot access other thread's working memory. Threads communicate via the main memory. Therefore, when a thread change the value of the shared variable, other threads do not know. Then we say that the shared variable made by a thread is not visible to other thread. Thus, we need the keyword "volatile". The "volatile" keyword can make sure that a given variable is read directly from main memory, and always written back to main memory when updated. Thus, the memory visibility is guaranteed. But we need to notice that the "volatile" keyword works after JDK 1.5. 

![Internal Java Memory Model](https://github.com/HongyangYu/hongyangyu.github.io/blob/master/images/2018-05-31-Singleton-Pattern/java-memory-model-0.png)

Fig 1. Internal Java Memory Model

![Visibility of Shared Objects](https://github.com/HongyangYu/hongyangyu.github.io/blob/master/images/2018-05-31-Singleton-Pattern/java-memory-model-1.png)

Fig 2. Visibility of Shared Objects

![Bridging The Gap Between The Java Memory Model And The Hardware Memory Architecture](https://github.com/HongyangYu/hongyangyu.github.io/blob/master/images/2018-05-31-Singleton-Pattern/java-memory-model-2.png)

Fig 3. Bridging The Gap Between The Java Memory Model And The Hardware Memory Architecture


```
public class Singleton {

    private static volatile Singleton singleton;

    private Singleton() {}

    public static Singleton getSingleton() {
        synchronized(Singleton.class) {
            if(singleton == null) {
                singleton = new Singleton();
            }
        }
        return singleton;
    }
}
```

Advantage: Thread safety and lazy loading

Disadvantage: Inefficient and "volatile" works after JDK 1.5

This way is inefficient. Because singleton only initialized once, but they need lock the singleton to check whether it is null every time the getSingleton() is called. Let's see how to improve this method.

### 2.4 Improved Multithreading & Lazy Loading Style

To imporve the efficiency of the code in the last section, we could check whether the singleton is null again before the synchronized block (Critical Area). We call it Double-checked Locking. Definitely, we still need to check within the critical area, because several different thread may find the singleton is null during the first check.

Notice the "volatile" keyword, whose features we have mentioned above.

```
public class Singleton {

    private static volatile Singleton singleton;

    private Singleton() {}

    public static Singleton getSingleton() {
        if(singleton == null) { // first check
            synchronized(Singleton.class) {
                if(singleton==null) // second check
                    singleton = new Singleton();
            }
        }
        return singleton;
    }
}
```

Advantage: lazy loading, thread safety and pretty high efficiency

Disadvantage: "volatile" keyword works after JDK 1.5

Is there a solution working for any version of JDK? Let's see.

### 2.5 Static Inner Class Style

We can put the singleton instance into a Static Inner Class. The singleton will be initialized when the Holder is accesses first time. In this way, lazy loading can be implemented. And it guarantees the thread safety because static inner class is only loaded once.

```
public class Singleton {

    private static class Holder {
        private static Singleton singleton = new Singleton();
    }

    private Singleton(){}

    public static Singleton getSingleton() {
        return Holder.singleton;
    }
}
```

Two problems we need to notice:

1. ALL methods we mentioned above need extra works to implement serialization (Serializable、transient、readResolve()). Otherwise, during the deserialization, a new object will be created. 

2. ALL methods we mentioned above cannot guarantee reflection safety. People may use reflection and call the private constructor. To avoid this situation, we can use a flag and set the flag as true when the object is initialized, and if the flag has been already true, throw a exception.

Advantage: Lazy loading and thread safety

Disadvantage: 1. Extra work to implement serialization. 2. Not reflection safety.

Then let's see if there is a solution to solve these two problems.

### 2.6 Enum Style

This way is clear and concise. The Enum not only guarantees singleton, thread safety and reflection safety, but also prvides the auto serialization machine. It can avoid creating a new instance during deserialization. Thus, the authors of the book "Effective Java" recommend that we should always use Enum to implement a singleton pattern. 


```
public enum Singleton {
    SINGLETON;
}

```

In the Enum, we can also add variables and methods just like a plain class. 

```
public enum Singleton {

    SINGLETON;

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

In some blogs, they said, "Enums often require more than twice as much memory as static constants. We should strictly avoid using enums on Android.", which quotes from the official Android developer document. However, the words has already been deleted in the latest doc. At least, I did not find them. In my view, even thought Enums need a little bit more memory space than static constants, might be only 2 KB of RAM, enum should never be avoid. How stupid it is to use integers to replace enums. Especially, nowadays the RAM is really large in both PC and smart phones. Several KB of RAM are accecpable compared with the readability and maintainability. 

Anyway, just like the "Effective Java" mentions, we should always use Enum to implement a singleton pattern, which is the most consice, clear and safety way. 


## Reference
1. Wikipedia: [Singleton Pattern](https://en.wikipedia.org/wiki/Singleton_pattern)
2. Java Memory Model: [Java Memory Model](http://tutorials.jenkov.com/java-concurrency/java-memory-model.html)

