# VanillaJS

### 1. Query Selector

__1.1 Поиск по селектору (```.class, #id, a[target=_blank] ...```)__

___jQuery___

```javascript
$('selector')
```
___Native___

```javascript
document.querySelectorAll('selector'); // возвращает массив элементов
document.querySelector('selector');    // возвращает первый найденный элемент
```

__1.2 Запрос по классу (```.class```)__

___jQuery___

```javascript
$('.class')
```
___Native___

```javascript
document.getElementsByClassName('.class');   // возвращает массив элементов
```

__1.3 Запрос по id (```#class```)__

___jQuery___

```javascript
$('#id')
```
___Native___

```javascript
document.getElementById('id')   // возвращает первый найденный элемент
```

__1.4 Найти среди потомков__

___jQuery___

```javascript
$el.find('element')
```

___Native___

```javascript
el.querySelectorAll('element')
```

__1.5 Родственные/Предыдущие/Следующие Элементы__

  + Родственные элементы

  ___jQuery___

  ```javascript
  $el.siblings()
  ```

  ___Native___

  ```javascript
  [].filter.call(el.parentNode.children, function(child) {
    return child !== el;
  });
  ```
  + Предыдущие элементы

  ___jQuery___

  ```javascript
  $el.prev()
  ```

  ___Native___

  ```javascript
  el.previousElementSibling
  ```
  + Следующие элементы

  ___jQuery___

  ```javascript
  $el.next()
  ```

  ___Native___

  ```javascript
  el.nextElementSibling
  ```

__1.6 Closest__

Возвращает первый совпавший элемент по предоставленному селектору, обходя от текущего элементы до документа.

___jQuery___

```javascript
$el.closest(selector)
```

___Native___

```javascript
el.closest(selector)

// IE и Edge
(function() {
  // проверяем поддержку
  if (!Element.prototype.closest) {
    // реализуем
    Element.prototype.closest = function(css) {
      var node = this;

      while (node) {
        if (node.matches(css)) return node;
        else node = node.parentElement;
      }
      return null;
    };
  }

})();
```
