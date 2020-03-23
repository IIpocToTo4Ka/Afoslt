# Afoslt

## Структура проекта:
+ **\app\\** - Директория содержащая все файлы приложения.
+ **\app\cfg\\** - Директория с конфигурационными файлами.
+ **\app\controllers\\** - Директория с контроллерами приложения*.
+ **\app\controllers\ftbox\\** - Контроллеры поставляемые *'из коробки'*.
+ **\app\core\\** - Ядро приложения.
+ **\app\layouts\\** - Директория с схемами приложения.
+ **\app\views\\** - Директория с представлениями приложения.
+ **\app\views\ftbox\\** - Представления поставляемые *'из коробки'*.
+ **\vendor\\** - Директория содержащая сторонние библиотеки.
+ **\web\\** - Директория с публичными ресурсами.

## Установка:

```
git clone https://github.com/IIpocToTo4Ka/Afoslt
```

## Первичная настройка:

1. Скопировать репозиторий с GitHub.
2. Выбрать источник, соответствующий желаемому языку.
3. Переименовать класс `MyApplication` описываемый в файле `\app\MyApplication.php` в удобный Вам.
4. Изменить создаваемый экземпляр приложения в файле `\index.php`, на экземпляр вашего приложения (из пт. 1).
5. Изменить в **манифесте приложения** данные, на необходимые Вам. Конфигурационный файл, содержащий манифест приложения, располагается по пути `\app\cfg\manifest.php`.
6. Загрузить приложение на локальный или удаленный сервер.
7. Если настройка произведена правильно, то вы увидите главную страницу, при открытии сайта, в вашем браузере.
8. Вы великолепны!

## Добавление нового контроллера:
Чтобы добавить новый контроллер, создайте соответсвтующий файл в директории **\app\controllers\\** и опишите там Ваш класс контроллера.

```
<?php
namespace App\Controllers;

use App\Core\Controller;
use App\Core\View;

// Новый контроллер.
class MyController extends Controller
{
    // Новое действие.
    public function doAction()
    {
        // Создание и отображение представления.
        $this->setAndRender();
    }
}
```
После того как класс контроллера создан и описан, перейдите в директорию **\app\views\\** и создайте там новое представление **\app\views\my\do.php**.
+ Если параметр `dynamic_routes` **манифеста приложения** установлен в значение **true**, то перейдя по запросу **/my/do**, Вы увидите созданный Вами вид.
+ Если параметр `dynamic_routes` установлен в значение **false**, то следует добавить статический маршрут.

## Добавление нового маршрута:

Чтобы добавить статический маршрут, откройте конфигурационный файл маршрутов (**\app\cfg\routes.php**) и добавьте туда новый маршрут.
```
'myview' => ['controller' => 'my', 'action' => 'do'], 
```

## Обработка входящих данных:

+ Для статических маршрутов входящие переменные можно задать следующим образом:

```
'myview/{my_variable}' => ['controller' => 'my', 'action' => 'do'], 
```
При обработке запроса значение, стоящее на месте переменной будет записано в `Application::$arguments` - статическое поле класса приложения. **Ключ** переменной будет соответствовать введенному Вами в маршрут.
Переменных можно добавлять столько, сколько нужно.

+ Для динамических маршрутов входящие переменные будут считываться следующим образом:

Текст запроса: **/my/do/my_variable/value1/my_variable2/value2**

При обработке запроса переменные будут записаны в `Application::$arguments` - статическое поле класса приложения. **Ключи** переменных будут *my_variable* и *my_variable2* соответветственно, а значения *value1* и *value2* соответственно.

+ **GET** запросы можно добавлять как к статическим маршрутам, так и к динамическим. Они будут доступными в массиве `$_GET`. Если параметр `get_to_arguments` **манифеста приложения** установлен в значение **true**, то данные из массива `$_GET` будут скопированны в `Application::$arguments` - статическое поле класса приложения.

+ **POST** запросы так же обрабатываются, как обычно. Они будут доступными в массиве `$_POST`. Если параметр `get_to_arguments` **манифеста приложения** установлен в значение **true**, то данные из массива `$_POST` будут скопированны в `Application::$arguments` - статическое поле класса приложения.

**Важно:** Если при записи в массив `Application::$arguments` из разных источников будут найдены одинаковые ключи, то значение перезапишеться. **Пожалуйста, будте внимательны**.

Приоритет источника (*те, что выше, перезапишут тех, что ниже*):
1. **POST**
1. **GET**
1. **Route**

+ Загружаемые файлы, доступны в массиве `$_FILES`. Если параметр `files_to_app` **манифеста приложения** установлен в значение **true**, то данные из массива `$_FILES` будут скопированны в `Application::$uploadedFiles` - статическое поле класса приложения.