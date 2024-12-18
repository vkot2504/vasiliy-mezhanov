---
{"dg-publish":true,"permalink":"/pattern-state/pattern-state/"}
---

``` python
from abc import ABC, abstractmethod

# Состояние (State)
class TrafficLightState(ABC):
    @abstractmethod
    def handle(self, context):
        pass

# Конкретные состояния
class RedLightState(TrafficLightState):
    def handle(self, context):
        print("Красный свет. Стоп.")
        context.state = YellowLightState()

class YellowLightState(TrafficLightState):
    def handle(self, context):
        print("Желтый свет. Готовьтесь.")
        context.state = GreenLightState()

class GreenLightState(TrafficLightState):
    def handle(self, context):
        print("Зеленый свет. Езжайте.")
        context.state = RedLightState()

# Контекст (Context)
class TrafficLight:
    def __init__(self):
        self.state = RedLightState()

    def change(self):
        self.state.handle(self)

# Клиентский код
if __name__ == "__main__":
    traffic_light = TrafficLight()

    for _ in range(6):  # Симуляция переключения
        traffic_light.change()
