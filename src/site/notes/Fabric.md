---
{"dg-publish":true,"permalink":"/fabric/"}
---

Система для управления транспортными средствами с использованием паттерна "Фабрика". Создадим классы для различных типов транспортных средств, таких как автомобили, велосипеды и мотоциклы, и будем использовать фабрику для их создания.

```python
class Vehicle:
    def drive(self):
        raise NotImplementedError("This method should be overridden.")


class Car(Vehicle):
    def __init__(self, brand):
        self.brand = brand

    def drive(self):
        return f"Driving a car: {self.brand}"


class Bicycle(Vehicle):
    def __init__(self, brand):
        self.brand = brand

    def drive(self):
        return f"Riding a bicycle: {self.brand}"


class Motorcycle(Vehicle):
    def __init__(self, brand):
        self.brand = brand

    def drive(self):
        return f"Riding a motorcycle: {self.brand}"

class VehicleFactory:
    @staticmethod
    def create_vehicle(vehicle_type, brand):
        if vehicle_type == 'car':
            return Car(brand)
        elif vehicle_type == 'bicycle':
            return Bicycle(brand)
        elif vehicle_type == 'motorcycle':
            return Motorcycle(brand)
        else:
            raise ValueError(f"Unknown vehicle type: {vehicle_type}")

def main():
    vehicles = [
        ('car', "Toyota"),
        ('bicycle', "Giant"),
        ('motorcycle', "Harley-Davidson")
    ]

    for vehicle_type, brand in vehicles:
        vehicle = VehicleFactory.create_vehicle(vehicle_type, brand)
        print(vehicle.drive())

if __name__ == '__main__':
    main()

```

Вывод:
```python
Driving a car: Toyota
Riding a bicycle: Giant
Riding a motorcycle: Harley-Davidson
```
