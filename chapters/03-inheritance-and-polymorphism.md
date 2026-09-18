# Chapter 3: Inheritance and Polymorphism in OOPs

Explores inheritance and polymorphism through object-oriented design examples.

# Inheritance and Polymorphism

# 1. Inheritance

## 1.1 What is Inheritance?

- Real-world objects are often related in parent-child relationships.
- Example: Object A (Parent) and Object B (Child) share properties.
- In programming, this relationship is mimicked using **Inheritance**.

---

## 1.2 Real-Life Example: Car Hierarchy

### Parent Class: `Car` (Generic)

**Common attributes:**

- `Brand`
- `Model`
- `IsEngineOn`
- `CurrentSpeed`

**Common behaviors:**

- `startEngine()`
- `stopEngine()`
- `accelerate()`
- `brake()`

### Child Classes

#### `ManualCar` (inherits `Car`)

**Specific attribute:**

- `CurrentGear`

**Specific behavior:**

- `shiftGear()`

#### `ElectricCar` (inherits `Car`)

**Specific attribute:**

- `BatteryPercentage`

**Specific behavior:**

- `chargeBattery()`

---

## 1.3 C++ Syntax

```cpp
class ManualCar : public Car { ... };

class ElectricCar : public Car { ... };
```

- **public inheritance** maintains access specifiers.
- `private` and `protected` alter accessibility.

---

## 1.4 Access Specifiers in Inheritance

### `public`

- Public members stay public.
- Protected members stay protected.

### `protected`

- Public and protected members become protected.

### `private`

- All inherited members become private.

### Important Point

- Private members of the parent class are **never directly accessible in the child class**.
**Dynamic Polymorphism**

```python
from abc import ABC, abstractmethod


"""
Dynamic Polymorphism in real life says that 2 Objects coming from the same
family will respond to the same stimulus differently.

For example:
Manual Car and Electric Car will respond to accelerate() differently.

To represent this in programming:

1. We create a parent class that defines all characteristics and behaviours
   that are generic to all child classes.

2. We make those methods abstract that are common to all child classes but
   whose implementation will be different for each child class.

3. Child classes provide their own implementation of these abstract methods.

This is Dynamic Polymorphism.
"""


# Parent / Abstract Class
class Car(ABC):

    def __init__(self, brand, model):
        # Protected attributes
        self._brand = brand
        self._model = model
        self._is_engine_on = False
        self._current_speed = 0

    # Common method for all cars
    def start_engine(self):
        self._is_engine_on = True

        print(
            f"{self._brand} {self._model} : "
            "Engine started."
        )

    # Common method for all cars
    def stop_engine(self):
        self._is_engine_on = False
        self._current_speed = 0

        print(
            f"{self._brand} {self._model} : "
            "Engine turned off."
        )

    # Abstract methods for Dynamic Polymorphism
    @abstractmethod
    def accelerate(self):
        pass

    @abstractmethod
    def brake(self):
        pass


# Manual Car
class ManualCar(Car):

    def __init__(self, brand, model):
        # Call parent constructor
        super().__init__(brand, model)

        self.__current_gear = 0

    # Specialized method for Manual Car
    def shift_gear(self, gear):
        self.__current_gear = gear

        print(
            f"{self._brand} {self._model} : "
            f"Shifted to gear {self.__current_gear}"
        )

    # Overriding accelerate - Dynamic Polymorphism
    def accelerate(self):

        if not self._is_engine_on:
            print(
                f"{self._brand} {self._model} : "
                "Cannot accelerate! Engine is off."
            )
            return

        self._current_speed += 20

        print(
            f"{self._brand} {self._model} : "
            f"Accelerating to {self._current_speed} km/h"
        )

    # Overriding brake - Dynamic Polymorphism
    def brake(self):

        self._current_speed -= 20

        if self._current_speed < 0:
            self._current_speed = 0

        print(
            f"{self._brand} {self._model} : "
            f"Braking! Speed is now {self._current_speed} km/h"
        )


# Electric Car
class ElectricCar(Car):

    def __init__(self, brand, model):
        # Call parent constructor
        super().__init__(brand, model)

        self.__battery_level = 100

    # Specialized method for Electric Car
    def charge_battery(self):
        self.__battery_level = 100

        print(
            f"{self._brand} {self._model} : "
            "Battery fully charged!"
        )

    # Overriding accelerate - Dynamic Polymorphism
    def accelerate(self):

        if not self._is_engine_on:
            print(
                f"{self._brand} {self._model} : "
                "Cannot accelerate! Engine is off."
            )
            return

        if self.__battery_level <= 0:
            print(
                f"{self._brand} {self._model} : "
                "Battery dead! Cannot accelerate."
            )
            return

        self.__battery_level -= 10
        self._current_speed += 15

        print(
            f"{self._brand} {self._model} : "
            f"Accelerating to {self._current_speed} km/h. "
            f"Battery at {self.__battery_level}%."
        )

    # Overriding brake - Dynamic Polymorphism
    def brake(self):

        self._current_speed -= 15

        if self._current_speed < 0:
            self._current_speed = 0

        print(
            f"{self._brand} {self._model} : "
            f"Regenerative braking! "
            f"Speed is now {self._current_speed} km/h. "
            f"Battery at {self.__battery_level}%."
        )


# Main function

# Parent class reference can point to child class object.
my_manual_car: Car = ManualCar("Suzuki", "WagonR")

my_manual_car.start_engine()
my_manual_car.accelerate()
my_manual_car.accelerate()
my_manual_car.brake()
my_manual_car.stop_engine()


print("----------------------")


my_electric_car: Car = ElectricCar("Tesla", "Model S")

my_electric_car.start_engine()
my_electric_car.accelerate()
my_electric_car.accelerate()
my_electric_car.brake()
my_electric_car.stop_engine()
```
**Inheritance**
```python
class Car:
    def __init__(self, brand, model):
        self._brand = brand
        self._model = model
        self._is_engine_on = False
        self._current_speed = 0

    # Common methods for all cars
    def start_engine(self):
        self._is_engine_on = True
        print(f"{self._brand} {self._model} : Engine started.")

    def stop_engine(self):
        self._is_engine_on = False
        self._current_speed = 0
        print(f"{self._brand} {self._model} : Engine turned off.")

    def accelerate(self):
        if not self._is_engine_on:
            print(
                f"{self._brand} {self._model} : "
                "Cannot accelerate! Engine is off."
            )
            return

        self._current_speed += 20
        print(
            f"{self._brand} {self._model} : "
            f"Accelerating to {self._current_speed} km/h"
        )

    def brake(self):
        self._current_speed -= 20

        if self._current_speed < 0:
            self._current_speed = 0

        print(
            f"{self._brand} {self._model} : "
            f"Braking! Speed is now {self._current_speed} km/h"
        )


class ManualCar(Car):  # Inherits from Car
    def __init__(self, brand, model):
        super().__init__(brand, model)
        self.__current_gear = 0

    # Specialized method for Manual Car
    def shift_gear(self, gear):
        self.__current_gear = gear
        print(
            f"{self._brand} {self._model} : "
            f"Shifted to gear {self.__current_gear}"
        )


class ElectricCar(Car):  # Inherits from Car
    def __init__(self, brand, model):
        super().__init__(brand, model)
        self.__battery_level = 100

    # Specialized method for Electric Car
    def charge_battery(self):
        self.__battery_level = 100
        print(
            f"{self._brand} {self._model} : "
            "Battery fully charged!"
        )


# Main Method

my_manual_car = ManualCar("Suzuki", "WagonR")

my_manual_car.start_engine()
my_manual_car.shift_gear(1)  # specific to manual car
my_manual_car.accelerate()
my_manual_car.brake()
my_manual_car.stop_engine()


print("----------------------")


my_electric_car = ElectricCar("Tesla", "Model S")

my_electric_car.charge_battery()  # specific to electric car
my_electric_car.start_engine()
my_electric_car.accelerate()
my_electric_car.brake()
my_electric_car.stop_engine()
```

---

# 2. Polymorphism

## 2.1 What is Polymorphism?

**Polymorphism** is derived from:

- **Poly** → many
- **Morph** → forms

Therefore:

> **Polymorphism = Many Forms**

One stimulus can produce different responses based on the object or situation.

---

## 2.2 Two Real-Life Scenarios

### Scenario 1: Different Objects

Different animals such as:

- Duck
- Human
- Tiger

can all have a `run()` behavior.

However, each performs it differently.

### Scenario 2: Same Object, Different Context

The same human can `run()` differently depending on the situation:

- Running while tired.
- Running while being chased.

---

# 2.3 Types of Polymorphism in Programming

## Static Polymorphism — Compile Time

- Achieved via **Method Overloading**.
- Method selection is determined at compile time.

## Dynamic Polymorphism — Runtime

- Achieved via **Method Overriding**.
- Method selection happens at runtime.

---

# 3. Static Polymorphism — Method Overloading

- Same method name with different parameter lists.
- The overloaded method is resolved at compile time.

### Example

```cpp
class ManualCar {
    void accelerate();       // No parameter
    void accelerate(int speed); // With parameter
};
```

### Purpose

Allows the same behavior to adapt based on the arguments passed.

---

## Rules of Method Overloading

### Method Name

- Must be the **same**.

### Return Type

- Can be same or different.
- **Return type alone cannot be used for method overloading.**

### Parameters

Parameters must vary in:

- Number
- Type

Example:

```cpp
void accelerate();
void accelerate(int speed);
void accelerate(double speed);
```

---

# 4. Dynamic Polymorphism — Method Overriding

- Same method signature is redefined in child classes.
- Achieved using **virtual functions** in C++.
- Method selection is resolved at runtime.

### Example

```cpp
class Car {
    virtual void accelerate() = 0; // Abstract
};

class ManualCar : public Car {
    void accelerate() override; // Manual-specific logic
};

class ElectricCar : public Car {
    void accelerate() override; // Electric-specific logic
};
```

### Concept

The parent class defines **what operation should exist**, while child classes provide their own implementation.

---

# 5. Combined Use of OOP Pillars

Final code demonstrates:

```cpp
// See code section for full code example.
```

### OOP Concepts Used

#### 1. Abstraction

- Hiding implementation details.
- Exposing only necessary functionality.

#### 2. Encapsulation

- Keeping data and behavior together.
- Using private/protected members to control access.

#### 3. Inheritance

- `ManualCar` and `ElectricCar` inherit from `Car`.

#### 4. Polymorphism

- **Method overriding** → Dynamic polymorphism.
- **Method overloading** → Static polymorphism.

---

# 6. Additional Concepts

## Protected

- `protected` members are inaccessible directly from outside the class.
- They are accessible within the class and its child classes.

---

# Questions

## Operator Overloading

## 1. What is **Operator Overloading**?

**Operator Overloading** is a feature in which we define how an existing operator behaves when it is used with **user-defined objects/classes**.

For example:

- `+` → Addition
- `-` → Subtraction
- `*` → Multiplication
- `==` → Comparison
- `<` → Less than
- `>` → Greater than

### Python Example

In Python, operator overloading is implemented using **special/dunder methods**.

For example, we can define the behavior of `+` using `__add__()`:

```python
class Complex:
    def __init__(self, real, imag):
        self.real = real
        self.imag = imag

    # Operator Overloading for +
    def __add__(self, other):
        return Complex(
            self.real + other.real,
            self.imag + other.imag
        )


c1 = Complex(2, 3)
c2 = Complex(4, 5)

c3 = c1 + c2

print(c3.real, "+", c3.imag, "i")
```

### Output

```text
6 + 8 i
```

When Python sees:

```python
c3 = c1 + c2
```

it internally calls:

```python
c1.__add__(c2)
```

Therefore:

```text
c1 + c2
   ↓
c1.__add__(c2)
   ↓
Complex(6, 8)
```

### Common Python Operator Overloading Methods

| Operator | Python Special Method |
|---|---|
| `+` | `__add__()` |
| `-` | `__sub__()` |
| `*` | `__mul__()` |
| `/` | `__truediv__()` |
| `==` | `__eq__()` |
| `<` | `__lt__()` |
| `>` | `__gt__()` |

---

## 2. Why is Operator Overloading available in **C++** but not in **Java/Python**?

The statement **"Operator Overloading is available in C++ but not in Java/Python" is incorrect.**

The correct comparison is:

| Language | Operator Overloading |
|---|---|
| **C++** | ✅ Supported |
| **Java** | ❌ Not supported for user-defined classes |
| **Python** | ✅ Supported |

### C++

C++ provides explicit operator overloading syntax:

```cpp
Complex operator+(Complex &c) {
    return Complex(real + c.real, imag + c.imag);
}
```

### Java

Java does **not** support user-defined operator overloading.

Instead of:

```java
c3 = c1 + c2;
```

we generally use a method:

```java
c3 = c1.add(c2);
```

### Python

Python supports operator overloading through **dunder/special methods**:

```python
def __add__(self, other):
    ...
```

So:

```python
c1 + c2
```

can be customized by implementing:

```python
c1.__add__(c2)
```

### Final Exam/Interview Point

> **C++ and Python support user-defined operator overloading, whereas Java does not.**

---

# 7. Conclusion & Practice

- Understanding OOPs is best done through **real-world relatable examples**.
- Practice by modifying and adding features to the existing car classes.

### Practice Suggestion

Modify the existing car hierarchy by adding features such as:

- New car types.
- New attributes.
- New behaviors.
- Different implementations of existing methods.

---

# Homework

1. **Define Operator Overloading.**
2. **Why is it not supported in Java/Python?**


[Back to the course index](../README.md)
