# Native JavaScript

### Query Selector

__Поиск по селектору (```.class, #id, a[target=_blank] ...```)__

```javascript
document.querySelectorAll('selector'); // возвращает массив элементов
document.querySelector('selector');    // возвращает первый найденный элемент
```

__Запрос по классу (```.class```)__

```javascript
document.getElementsByClassName('.class');   // возвращает массив элементов
```

__Запрос по id (```#class```)__

```javascript
document.getElementById('id')   // возвращает первый найденный элемент
```

__Найти среди потомков__

```javascript
el.querySelectorAll('element')
```

__Родственные/Предыдущие/Следующие Элементы__

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

__Closest__

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

__Родители до__

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

### Манипуляция с свойствами и атрибутами

__Input/Textarea__

```javascript
el.value
```

__Получить индекс e.currentTarget между .radio__

```javascript
[].indexOf.call(document.querySelectorAll('.radio'), e.currentTarget);
```

__Получить значение атрибута и изменение его__

```javascript
el.getAttribute('foo')
el.setAttribute('foo', 'bar')
```

### СSS и операция с классами

__Получить стили__

```javascript
  // ЗАМЕТКА: Известная ошибка, возвращает 'auto' если значение стиля 'auto'
  const win = el.ownerDocument.defaultView;
  // null означает не возвращать псевдостили
  win.getComputedStyle(el, null).color;
```

__Присвоение style__

```javascript
  el.style.color = '#f01';
```

__Добавить/Удалить/Переключить/Проверить на сушествование class__

```javascript
  el.classList.add(className);
  el.classList.remove(className);
  el.classList.toggle(className);
  el.classList.contains(className);
```

### Ширина и Высота

__Ширина окна__
```javascript
  // вместе с полосой прокрутки
  window.document.documentElement.clientHeight;

  // без полосы прокрутки, ведет себя как jQuery
  window.innerHeight;
```

__Высота окна__

```javascript
const body = document.body;
const html = document.documentElement;
const height = Math.max(
  body.offsetHeight,
  body.scrollHeight,
  html.clientHeight,
  html.offsetHeight,å
  html.scrollHeight
);
```

__Высота элемента__

```javascript
  function getHeight(el) {
    const styles = window.getComputedStyle(el);
    const height = el.offsetHeight;
    const borderTopWidth = parseFloat(styles.borderTopWidth);
    const borderBottomWidth = parseFloat(styles.borderBottomWidth);
    const paddingTop = parseFloat(styles.paddingTop);
    const paddingBottom = parseFloat(styles.paddingBottom);
    return height - borderBottomWidth - borderTopWidth - paddingTop - paddingBottom;
  }

  // С точностью до целого числа（когда `border-box`, это `height - border`; когда `content-box`, это `height + padding`）
  el.clientHeight;

  // С точностью до десятых（когда `border-box`, это `height`; когда `content-box`, это `height + padding + border`）
  el.getBoundingClientRect().height;
```

### Позиция и смещение

__Получить текущие координаты элемента относительно смещения его родителя__

```javascript
  { left: el.offsetLeft, top: el.offsetTop }
```

__Получить текущие координаты элемента относительно документа__

```javascript
  function getOffset (el) {
  const box = el.getBoundingClientRect();
    return {
      top: box.top + window.pageYOffset - document.documentElement.clientTop,
      left: box.left + window.pageXOffset - document.documentElement.clientLeft
    };
  }
```

__Позициия скролла__

```javascript
  (document.documentElement && document.documentElement.scrollTop) || document.body.scrollTop;
```