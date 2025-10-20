# Inversion of Control

## Links

* [InversionOfControl - Fowler](https://martinfowler.com/bliki/InversionOfControl.html)
* [Inversion of Control Containers and the Dependency Injection pattern - Fowler](https://martinfowler.com/articles/injection.html)
* [DIP in the Wild - Brett L. Schuchert](https://martinfowler.com/articles/dipInTheWild.html)

## Related topics

* Topics that might be confused with each other
  - Inversion of Control
  - Dependency Inversion Principle (from SOLID)
  - Dependency Injection

#### Dependency Inversion Principle (from SOLID)
#### DESC:
 * High-level modules should not depend on low-level modules. Both should depend on abstractions.
 * Abstractions should not depend on details. Details should depend on abstractions.
 * Don’t let your main business logic depend directly on concrete implementations. Depend on abstract interfaces so your code stays flexible and testable.
 * With DIP high-level modules don’t care which low-level implementation is used. You can easily swap or mock implementations. Code is more flexible, reusable, and maintainable.
#### EXAMPLE:
use <br/>
```python
# service.py
from abc import ABC, abstractmethod

# Abstraction
class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass

# Low-level implementations
class MySQLDatabase(Database):
    def save(self, data):
        print(f"Saving {data} to MySQL")

class PostgreSQLDatabase(Database):
    def save(self, data):
        print(f"Saving {data} to PostgreSQL")

# High-level module depends on abstraction
class UserService:
    def __init__(self, db: Database):
        self.db = db

    def add_user(self, name):
        self.db.save({"name": name})

# main.py
mysql_db = MySQLDatabase()
service1 = UserService(mysql_db)
service1.add_user("Alice")

postgres_db = PostgreSQLDatabase()
service2 = UserService(postgres_db)
service2.add_user("Bob")
```
instead of
```python
class MySQLDatabase:
    def save(self, data):
        print(f"Saving {data} to MySQL")

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()  # tightly coupled
    def add_user(self, name):
        self.db.save({"name": name})

service = UserService()
service.add_user("Alice")
```

#### Dependency Injection
#### DESC:
 * A design pattern where a class receives its dependencies from the outside rather than creating them internally.
 * DI is a way to implement IoC (you invert control by letting something else provide the dependencies).
 * Instead of a class building what it needs, someone else gives it what it needs.
 * Decouples code: classes don’t care how dependencies are created, Easier to test: inject mock objects in tests.
 * Flexible: swap implementations without changing the class.
#### Forms of Dependency Injection:
 * Constructor Injection – Pass dependencies via __init__ (most common in Python).
 * Setter Injection – Pass dependencies via a method or property.
 * Interface Injection – Less common in Python; class implements an interface to accept dependencies.
use
```python
# service.py
from abc import ABC, abstractmethod

# Abstraction
class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass

# Low-level implementations
class MySQLDatabase(Database):
    def save(self, data):
        print(f"Saving {data} to MySQL")

class PostgreSQLDatabase(Database):
    def save(self, data):
        print(f"Saving {data} to PostgreSQL")

# High-level module depends on abstraction
class UserService:
    def __init__(self, db: Database):
        self.db = db

    def add_user(self, name):
        self.db.save({"name": name})

# main.py
mysql_db = MySQLDatabase()
service1 = UserService(mysql_db)
service1.add_user("Alice")

postgres_db = PostgreSQLDatabase()
service2 = UserService(postgres_db)
service2.add_user("Bob")
```
instead of
```python
class MySQLDatabase:
    def save(self, data):
        print(f"Saving {data} to MySQL")

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()  # tightly coupled
    def add_user(self, name):
        self.db.save({"name": name})

service = UserService()
service.add_user("Alice")
```
#### Dependency Injection (DI), Inversion of Control (IoC), and Dependency Inversion Principle (DIP) often look very similar in code.
 * DIP is a principle: It tells you to depend on abstractions, not concrete classes.
 * IoC is a general idea: “Don’t control your dependencies; let someone else provide them.”
 * DI is a concrete technique: Passing dependencies into a class (via __init__) is how you implement IoC and often enforce DIP.

| Concept | Definition | Focus | Implementation in Python |
|---------|------------|-------|--------------------------|
| **Dependency Inversion Principle (DIP)** | High-level modules should depend on abstractions, not concretes. | Design principle | Use abstract classes or protocols to define interfaces. |
| **Inversion of Control (IoC)** | A class or module gives up control of its dependencies; an external entity provides them. | Architecture / flow control | Don’t instantiate dependencies inside the class; let them be provided. |
| **Dependency Injection (DI)** | A way to supply the dependencies of a class from outside, instead of creating them internally. | Design pattern / implementation | Pass dependencies via constructor, setter, or method. |


They are all related, but in fact separate.

### Relation with Hollywood Principle
For some the Hollywood Principle (or Law) is just a synonym of Inversion of Control; some others see subtle differences. See  [Confusion between Inversion of Control and Hollywood Principle](https://stackoverflow.com/questions/43786221/confusion-between-inversion-of-control-and-hollywood-principle).

In this context, another related topic is the difference between libraries and frameworks, as it is stated that frameworks follow the Hollywood Principle (see [What is the difference between a framework and a library?](https://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library)).

#### Hollywood Principle
 * "Don’t call us, we’ll call you."
 * Lower-level components shouldn’t control the flow of the program — instead, higher-level frameworks or systems call them when needed.
 * You don’t call the framework; The framework calls your code at the right time.
 * Your code doesn’t drive the flow — it provides hooks, callbacks, or implementations that a framework will call.
