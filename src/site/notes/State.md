---
{"dg-publish":true,"permalink":"/state/"}
---

**ФИО:** 2022-ФГиИБ-ИБ-1б Воронов Даниил Алексеевич 

# Case

Представим себе автомат, который продает напитки. Автомат может находиться в различных состояниях, таких как "Ожидание выбора", "Ожидание оплаты", "Выдача напитка" и "Ошибка". Каждое состояние определяет, как автомат реагирует на действия пользователя.

```python
class VendingMachine:
    def __init__(self):
        self.state = WaitingForSelection(self)

    def set_state(self, state):
        self.state = state

    def select_drink(self):
        self.state.select_drink()

    def insert_money(self, amount):
        self.state.insert_money(amount)

    def dispense_drink(self):
        self.state.dispense_drink()

    def error(self):
        self.state.error()


class State:
    def select_drink(self):
        raise NotImplementedError

    def insert_money(self, amount):
        raise NotImplementedError

    def dispense_drink(self):
        raise NotImplementedError

    def error(self):
        raise NotImplementedError


class WaitingForSelection(State):
    def __init__(self, vending_machine):
        self.vending_machine = vending_machine

    def select_drink(self):
        print("Выберите напиток.")
        # Логика выбора напитка
        self.vending_machine.set_state(WaitingForPayment(self.vending_machine))


class WaitingForPayment(State):
    def __init__(self, vending_machine):
        self.vending_machine = vending_machine
        self.amount_inserted = 0

    def insert_money(self, amount):
        self.amount_inserted += amount
        print(f"Внесено: {self.amount_inserted} рублей.")
        # Логика проверки суммы
        if self.amount_inserted >= 50:  
            self.vending_machine.set_state(DispensingDrink(self.vending_machine))
        else:
            print("Недостаточно средств.")

    def select_drink(self):
        print("Сначала внесите деньги.")


class DispensingDrink(State):
    def __init__(self, vending_machine):
        self.vending_machine = vending_machine

    def dispense_drink(self):
        print("Напиток выдан.")
        self.vending_machine.set_state(WaitingForSelection(self.vending_machine))


class Error(State):
    def __init__(self, vending_machine):
        self.vending_machine = vending_machine

    def error(self):
        print("Произошла ошибка. Пожалуйста, обратитесь к обслуживающему персоналу.")
        # Логика обработки ошибки
        self.vending_machine.set_state(WaitingForSelection(self.vending_machine))


# Пример использования
vending_machine = VendingMachine()
vending_machine.select_drink()  # Выбор напитка
vending_machine.insert_money(20)  # Внесение денег
vending_machine.insert_money(30)  # Внесение денег
vending_machine.dispense_drink()  # Выдача напитка
```

