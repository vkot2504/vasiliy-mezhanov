---
{"dg-publish":true,"permalink":"/homework/pattern/pattern-031/"}
---

Представим ситуацию: у нас есть приложение для заботиты о своих питомцах, отслеживания их расписания и потребностей. Пользователь может завести разных питомцев, и для каждого типа животного нужно обеспечить специфическую информацию и особенности ухода. Допустим, приложение должно поддерживать собак, кошек и попугаев. Каждое животное будет иметь уникальные характеристики, такие как звук, который оно издает, и предпочтения в уходе.

Здесь паттерн "Фабрика" поможет создать разные классы животных, каждый из которых будет представлять отдельный вид, предоставляя единый интерфейс для взаимодействия с каждым питомцем.

Если пользователь выберет "собака", то фабрика вернет объект `Dog`, который будет выдавать лай и инструкции по выгулу и корму. Аналогично, если выбрать "кошка" или "попугай", будут созданы объекты соответствующих классов, предоставляющие свои инструкции по уходу и звуки.

 ``` python 
 from abc import ABC, abstractmethod

# 1. Абстрактный класс Animal с общими методами
class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass
    
    @abstractmethod
    def care_instructions(self):
        pass

# 2. Классы для конкретных животных
class Dog(Animal):
    def sound(self):
        return "Гав-гав!"
    
    def care_instructions(self):
        return "Собаке нужен выгул дважды в день и качественный корм."

class Cat(Animal):
    def sound(self):
        return "Мяу!"
    
    def care_instructions(self):
        return "Кошке нужно место для сна и когтеточка."

class Parrot(Animal):
    def sound(self):
        return "Чирик-чирик!"
    
    def care_instructions(self):
        return "Попугаю нужна большая клетка и регулярное общение."

# 3. Фабрика для создания объектов животных
class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "собака":
            return Dog()
        elif animal_type == "кошка":
            return Cat()
        elif animal_type == "попугай":
            return Parrot()
        else:
            raise ValueError("Неизвестный тип животного")

# 4. Пример использования фабрики
def main():
    animal_type = input("Выберите животное (собака, кошка, попугай): ").strip().lower()
    
    try:
        animal = AnimalFactory.create_animal(animal_type)
        print(f"Ваш питомец издает звук: {animal.sound()}")
        print(f"Рекомендации по уходу: {animal.care_instructions()}")
    except ValueError as e:
        print(f"Ошибка: {e}")

if __name__ == "__main__":
    main()
