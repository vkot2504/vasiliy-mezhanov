---
{"dg-publish":true,"permalink":"/homework/pattern/pattern-03/"}
---

# 1
 
 ``` python 
class ArrayToQueueAdapter(list):
    def enqueue(self, data):
        self.append(data)  # аналог push в Python

    def dequeue(self):
        return self.pop()  # удаление последнего элемента

    @property
    def count(self):
        return len(self)  # возвращает длину списка


# Пример использования

queue = ArrayToQueueAdapter()
queue.enqueue('uno')
queue.enqueue('due')
queue.enqueue('tre')

while queue.count:
    print(queue.dequeue())
```

# 2
 ``` python 
 def array_to_queue_adapter():
    array = []
    
    # Добавляем методы к списку
    array.enqueue = lambda data: array.append(data)
    array.dequeue = lambda: array.pop()
    array.count = lambda: len(array)
    
    return array


# Пример использования

queue = array_to_queue_adapter()
queue.enqueue('uno')
queue.enqueue('due')
queue.enqueue('tre')

while queue.count():
    print(queue.dequeue())
    ```

# 3
 ``` python 
import asyncio

# Функция для преобразования функции с колбэком в асинхронную функцию с поддержкой таймаута
def promisify(fn):
    async def wrapper(*args, timeout=None):
        loop = asyncio.get_event_loop()
        
        def callback(resolve, reject):
            def inner_callback(err, data):
                if err:
                    reject(err)
                else:
                    resolve(data)
            return inner_callback

        promise = loop.create_future()

        def resolve(data):
            if not promise.done():
                promise.set_result(data)

        def reject(err):
            if not promise.done():
                promise.set_exception(err)
        
        # Вызов оригинальной функции с колбэком
        fn(*args, callback(resolve, reject))
        
        # Если указан таймаут, то отменяем по истечению времени
        if timeout is not None:
            try:
                return await asyncio.wait_for(promise, timeout)
            except asyncio.TimeoutError:
                raise TimeoutError(f"Operation timed out after {timeout} seconds")
        
        return await promise
    
    return wrapper

# Пример использования

import aiofiles

async def read_file(file_name, encoding, callback):
    try:
        async with aiofiles.open(file_name, mode='r', encoding=encoding) as f:
            data = await f.read()
            callback(None, data)  # Вызываем колбэк без ошибки
    except Exception as e:
        callback(e, None)  # Вызываем колбэк с ошибкой

# Преобразуем функцию чтения файла
read = promisify(read_file)

# Главная функция
async def main():
    file_name = '1-promisify.js'
    
    try:
        # Чтение файла с таймаутом 2 секунды
        data = await read(file_name, 'utf8', timeout=2)
        print(f'File "{file_name}" size: {len(data)}')
    except TimeoutError as e:
        print(e)
    except Exception as e:
        print(f"Error reading file: {e}")

# Запускаем асинхронный цикл
asyncio.run(main())
```

# Кейс - реализация паттерна Адаптер

Пример использования паттерна **Адаптер** на Python можно привести в контексте интеграции старого и нового API для работы с платежными системами. Допустим, у нас есть старая платежная система, которая использует устаревший API, и нам нужно подключить её к новой системе, но без переписывания существующего кода.
 ``` python 
# Старая платежная система (старый API)
class OldPaymentSystem:
    def pay(self, amount):
        print(f"Произведена оплата через старую систему на сумму: {amount} рублей")


# Новая платежная система (новый API)
class NewPaymentSystem:
    def make_payment(self, amount, currency="RUB"):
        print(f"Произведена оплата через новую систему на сумму: {amount} {currency}")


# Адаптер для новой платежной системы
class PaymentAdapter:
    def __init__(self, new_payment_system):
        self.new_payment_system = new_payment_system

    def pay(self, amount):
        # Преобразуем вызов метода старого интерфейса в новый
        self.new_payment_system.make_payment(amount)


# Клиентский код, который использует старый интерфейс
def process_payment(payment_system, amount):
    payment_system.pay(amount)


# Пример использования
if __name__ == "__main__":
    # Используем старую систему
    old_payment = OldPaymentSystem()
    process_payment(old_payment, 100)

    # Используем новую систему через адаптер
    new_payment = NewPaymentSystem()
    adapter = PaymentAdapter(new_payment)
    process_payment(adapter, 250)
     ``` 
 