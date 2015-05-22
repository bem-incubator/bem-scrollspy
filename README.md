BEM-Scrollspy
=============

Блок для отслеживания скролла страницы. Предназначен для проектов, использующих БЭМ методологию.

## Зависимости

bem-core
  * блок `i-bem` 
  * блок `next-thick`
  * блок `functions` 
  * блок `jquery`
   
## Установка

 1. Достаточно прописать библиотеку как зависимость в вашем `bower.json` и выполнить `bower install` для установки из GitHub-репозитория или Bower регистра.

 2. Добавьте уровень переопределения в файл make.js:

``` javascript
[ 'libs/bem-scrollspy/common.blocks' ]
```

## Использование

Все, что делает блок `scrollspy` — генерирует два БЭМ-события:

  * `scrollin` - когда блок становится виден пользователю;
  * `scrollout` - когда блок скрыватся из виду.
  
Подписавшись на эти события, можно выполнять различные действия. Например, запускать и останавливать анимацию:

````javascript
    var ss = this.findBlockOn('scrollspy');

    ss.on('scrollin', function() { this.setMod('animation', 'progress'); }, this);                  
    ss.on('scrollout', function() { this.setMod('animation', 'stop'); }, this);   
````

В объекте события передается направление скролла. Например:
````javascript
    var ss = this.findBlockOn('scrollspy');
    ss.on('scrollin', this._onScrollIn, this); //подписка на событие
    
    /*.....*/
    
    _onScrollIn: function(e, dir){ //получаем направление скролла вторым параметром
      if (dir === 'down') {
        doAction();
      } else if (dir === 'up') {
        doAnotherAction();
      }
    }
````

## Параметры

Можно задать отступ для блока в пикселях или процентах (по умолчанию 10% от края окна). Можно задать отступ сразу в js-параметрах блока:

````bemjson
  {
    block: 'scrollspy',
    js: { offset: 40 }
  }
````

или использовать метод `setOffset()`:
````javascript
    var ss = this.findBlockInside('scrollspy');
    ss.setOffset(0);
````
Этот метод установит новый размер отступа и пересчитает позицию блока.

Если вам нужно только пересчитать позицию блока, не меняя значений, можно использовать метод `calcOffsets()`.

## Методы блока

| Метод           | Описание                                            |
|-----------------|-----------------------------------------------------| 
| setOffset(val)  | Устанавливает отступ в процентах или пикселях.      |
| calcOffsets()   | Запускает расчет позиции блока.                     |
| activate()      | Ручная активация блока (вызовет событие `scrollin`).|
| deactivate()    | «Ручнной» `scrollout`.                              |
| getDir()        | Возвращает направление последнего скролла.          |
| isActive()      | Проверяет, находится ли блок в зоне просмотра       |