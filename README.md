# Native JavaScript

### 1. Query Selector

__1.1 Поиск по селектору (```.class, #id, a[target=_blank] ...```)__

```javascript
document.querySelectorAll('selector'); // возвращает массив элементов
document.querySelector('selector');    // возвращает первый найденный элемент
```

__1.2 Запрос по классу (```.class```)__

```javascript
document.getElementsByClassName('.class');   // возвращает массив элементов
```

__1.3 Запрос по id (```#class```)__

```javascript
document.getElementById('id')   // возвращает первый найденный элемент
```

__1.4 Найти среди потомков__

```javascript
el.querySelectorAll('element')
```

__1.5 Родственные/Предыдущие/Следующие Элементы__

  + Родственные элементы

  ```javascript
  [].filter.call(el.parentNode.children, function(child) {
    return child !== el;
  });
  ```
  + Предыдущие элементы

  ```javascript
  el.previousElementSibling
  ```
  + Следующие элементы

  ```javascript
  el.nextElementSibling
  ```

__1.6 Closest__

Возвращает первый совпавший элемент по предоставленному селектору, обходя от текущего элементы до документа.

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

__1.7 Родители до__

Получить родителей каждого элемента в текущем сете совпавших элементов, но не включая элемент, совпавший с селектором, узел DOM'а.

```javascript
function parentsUntil(el, selector, filter) {
  const result = [];
  const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

  // Совпадать начиная от родителя
  el = el.parentElement;
  while (el && !matchesSelector.call(el, selector)) {
    if (!filter) {
      result.push(el);
    } else {
      if (matchesSelector.call(el, filter)) {
        result.push(el);
      }
    }
    el = el.parentElement;
  }
  return result;
}
```

---

### 2.Манипуляция с свойствами и атрибутами

__2.1 Input/Textarea__

```javascript
el.value
```

__2.2 Получить индекс e.currentTarget между .radio__

```javascript
[].indexOf.call(document.querySelectorAll('.radio'), e.currentTarget);
```

__2.3 Получить значение атрибута и изменение его__

```javascript
el.getAttribute('foo')
el.setAttribute('foo', 'bar')
```

### 2. СSS and Style
