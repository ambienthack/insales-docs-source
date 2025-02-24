# Вводная

Раздел посвящен библиотеке `common.v2.js`, которая является набором готовых скриптов для упрощения и ускорения разработки шаблонов на платформе inSales.

## Модули

**[Товар:](/common.v2.js/2Products/)**

- Назначение атрибутов
- Селектор модификаций
- Методы класса Products
- События

**[Корзина:](/common.v2.js/3Cart/)**

- Назначение атрибутов
- Методы и события

**[Живой поиск по сайту:](/common.v2.js/4AjaxSearch/)**

- Назначение атрибутов
- Методы класса AjaxSearch
- События

**[Сравнение:](/common.v2.js/5Compare/)**

- Назначение атрибутов
- Методы класса Compare
- События

**[Избранное:](/common.v2.js/6FavoritesProducts/)**

- Назначение атрибутов
- Методы класса FavoritesProducts
- События

**[Шина событий EventBus:](/common.v2.js/7EventBus/)**

- Подписка на события
- Создание событий
- Отладка событий

**[Модуль для работы с API магазина:](/common.v2.js/8ajaxAPI/)**

- Инструмент для получения и обновления данных о товарах, содержимого корзины, клиентах, категориях
- Отправка сообщений в формах обратной связи, отзывов, комментариев к статьям
- Отправка формы заказа

**[AJAX-фильтры:](/common.v2.js/9ui-ajax-filters/)**

- Принцип работы
- Пример разметки

**[Форма обратной связи:](/common.v2.js/10ui-feedback/)**

- Назначение атрибутов
- События
- Пример разметки

**[Lodash шаблоны:](/common.v2.js/12Template/)**

- Работа с динамическими данными через шаблонизатор библиотеки Lodash

## Подключение

В шаблоны `layots.{layout,checkout2,client_account}.liquid` добавить тег `include_insales_scripts` с параметром `common-js@v2`

```
{% include_insales_scripts "common-js@v2" %}
```

Так же можно подключить через settings_data.json, прописав там поле - "common_js_version": "v2".

Файл settings_data.json недоступен через админ-панель, поэтому новое свойство нужно добавлять вручную путём скачивания шаблона и последующей установки с новыми параметрами. Также файл можно поправить, если для разработки используется [InSales-uploader](https://insales.github.io/insales-uploader/){:target="_blank"}.

Пример `settings_data.json`:
```json
{
  "common_js_version": "v2",
  "presets": {
    "custom": {
      "color_text_primary": "#222222",
      "font_size_primary": "16px"
    }
  },
  "theme_title": "Базовый шаблон"
}
```

!!! info
    В шаблонах 4 поколения common-js указывается в качестве зависимости для виджетов

## Библиотеки используемые в Common.js

- [Lodash](https://lodash.com/docs/){:target="_blank"}
- [jQuery](http://jquery.com/){:target="_blank"}

Lodash доступен глобально, можно и нужно пользоваться возможностями этой библиотеки при разработке.

## Зарезервированные переменные

Имена переменных которые Common.js присваивает в глобальную область видимости:

- Cart
- Shop
- Products
- Compare
- FavoritesProducts
- Site
- AjaxSearch
- ajaxAPI
- Template
- EventBus

В проектах, где используется Common.js, нельзя переопределять данные переменные. Также при подключении Common.js к работающему сайту нужно проверять переменные на переопределение и особенно обратить внимание на переменные `Cart` и `Site`.

## Логика работы

Готовые решения Common.js для компонентов магазина работают по следующей логике:

1. В разметку компонента добавляются элементы с обязательными data-атрибутами. Далее в эти атрибуты через Liquid-переменные передаются данные из магазина. Также есть data-атрибуты, которые реализуют действия (кнопки добавления или удаления товара из корзины, переключение вариантов, изменение количества и т.д.).
2. Common.js при загрузке страницы пробегает по элементам с data-атрибутам и на основе полученных данных инициализирует скрипты для работы компонентов. 
3. Практически на каждое взаимодействие с компонентами, реализованными с помощью фреймворка (обновление корзины, переключение модификации и т.д.), можно повесить обработчик, в callback которого приходит информация о событии. Например, при обновлении корзины в callback попадает информация об актуальном состоянии корзины, исходя из чего можно сделать динамический виджет корзины или обновление информации на странице корзины без перезагрузки страницы.

Кроме того стоит ознакомиться с [подробным описанием API фреймворка Common.js](/common.v2.js/ajaxAPI/).

API фреймворка предоставляет удобные и протестированные методы для разработки своих компонентов.

Все обращения к API нужно производить после события `DOMContentLoaded`, оно же `$(document).ready(function() {}), $(function() {})`.
