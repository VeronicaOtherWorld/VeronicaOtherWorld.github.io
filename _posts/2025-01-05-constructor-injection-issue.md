---
title: "01/05 Resolving Constructor Injection Issue in Spring Framework"
date: 2025-01-05 23:00:00 +0100
categories:
  [study, springBoot]
tags: 
  - [issue]
---


Today, while learning a framework, I encountered a frustrating issue: the instance of a class I created could not be found. I carefully reviewed my code to ensure there were no mistakes. I even tried deleting other classes that implemented the same interface, hoping it would resolve the problem. Unfortunately, that didn’t help either.  

<img src="assets/diary-pic/05-01-2025-error.png" alt="Diary Image" style="border: 1px solid black; border-radius: 2px;"/>


The tutorial video I was following didn’t address this situation, so I turned to GPT for help. After consulting, I finally identified the root cause of the problem: the issue was caused by the no-argument constructor.  

I had created both a no-argument constructor and a parameterized constructor, following the standard JavaBean conventions. However, Spring prioritizes using the no-argument constructor to instantiate the GameRunner class. Since the no-argument constructor does not allow Spring to inject an implementation of the GamingConsole interface, the game field was left uninitialized (null).    

## Solution  

Once I pinpointed the issue, it was easy to fix. There are two possible solutions:  

✅ Remove the no-argument constructor  

This ensures that Spring uses the parameterized constructor for dependency injection.  

```java
@Component
public class GameRunner {
    private final GamingConsole game;

    // Constructor injection
    public GameRunner(GamingConsole game) {
        this.game = game;
    }

    public void runGame() {
        System.out.println("Running game: " + game);
        game.down();
        game.left();
        game.right();
        game.up();
    }
}
```

✅ Annotate the constructor with @Autowired  

If I want to keep the no-argument constructor, I can explicitly tell Spring to use the parameterized constructor for injection by adding the @Autowired annotation.  

```java
@Component
public class GameRunner {
    private GamingConsole game;

    // No-argument constructor
    public GameRunner() {
    }

    // Parameterized constructor with @Autowired annotation
    @Autowired
    public GameRunner(GamingConsole game) {
        this.game = game;
    }

    public void runGame() {
        System.out.println("Running game: " + game);
        game.down();
        game.left();
        game.right();
        game.up();
    }
}
```

## Reflection  

This issue taught me an important lesson about Spring’s behavior with constructors and dependency injection. By default, Spring prefers the no-argument constructor if it exists, which may cause null dependencies if the required fields are not properly initialized. Understanding this helped me not only resolve the issue but also deepen my knowledge of how Spring manages Bean instantiation.  

I’m glad I took the time to troubleshoot and learn from this experience!  