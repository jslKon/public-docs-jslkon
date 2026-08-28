---
title: Singleton
description: One instance per process, and why that is a stronger claim than it sounds.
tags:
  - design-patterns
  - creational
date: 2026-08-28
---

> **Placeholder content.** Written to exercise the renderer — headings, a code block, a table,
> a blockquote and an inline PlantUML diagram. Replace the prose before publishing.

A singleton guarantees one instance per process and gives you a global access point to it.
The guarantee is narrower than it looks: "per process" is not "per cluster", and not even
"per classloader".

## Minimal implementation

```java
public enum Config {
  INSTANCE;

  private final String url = System.getenv("DB_URL");

  public String url() {
    return url;
  }
}
```

The enum form is the one to reach for in Java — the JVM handles lazy init and serialization,
and it is not defeatable by reflection.

## Trade-offs

| Property | Benefit | Cost |
| --- | --- | --- |
| Global access | No wiring needed | Hidden dependency — callers do not declare it |
| Single instance | Shared cache, one connection pool | Shared mutable state across threads |
| Lazy init | Cheap startup | Init order becomes implicit and hard to trace |

## Lifecycle

```plantuml
@startuml
skinparam backgroundColor transparent
autonumber

participant Caller
participant "Config (enum)" as Config

Caller -> Config : INSTANCE
Config -> Config : class init (once, JVM-guaranteed)
Config --> Caller : instance
Caller -> Config : url()
Config --> Caller : value
@enduml
```

> Most singletons in application code would be better as a single instance held by the DI
> container. You keep one instance without making the dependency invisible to callers.
