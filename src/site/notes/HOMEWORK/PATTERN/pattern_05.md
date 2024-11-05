---
{"dg-publish":true,"permalink":"/homework/pattern/pattern-05/"}
---

``` python
class HTMLPage:

def generate(self):

"""Метод, который должен быть реализован в дочерних классах для генерации HTML-кода."""

raise NotImplementedError("Этот метод должен быть переопределен")

  
  

class ThemedPage(HTMLPage):

def generate(self):

"""Генерирует HTML-код для страницы с возможностью смены темы."""

return """

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Themed Page</title>

<style>

/* Основные стили для светлой и темной темы */

:root {

--bg-color-light: #ffffff;

--text-color-light: #333333;

--input-bg-light: #f0f0f0;

--button-bg-light: #4CAF50;

--button-text-light: #ffffff;

  

--bg-color-dark: #1a1a1a;

--text-color-dark: #e0e0e0;

--input-bg-dark: #333333;

--button-bg-dark: #ff5733;

--button-text-dark: #ffffff;

}

  

body {

font-family: Arial, sans-serif;

transition: background-color 0.3s, color 0.3s;

margin: 0;

padding: 0;

}

  

/* Стиль для контейнера */

.container {

max-width: 600px;

margin: 5% auto;

padding: 20px;

text-align: center;

}

  

/* Стиль для кнопки */

.theme-toggle-btn {

padding: 10px 20px;

border: none;

border-radius: 5px;

cursor: pointer;

font-size: 16px;

margin-top: 20px;

transition: background-color 0.3s, color 0.3s;

}

  

/* Стиль для полей ввода */

.input-field {

width: 100%;

padding: 10px;

margin-top: 10px;

border: none;

border-radius: 5px;

transition: background-color 0.3s, color 0.3s;

font-size: 16px;

}

  

/* Тема светлая */

.light-theme {

background-color: var(--bg-color-light);

color: var(--text-color-light);

}

.light-theme .input-field {

background-color: var(--input-bg-light);

color: var(--text-color-light);

}

.light-theme .theme-toggle-btn {

background-color: var(--button-bg-light);

color: var(--button-text-light);

}

  

/* Тема темная */

.dark-theme {

background-color: var(--bg-color-dark);

color: var(--text-color-dark);

}

.dark-theme .input-field {

background-color: var(--input-bg-dark);

color: var(--text-color-dark);

}

.dark-theme .theme-toggle-btn {

background-color: var(--button-bg-dark);

color: var(--button-text-dark);

}

</style>

</head>

<body class="light-theme">

<div class="container">

<h1>Themed Page</h1>

<input type="text" class="input-field" placeholder="Enter text...">

<button class="theme-toggle-btn" onclick="toggleTheme()">Switch Theme</button>

</div>

  

<script>

function toggleTheme() {

const body = document.body;

body.classList.toggle('light-theme');

body.classList.toggle('dark-theme');

}

</script>

</body>

</html>

"""

  
  

class ThemeFactory:

@staticmethod

def create_page() -> HTMLPage:

"""Фабричный метод для создания страницы с возможностью смены темы."""

return ThemedPage()

  
  

# Генерация HTML-кода для страницы с возможностью смены темы

def generate_html():

page = ThemeFactory.create_page()

return page.generate()

  
  

# Создание HTML-файла

with open("themed_page.html", "w") as file:

file.write(generate_html())

  

print("HTML-код успешно сгенерирован в файле 'themed_page.html'")