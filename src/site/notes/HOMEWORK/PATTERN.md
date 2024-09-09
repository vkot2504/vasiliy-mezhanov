---
{"dg-publish":true,"permalink":"/homework/pattern/","tags":["gardenEntry"]}
---

```
from datetime import datetime

import re

from typing import List, Dict

class SortingStrategy:

    def sort(self, items: List[Dict[str, str]], rate: float = None) -> List[Dict[str, str]]:

        pass

class NameSortingStrategy(SortingStrategy):

    def sort(self, items: List[Dict[str, str]], rate: float = None) -> List[Dict[str, str]]:

        return sorted(items, key=lambda x: x['name'].lower())

class PriceSortingStrategy(SortingStrategy):

    def sort(self, items: List[Dict[str, str]], rate: float) -> List[Dict[str, str]]:

        def price_to_rub(value: str) -> float:

            if value.startswith('$'):

                return int(value[1:]) * rate 

            return int(value[3:])

        return sorted(items, key=lambda x: price_to_rub(x['price']))

class DateSortingStrategy(SortingStrategy):

    def sort(self, items: List[Dict[str, str]], rate: float = None) -> List[Dict[str, str]]:

        return sorted(items, key=lambda x: datetime.strptime(x['date'], '%d.%m.%Y'))

class Sorter:

    def __init__(self, strategy: SortingStrategy):

        self.strategy = strategy

    def sort(self, items: List[Dict[str, str]], rate: float = None) -> List[Dict[str, str]]:

        return self.strategy.sort(items, rate)

rate = 100 

Ну, и использование

sorter = Sorter(NameSortingStrategy())

print("Sorted by Name:", sorter.sort(items))

ИЛИ

sorter = Sorter(PriceSortingStrategy())

print("Sorted by Price:", sorter.sort(items, rate))

ИЛИ

sorter = Sorter(DateSortingStrategy())

print("Sorted by Date:", sorter.sort(items))




