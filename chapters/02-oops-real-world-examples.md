# Chapter 2: OOPs Real-World Examples

Covers OOP pillars with real-world examples: abstraction and encapsulation.

# OOPS — Abstraction & Encapsulation

## 1. Why Did We Move Beyond Procedural Programming?

### 1.1 Early Languages

#### 1. Machine Language (Binary)

- Direct CPU instructions in 0s & 1s.

**Drawbacks:**

- Extremely error-prone: one bit flip breaks the program.
- Tedious to write and maintain.
- No abstraction — every detail is manual.

#### 2. Assembly Language

- Introduced mnemonics (e.g. `MOV A, 61h`) instead of raw bits.
- Still hardware-tied: code changes with CPU architecture.
- **Scalability:** remains very limited for large systems.

---

### 1.2 Procedural (Structured) Programming

**Features Introduced:**

- Functions for code reuse.
- Control structures:
  - `if-else`
  - `switch`
  - `for/while` loops
- Blocks for grouping statements.

**Advantages:**

- Improved readability over assembly.
- Modularized small to mid-size programs.

**Limitations:**

- **Poor real-world mapping:** Difficult to model complex entities (e.g. a ride-booking system’s users, drivers, payments).
- **Data security gaps:** No built-in access control — everything is globally visible.
- **Reusability & scalability:** Functions alone can’t enforce consistent interfaces or safe extension.

---

# 2. Entering Object-Oriented Programming

**Core Idea:**  
Model your application as interacting objects mirroring real-world entities.

**Benefits:**

- **Natural mapping of domain concepts:** `User`, `Car`, `Ride`.
- **Secure data encapsulation:** Control who can read or modify state.
- **Code reuse:** Via inheritance and interfaces.
- **Scalability:** Through loosely coupled modules.

---

# 3. Modeling Real-World Entities in Code

## 3.1 Objects, Classes, & Instances

- **Object:** A real-world “thing” with attributes and behaviors.
- **Class:** Blueprint defining those attributes (fields) and behaviors (methods).
- **Instance:** Concrete object in memory, created via the class.

---

# 4. Deep Dive: Pillar 1 — Abstraction

## Definition

**Abstraction** hides unnecessary implementation details from the client and exposes only what is essential to use an object’s functionality.

---

## 4.1 Real-World Analogies

### Driving a Car

**What you do:**

- Insert key.
- Press pedals.
- Turn steering wheel.

**What you don’t need to know:**

- How the fuel-injection system works.
- How the transmission synchronizes gears.
- How the engine control unit computes ignition timing.

**Abstraction in action:**

The car provides a simple interface (`start`, `accelerate`, `brake`) and conceals all mechanical complexity under the hood.

---

### Using a TV or Laptop

**What you do:**

- Press buttons on a remote.
- Click icons.

**What you don’t need to know:**

- How the display panel refreshes.
- How the CPU executes machine code.
- How the OS schedules tasks.

**Abstraction in action:**

A graphical interface abstracts away thousands of low-level operations.

---

## 4.2 Language-Level Abstraction

### Control Structures as Abstraction

- Keywords like `if`, `for`, `while` let you express complex branching and loops without writing jump addresses or machine instructions.
- The compiler translates these high-level constructs into assembly or machine code behind the scenes.

---

# 5. Code-Based Abstraction: Abstract Classes & Interfaces

## 5.1 Abstract Class Example (C++)

```cpp
// Abstract interface for any Car type
class Car {
public:
    // Pure virtual methods - no implementation here
    virtual void startEngine() = 0;
    virtual void shiftGear(int newGear) = 0;
    virtual void accelerate() = 0;
    virtual void brake() = 0;
    virtual ~Car() {}
};
```

## Key Points

- The `Car` class declares what operations must exist but hides how they work.
- No code for `startEngine()`, etc., lives here — only signatures.
- Clients use `Car*` pointers without needing concrete details.

---
## 5.2 Concrete Subclass Example

 **Abstraction**

```python
from abc import ABC, abstractmethod


# Abstract class
#
# 1. Acts as an interface for the outside world to operate the car.
# 2. This abstract class tells WHAT the car can do rather than HOW it does it.
# 3. Since this is an abstract class, we cannot directly create objects of this class.
# 4. We need to inherit it first, and then the child class will provide
#    implementation details for all the abstract methods.

class Car(ABC):

    @abstractmethod
    def start_engine(self):
        pass

    @abstractmethod
    def shift_gear(self, gear):
        pass

    @abstractmethod
    def accelerate(self):
        pass

    @abstractmethod
    def brake(self):
        pass

    @abstractmethod
    def stop_engine(self):
        pass


# Concrete class
#
# SportsCar provides the actual implementation of the abstract Car class.
# Now we can create an object of SportsCar.
#
# In the real-world example:
# Car -> represents the interface/buttons/pedals/steering wheel.
# SportsCar -> represents the actual implementation of those operations.

class SportsCar(Car):

    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
        self.is_engine_on = False
        self.current_speed = 0
        self.current_gear = 0

    def start_engine(self):
        self.is_engine_on = True
        print(
            f"{self.brand} {self.model} : "
            "Engine starts with a roar!"
        )

    def shift_gear(self, gear):
        if not self.is_engine_on:
            print(
                f"{self.brand} {self.model} : "
                "Engine is off! Cannot Shift Gear."
            )
            return

        self.current_gear = gear

        print(
            f"{self.brand} {self.model} : "
            f"Shifted to gear {self.current_gear}"
        )

    def accelerate(self):
        if not self.is_engine_on:
            print(
                f"{self.brand} {self.model} : "
                "Engine is off! Cannot accelerate."
            )
            return

        self.current_speed += 20

        print(
            f"{self.brand} {self.model} : "
            f"Accelerating to {self.current_speed} km/h"
        )

    def brake(self):
        self.current_speed -= 20

        if self.current_speed < 0:
            self.current_speed = 0

        print(
            f"{self.brand} {self.model} : "
            f"Braking! Speed is now {self.current_speed} km/h"
        )

    def stop_engine(self):
        self.is_engine_on = False
        self.current_gear = 0
        self.current_speed = 0

        print(
            f"{self.brand} {self.model} : "
            "Engine turned off."
        )


# Main Method / Program Execution

my_car = SportsCar("Ford", "Mustang")

my_car.start_engine()
my_car.shift_gear(1)
my_car.accelerate()
my_car.shift_gear(2)
my_car.accelerate()
my_car.brake()
my_car.stop_engine()
```
**Encapsulation**

```python
class SportsCar:
    """
    Encapsulation says 2 things:

    1. An object's characteristics and behaviour are encapsulated
       together within that object.

    2. All characteristics or behaviours are not for everyone to access.
       The object should provide data security.

    In Python:
    - Class acts as a blueprint for object creation.
    - Attributes and methods are kept together inside the class.
    - Private attributes are indicated using __ (double underscore).
    """

    def __init__(self, brand, model):
        # Private attributes
        self.__brand = brand
        self.__model = model
        self.__is_engine_on = False
        self.__current_speed = 0
        self.__current_gear = 0

        # Variable used to explain setter
        self.__tyre_company = "MRF"

    # Getter for current speed
    def get_speed(self):
        return self.__current_speed

    # Getter for tyre company
    def get_tyre_company(self):
        return self.__tyre_company

    # Setter for tyre company
    def set_tyre_company(self, tyre_company):
        self.__tyre_company = tyre_company

    def start_engine(self):
        self.__is_engine_on = True

        print(
            f"{self.__brand} {self.__model} : "
            "Engine starts with a roar!"
        )

    def shift_gear(self, gear):
        if not self.__is_engine_on:
            print(
                f"{self.__brand} {self.__model} : "
                "Engine is off! Cannot Shift Gear."
            )
            return

        self.__current_gear = gear

        print(
            f"{self.__brand} {self.__model} : "
            f"Shifted to gear {self.__current_gear}"
        )

    def accelerate(self):
        if not self.__is_engine_on:
            print(
                f"{self.__brand} {self.__model} : "
                "Engine is off! Cannot accelerate."
            )
            return

        self.__current_speed += 20

        print(
            f"{self.__brand} {self.__model} : "
            f"Accelerating to {self.__current_speed} km/h"
        )

    def brake(self):
        self.__current_speed -= 20

        if self.__current_speed < 0:
            self.__current_speed = 0

        print(
            f"{self.__brand} {self.__model} : "
            f"Braking! Speed is now {self.__current_speed} km/h"
        )

    def stop_engine(self):
        self.__is_engine_on = False
        self.__current_gear = 0
        self.__current_speed = 0

        print(
            f"{self.__brand} {self.__model} : "
            "Engine turned off."
        )


# Main Method / Program Execution

my_sports_car = SportsCar("Ford", "Mustang")

my_sports_car.start_engine()
my_sports_car.shift_gear(1)
my_sports_car.accelerate()
my_sports_car.shift_gear(2)
my_sports_car.accelerate()
my_sports_car.brake()
my_sports_car.stop_engine()

# Setting arbitrary value to speed is NOT directly allowed
# because current_speed is a private attribute.

# my_sports_car.__current_speed = 500

print(
    "Current Speed of My Sports Car is",
    my_sports_car.get_speed()
)
```
---

# 6. Benefits of Abstraction

1. **Simplified Interfaces:**  
   Clients focus on what an object does, not how it does it.

2. **Ease of Maintenance:**  
   Internal changes (e.g., switching from a V6 to an electric motor) don’t affect client code.

3. **Code Reuse:**  
   Multiple concrete classes can implement the same abstract interface (e.g., `SportsCar`, `SUV`, `ElectricCar`).

4. **Reduced Complexity:**  
   Large systems are easier to reason about when broken into abstract modules.

---

# 7. Deep Dive: Pillar 2 — Encapsulation

## Definition

**Encapsulation** bundles an object’s data (its state) and the methods that operate on that data into a single unit, and controls access to its inner workings.

---

## 7.1 Two Facets of Encapsulation

### 1. Logical Grouping

- Data (fields) and behaviors (methods) that belong together live in the same “capsule” (class).
- Example: A `Car` class encapsulates:
  - `engineOn`
  - `currentSpeed`
  - `shiftGear()`
  - `accelerate()`
  - etc.

### 2. Data Security

- Restrict direct external access to sensitive fields to prevent invalid or unsafe operations.
- Example: You can read the car’s odometer but cannot directly set it back to zero.

---

## 7.2 Real-World Analogies

### Medicine Capsule

- The capsule holds both the medicine (data) and its protective shell (access control).
- You swallow the capsule without exposing its contents directly.

### Car Odometer

- You can view the mileage but cannot tamper with it via the dashboard interface.

---

## 7.3 Access Modifiers in C++

- **`public`:** Members are accessible everywhere.
- **`private`:** Members are accessible only within the class itself.
- **`protected`:** Accessible in the class and its subclasses (for inheritance scenarios).

---

## 7.4 Getters & Setters with Validation

**Purpose:**  
Allow controlled mutation with checks, rather than exposing fields blindly.

---

## 7.5 Encapsulation Benefits

1. **Robustness:**  
   Prevents accidental or malicious misuse of internal state.

2. **Maintainability:**  
   Internal changes (e.g., adding new constraints) do not ripple into client code.

3. **Clear Contracts:**  
   Clients interact only via well-defined methods (the public API).

4. **Modularity:**  
   Code is organized into self-contained units, easing testing and reuse.

[Back to the course index](../README.md)
