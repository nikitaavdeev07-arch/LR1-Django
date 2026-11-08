# Лабораторная работа № 1

## Создание виртуального окружения. Контроллеры и маршруты

**Дисциплина:** Разработка web-приложений  
**Тема:** Настройка виртуального пространства, разработка контроллера приложения и организация маршрутов  
**Выполнил:** *Авдеев Никита Викторович*  
**Группа:** *ПИЖ-б-о-25-2*  

---

## Цель работы

Настроить виртуальное пространство. Разработать контроллер приложения и организовать маршруты.

---

## Используемое ПО

| Компонент | Версия |
|---|---|
| PyCharm | 3.14.5 |
| Django | 6.1.1 |

---

## Ход выполнения работы

### 1. Создание проекта и виртуального окружения

Создана папка проекта `MyProject`, внутри — папка сайта `MySite`.  
В терминале выполнена команда создания виртуального окружения:

```bash
python -m venv venv
```

В папке проекта появился каталог `venv`.

> Второй способ — автоматическое создание окружения средствами PyCharm при создании проекта  
> (**Python Interpreter: New Virtualenv environment**).

---

### 2. Активация виртуального окружения

```bash
venv\Scripts\activate
```

После активации в начале строки терминала появился префикс `(venv)`:

```
(venv) D:\MyProject>
```

Проверка списка пакетов:

```bash
pip list
```

На данном этапе установлены только `pip` и `setuptools`.

---

### 3. Установка Django

```bash
pip install Django
```

Повторная проверка:

```bash
pip list
```

В списке появились: `asgiref`, `Django`, `sqlparse`, `tzdata` и др.

---

### 4. Создание проекта Django

```bash
django-admin startproject MySite
```

В дереве проекта появилась папка `MySite` с вложенной папкой `MySite` (пакет конфигурации) и файлом `manage.py`.

Переход в папку проекта и запуск отладочного сервера:

```bash
cd MySite
python manage.py runserver
```

В терминале:

```
Starting development server at http://127.0.0.1:8000/
```

При открытии адреса в браузере отобразилась стартовая страница Django «The install worked successfully!».  
При первом запуске создан файл базы данных `db.sqlite3`.

---

### 5. Создание приложения `news`

```bash
python manage.py startapp news
```

В проекте появилась папка `news` с файлами `views.py`, `models.py`, `apps.py` и др.

---

### 6. Регистрация приложения

В файле `MySite/settings.py` в список `INSTALLED_APPS` добавлена строка:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'news.apps.NewsConfig',
]
```

---

### 7. Создание контроллеров (views)

Файл `news/views.py`:

```python
from django.shortcuts import render
from django.http import HttpResponse


def index(request):
    return HttpResponse('Hello world')


def test(request):
    return HttpResponse('<h1>Тестовая страница</h1>')
```

- `index` — возвращает строку «Hello world»;
- `test` — возвращает HTML-заголовок «Тестовая страница».

---

### 8. Настройка маршрутов

#### 8.1. Главный `urls.py`

Файл `MySite/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('news/', include('news.urls')),
]
```

#### 8.2. Маршруты приложения

Создан файл `news/urls.py`:

```python
from django.urls import path
from .views import *

urlpatterns = [
    path('', index),
    path('test/', test),
]
```

> В PyCharm папка `MySite` помечена как **Sources Root**  
> (правой кнопкой → **Mark Directory as → Sources Root**), чтобы IDE корректно разрешала импорты.

---

### 9. Проверка работы

Запуск сервера:

```bash
python manage.py runserver
```

| Адрес | Результат |
|---|---|
| `http://127.0.0.1:8000/news/` | **Hello world** |
| `http://127.0.0.1:8000/news/test/` | **Тестовая страница** |
| `http://127.0.0.1:8000/admin/` | Страница входа в админку Django |

В терминале при каждом запросе отображаются HTTP-запросы:

```
[09/Nov/2026 12:00:00] "GET /news/ HTTP/1.1" 200 11
[09/Nov/2026 12:00:05] "GET /news/test/ HTTP/1.1" 200 34
```

---

## Структура проекта

```
MyProject/
├── .venv/                      # виртуальное окружение
└── MySite/                     # корень проекта Django
    ├── manage.py
    ├── db.sqlite3
    ├── MySite/                 # пакет конфигурации
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   ├── asgi.py
    │   └── wsgi.py
    └── news/                   # приложение
        ├── __init__.py
        ├── admin.py
        ├── apps.py
        ├── models.py
        ├── views.py
        └── urls.py
```

---

## Основные команды, использованные в работе

| Команда | Назначение |
|---|---|
| `python -m venv venv` | создание виртуального окружения |
| `venv\Scripts\activate` | активация окружения (Windows) |
| `pip list` | список установленных пакетов |
| `pip install Django` | установка Django |
| `django-admin startproject MySite` | создание проекта |
| `python manage.py startapp news` | создание приложения |
| `python manage.py runserver` | запуск отладочного сервера |
| `python manage.py check` | проверка проекта на ошибки |
| `Ctrl+C` | остановка сервера |

---

## Вывод

В ходе лабораторной работы:

1. Создано и активировано виртуальное окружение `venv`.
2. Установлен Django (версия 6.1.1).
3. Создан проект `MySite` и приложение `news`.
4. Приложение зарегистрировано в `INSTALLED_APPS` файла `settings.py`.
5. Разработаны контроллеры `index` и `test` в `news/views.py`.
6. Организованы маршруты: корневой `urls.py` подключает маршруты приложения через `include`, а `news/urls.py` сопоставляет адреса `''` и `'test/'` соответствующим контроллерам.
7. Работа проверена через отладочный сервер Django: страницы `/news/` и `/news/test/` возвращают корректные ответы (`HTTP 200`).

Получены практические навыки настройки виртуального окружения, создания проекта и приложения Django, разработки контроллеров и организации маршрутизации.

---

## Литература

1. Документация Django. — URL: https://docs.djangoproject.com/
2. PyPI — The Python Package Index. — URL: https://pypi.org/
3. Методические указания к лабораторной работе № 1 «Создание виртуального окружения. Контроллеры и маршруты».
