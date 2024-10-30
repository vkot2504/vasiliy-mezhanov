---
{"dg-publish":true,"permalink":"/homework/pattern/pattern-041/"}
---

Представим, что есть интернет-магазин, в котором можно заказать разные виды товаров: _Книги_ и _Электронику_. Каждый тип товара обрабатывается по-разному. Книги требуют проверки на наличие в библиотеке, а электронные устройства требуют проверки наличия гарантии.
``` python
# Базовый интерфейс для классов товаров
class Product:
    def process_order(self):
        raise NotImplementedError("This method should be overridden in subclasses")


# Фабрика товаров
class ProductFactory:
    @staticmethod
    def create_product(product_type):
        # Используем словарь для хранения различных классов продуктов
        product_classes = {
            'book': ProductFactory._create_book_class(),
            'electronics': ProductFactory._create_electronics_class()
        }
        
        # Возвращаем нужный класс продукта в зависимости от типа
        product_class = product_classes.get(product_type.lower())
        if product_class:
            return product_class()
        else:
            raise ValueError(f"Unknown product type: {product_type}")

    @staticmethod
    def _create_book_class():
        # Динамически создаем класс Book с помощью type
        return type('Book', (Product,), {
            'process_order': lambda self: print("Processing order for a Book: Checking library stock...")
        })

    @staticmethod
    def _create_electronics_class():
        # Динамически создаем класс Electronics с помощью type
        return type('Electronics', (Product,), {
            'process_order': lambda self: print("Processing order for Electronics: Checking warranty...")
        })


# Пример использования фабричного метода
try:
    product1 = ProductFactory.create_product("book")
    product1.process_order()  # Ожидается: Processing order for a Book: Checking library stock...
    
    product2 = ProductFactory.create_product("electronics")
    product2.process_order()  # Ожидается: Processing order for Electronics: Checking warranty...

except ValueError as e:
    print(e)
