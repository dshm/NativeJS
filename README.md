# Native JavaScript

### Query Selector

**Search by selector (`.class, #id, a[target=_blank] ...`)**

```javascript
document.querySelectorAll("selector"); // returns an array of elements
document.querySelector("selector"); // returns the first found element
```

**Find by className (`.class`)**

```javascript
document.getElementsByClassName(".class"); // returns an array of elements
```

**Find by ID (`#class`)**

```javascript
document.getElementById("id"); // returns the first found item
```

**Find among descendants**

```javascript
el.querySelectorAll("element");
```

**Related / Previous / Next items**

- Related items

```javascript
Array.prototype.filter.call(el.parentNode.children, function(child) {
  return child !== el;
});
```

- Previous item

```javascript
el.previousElementSibling;
```

- Next item

```javascript
el.nextElementSibling;
```

- Parent item

```javascript
el.parentNode;
```

**Closest**

Returns the first matched element on the provided selector, bypassing from the current elements to the document up.

```javascript
el.closest(selector)(
  // IE и Edge
  function() {
    // check support
    if (!Element.prototype.closest) {
      Element.prototype.closest = function(css) {
        var node = this;

        while (node) {
          if (node.matches(css)) return node;
          else node = node.parentElement;
        }
        return null;
      };
    }
  }
)();
```

**Parents to**

Get the parents of each element in the current set of matched elements, but not including the element that matched the selector, the DOM node.

```javascript
function parentsUntil(el, selector, filter) {
  const result = [];
  const matchesSelector =
    el.matches ||
    el.webkitMatchesSelector ||
    el.mozMatchesSelector ||
    el.msMatchesSelector;

  // Match starting from parent
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

### DOM node manipulation

**Перемещение/вставка элементов**

- Вставка дочернего элемента в конец родителя

```javascript
parent.appendChild(el);
```

- Вставка дочернего элемента в начало родителя

```javascript
parent.insertBefore(el, parent.firstChild);
```

- insertAdjacent

Метод insertAdjacentHTML позволяет вставлять произвольный HTML в любое место документа, в том числе и между узлами!

```javascript
elem.insertAdjacentHTML(where, html);
```

html cтрока HTML, которую нужно вставить
where - Куда по отношению к elem вставлять строку. Всего четыре варианта:

1. `beforeBegin` -- перед `elem`.
2. `afterBegin` -- внутрь `elem`, в самое начало.
3. `beforeEnd` -- внутрь `elem`, в конец.
4. `afterEnd` -- после `elem`.

**Склонировать элемент**

```javascript
el.cloneNode(true);
```

**Удалить элемент**

```javascript
el.remove(); // не поддерживаеться в IE
el.parentNode.removeChild(el); // IE 8+
```

**Получить внутренний HTML**

```javascript
el.innerHTML;
```

**Получить серриализированный elem HTML**

```javascript
element.outerHTML;
```

**Получить текстовый узел**

```javascript
el.textContent;
```

---

### Манипуляция с свойствами и атрибутами

**Input/Textarea**

```javascript
el.value;
```

\_\_Получить индекс e.currentTarget среди тех же елементов

```javascript
Array.prototype.indexOf.call(
  document.querySelectorAll("element"),
  e.currentTarget
);
```

**Получить значение атрибута и изменение его**

```javascript
el.getAttribute("foo");
el.setAttribute("foo", "bar");
```

### СSS и операция с классами

**Получить стили**

```javascript
getComputedStyle(el)[ruleName];
```

**Присвоение style**

```javascript
el.style.color = "#f01";
```

**Добавить/Удалить/Переключить/Проверить на сушествование class**

```javascript
el.classList.add(className);
el.classList.remove(className);
el.classList.toggle(className);
el.classList.contains(className);
```

### Ширина и Высота

**Ширина окна**

```javascript
// вместе с полосой прокрутки
window.document.documentElement.clientHeight;

// без полосы прокрутки, ведет себя как jQuery
window.innerHeight;
```

**Высота окна**

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

**Высота элемента**

```javascript
function getHeight(el) {
  const styles = window.getComputedStyle(el);
  const height = el.offsetHeight;
  const borderTopWidth = parseFloat(styles.borderTopWidth);
  const borderBottomWidth = parseFloat(styles.borderBottomWidth);
  const paddingTop = parseFloat(styles.paddingTop);
  const paddingBottom = parseFloat(styles.paddingBottom);
  return (
    height - borderBottomWidth - borderTopWidth - paddingTop - paddingBottom
  );
}

// С точностью до целого числа（когда `border-box`, это `height - border`; когда `content-box`, это `height + padding`）
el.clientHeight;

// С точностью до десятых（когда `border-box`, это `height`; когда `content-box`, это `height + padding + border`）
el.getBoundingClientRect().height;
```

### Позиция и смещение

**Получить текущие координаты элемента относительно смещения его родителя**

```javascript
  { left: el.offsetLeft, top: el.offsetTop }
```

**Получить текущие координаты элемента относительно документа**

```javascript
function getOffset(el) {
  const box = el.getBoundingClientRect();
  return {
    top: box.top + window.pageYOffset - document.documentElement.clientTop,
    left: box.left + window.pageXOffset - document.documentElement.clientLeft
  };
}
```

**Позициия скролла**

```javascript
(document.documentElement && document.documentElement.scrollTop) ||
  document.body.scrollTop;
```

### Events

- On

```javascript
el.addEventListener(eventName, eventHandler);
```

- Off

```javascript
el.removeEventListener(eventName, eventHandler);
```

- Document ready

```javascript
document.addEventListener("DOMContentLoaded", fn);
```

- Trigger Custom Event

```javascript
const event = new CustomEvent("my-event", { detail: { some: "data" } });
el.dispatchEvent(event);
```

- Trigger Native Event

```javascript
const event = document.createEvent("HTMLEvents");
event.initEvent("change", true, false);
el.dispatchEvent(event);
```

### Утилиты

- Trim

```javascript
string.trim();
```

- isArray

```javascript
Array.isArray(array);
```

- Проверка на содержание непосредственно child элемента

```javascript
el !== child && el.contains(child);
```

- Проверка на содержание selector элемента

```javascript
el.querySelector(selector) !== null;
```

- Фильтрация

```javascript
Array.prototype.filter.call(document.querySelectorAll(selector), filterFn);
```
