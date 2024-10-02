---
{"dg-publish":true,"permalink":"/pattern-2/"}
---


### Function
``` python
from tabulate import tabulate

renderers = {
    'abstract': lambda data: print('Not implemented'),

    'console': lambda data: print(tabulate(data, headers="keys")),

    'web': lambda data: print(
        '<table><tr>' +
        ''.join(f'<th>{key}</th>' for key in data[0].keys()) +
        '</tr>' +
        ''.join('<tr>' + ''.join(f'<td>{row[key]}</td>' for key in row) + '</tr>' for row in data) +
        '</table>'
    ),

    'markdown': lambda data: print(
        '|' + '|'.join(data[0].keys()) + '|\n' +
        '|' + '|'.join(['---'] * len(data[0])) + '|\n' +
        ''.join('|' + '|'.join(str(row[key]) for key in row) + '|\n' for row in data)
    ),
}

def context(renderer_name):
    renderer = renderers.get(renderer_name, renderers['abstract'])
    return lambda data: renderer(data)

# Usage

png = context('png')
con = context('console')
web = context('web')
mkd = context('markdown')

persons = [
    {'name': 'Marcus Aurelius', 'city': 'Rome', 'born': 121},
    {'name': 'Victor Glushkov', 'city': 'Rostov on Don', 'born': 1923},
    {'name': 'Ibn Arabi', 'city': 'Murcia', 'born': 1165},
    {'name': 'Mao Zedong', 'city': 'Shaoshan', 'born': 1893},
    {'name': 'Rene Descartes', 'city': 'La Haye en Touraine', 'born': 1596},
]

print('Unknown Strategy:')
png(persons)

print('\nConsoleRenderer:')
con(persons)

print('\nWebRenderer:')
web(persons)

print('\nMarkdownRenderer:')
mkd(persons)
```

### Class
``` python
class Renderer:
    def render(self, data):
        print('Not implemented')

class ConsoleRenderer(Renderer):
    def render(self, data):
        for row in data:
            print(row)

class WebRenderer(Renderer):
    def render(self, data):
        keys = data[0].keys()
        output = ['<table><tr>']
        output.append(''.join(f'<th>{key}</th>' for key in keys))
        output.append('</tr>')
        for row in data:
            output.append('<tr>' + ''.join(f'<td>{row[key]}</td>' for key in keys) + '</tr>')
        output.append('</table>')
        print(''.join(output))

class MarkdownRenderer(Renderer):
    def render(self, data):
        keys = data[0].keys()
        output = ['|' + '|'.join(keys) + '|']
        output.append('|' + '|'.join('---' for _ in keys) + '|')
        for row in data:
            output.append('|' + '|'.join(str(row[key]) for key in keys) + '|')
        print('\n'.join(output))

class Context:
    def __init__(self, renderer):
        self.renderer = renderer

    def process(self, data):
        return self.renderer.render(data)

# Использование

non = Context(Renderer())
con = Context(ConsoleRenderer())
web = Context(WebRenderer())
mkd = Context(MarkdownRenderer())

persons = [
    {'name': 'Marcus Aurelius', 'city': 'Rome', 'born': 121},
    {'name': 'Victor Glushkov', 'city': 'Rostov on Don', 'born': 1923},
    {'name': 'Ibn Arabi', 'city': 'Murcia', 'born': 1165},
    {'name': 'Mao Zedong', 'city': 'Shaoshan', 'born': 1893},
    {'name': 'Rene Descartes', 'city': 'La Haye en Touraine', 'born': 1596},
]

print('Abstract Strategy:')
non.process(persons)

print('\nConsoleRenderer:')
con.process(persons)

print('\nWebRenderer:')
web.process(persons)

print('\nMarkdownRenderer:')
mkd.process(persons)
``` 


## Кейс с применением паттерна Стратегия 

 Рассмотрим реальный пример использования паттерна "Стратегия" на примере системы онлайн-оплат. У нас есть интернет-магазин, который позволяет клиентам выбирать различные способы оплаты: кредитная карта, PayPal или криптовалюты.

Представим, что у нас есть интернет-магазин, и клиент хочет оплатить товар. Для этого мы предоставляем несколько вариантов оплаты: через кредитную карту, PayPal и криптовалюты. Каждый метод имеет свою логику обработки платежа. Паттерн "Стратегия" позволит нам гибко выбрать нужную стратегию оплаты в зависимости от выбора клиента.

``` python
# Стратегии оплаты
class PaymentStrategy:
    def pay(self, amount):
        raise NotImplementedError("Payment method not implemented")

# Оплата кредитной картой
class CreditCardPayment(PaymentStrategy):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def pay(self, amount):
        print(f'Processing credit card payment of ${amount} using card number {self.card_number}')

# Оплата через PayPal
class PayPalPayment(PaymentStrategy):
    def __init__(self, email):
        self.email = email
    
    def pay(self, amount):
        print(f'Processing PayPal payment of ${amount} using account {self.email}')

# Оплата криптовалютой
class CryptoPayment(PaymentStrategy):
    def __init__(self, wallet_address):
        self.wallet_address = wallet_address
    
    def pay(self, amount):
        print(f'Processing crypto payment of ${amount} using wallet {self.wallet_address}')

# Контекст, который использует разные стратегии оплаты
class PaymentContext:
    def __init__(self, payment_strategy):
        self.payment_strategy = payment_strategy
    
    def process_payment(self, amount):
        self.payment_strategy.pay(amount)

# Пример использования
if __name__ == "__main__":
    # Клиент хочет оплатить кредитной картой
    credit_card_payment = PaymentContext(CreditCardPayment("1234-5678-9876-5432"))
    credit_card_payment.process_payment(100)

    # Клиент хочет оплатить через PayPal
    paypal_payment = PaymentContext(PayPalPayment("user@example.com"))
    paypal_payment.process_payment(200)

    # Клиент хочет оплатить криптовалютой
    crypto_payment = PaymentContext(CryptoPayment("1A2b3C4D5e6F"))
    crypto_payment.process_payment(300)
```

