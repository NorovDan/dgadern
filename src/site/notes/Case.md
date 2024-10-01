---
{"dg-publish":true,"permalink":"/case/"}
---

Реализация паттерна "Стратегия" для различных способов расчета цены товара с учетом разных налоговых ставок

```python
from abc import ABC, abstractmethod

class PricingStrategy(ABC):
    @abstractmethod
    def calculate_price(self, base_price: float) -> float:
        pass

class NoTaxStrategy(PricingStrategy):
    def calculate_price(self, base_price: float) -> float:
        return base_price

class StandardTaxStrategy(PricingStrategy):
    def calculate_price(self, base_price: float) -> float:
        return base_price * 1.2  # 20% налог

class ReducedTaxStrategy(PricingStrategy):
    def calculate_price(self, base_price: float) -> float:
        return base_price * 1.1  # 10% налог

class ShoppingCart:
    def __init__(self, strategy: PricingStrategy):
        self.strategy = strategy
        self.items = []

    def add_item(self, item_name: str, base_price: float):
        self.items.append((item_name, base_price))

    def calculate_total(self) -> float:
        total = 0
        for item_name, base_price in self.items:
            total += self.strategy.calculate_price(base_price)
        return total

    def set_pricing_strategy(self, strategy: PricingStrategy):
        self.strategy = strategy

def main():
    cart = ShoppingCart(NoTaxStrategy())
    
    cart.add_item("Book", 10.0)
    cart.add_item("Pen", 2.0)

    print(f"Total price with no tax: {cart.calculate_total()}")

    # Меняем стратегию на стандартный налог
    cart.set_pricing_strategy(StandardTaxStrategy())
    
    print(f"Total price with standard tax: {cart.calculate_total()}")

    # Меняем стратегию на сниженный налог
    cart.set_pricing_strategy(ReducedTaxStrategy())
    
    print(f"Total price with reduced tax: {cart.calculate_total()}")

if __name__ == "__main__":
    main()

