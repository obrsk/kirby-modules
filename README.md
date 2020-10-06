![Kirby Modules](https://user-images.githubusercontent.com/7975568/93752618-37d29000-fbff-11ea-8276-abd679ef92ae.png)

Этот плагин упрощает создание модульных веб-сайтов с помощью Kirby.

## Особенности

- Модули объединены в `site/modules` и зарегистрированы как обычные blueprints и templates.
- Каждый модуль доступен для создания в разделе `modules` без редактирования любого другого файла.
- К модулям нельзя получить доступ напрямую, они автоматически перенаправляют на родительскую страницу с анкором.
- Контейнер страницы создается автоматически и скрыт на панели.
- Вы можете предварительно просмотреть черновики модулей на их родительских страницах с помощью кнопки предварительного просмотра на панели.

![Preview](https://user-images.githubusercontent.com/7975568/94016693-bb7eaf00-fdae-11ea-8114-f0862391ff91.gif)

## Что такое модуль?

Модуль - это обычная страница, которая отличается от других страниц тем, что находится внутри контейнера модулей.
Такой подход позволяет использовать страницы как модули без ущерба для обычных подстраниц.

```
📄 Page
  📄 Subpage A
  📄 Subpage B
  🗂 Modules
    📄 Module A
    📄 Module B
```

Blueprints и Templates модулей находятся в отдельной папке site/modules. Таким образом, вы можете легко повторно использовать модули в проектах и делиться ими с другими людьми.

## Инструкции

Добавьте раздел `modules` к любому blueprint страницы, и контейнер модулей будет создан автоматически.
 
Вы можете создавать модули, помещая их в папку `site/modules`. Например, вы можете добавить папку `site/modules/text` с template `text.php` и blueprint `text.yml`.

Затем в template родительской страницы вы можете использовать `<?php $page->renderModules() ?>`для рендеринга модулей.

### Родительская страница

#### `site/blueprints/pages/default.yml`

```yml
title: Default Page
sections:
  modules: true
```

#### `site/templates/default.php`

```php
<?php $page->renderModules() ?>
```

### Пример модуля

#### `site/modules/text/text.yml`

```yml
title: Text Module
fields:
  textarea: true
```

#### `site/modules/text/text.php`

```php
<div class="module" class="<?= $module->moduleId() ?>" id="<?= $module->uid() ?>">
  <h1><?= $module->title() ?></h1>
  <?= $module->text()->kt() ?>
</div>
```

Вы можете получить доступ к объекту модуля страницы с помощью `$module` и к объекту родительской страницы с помощью `$page`.
Метод `$module->moduleId()` возвращает идентификатор ID модуля, например `module_text` или `module_gallery`.

## Параметры

### Blueprint модуля по умолчанию

По умолчанию модуль `text` будет first/default опцией в модальном окне "Add page".
Вы можете перезаписать его в своем `site/config/config.php`:

```php
return [
  'medienbaecker.modules.default' => 'gallery'
];
```

### Автопубликация модулей

Вы можете включить автоматическую публикацию модулей в вашем `site/config/config.php`:

```php
return [
  'medienbaecker.modules.autopublish' => true
];
```

### Кастомная модель модуля

Этот плагин создает модель `ModulePage`, перезаписывая определенные методы.
Вы можете расширить эту модель своей собственной моделью:

```php
// site/config/config.php

return [
  'medienbaecker.modules.model' => 'CustomModulePage'
];
```

```php
// site/models/module.php

class CustomModulePage extends ModulePage {
  // methods...
}
```
