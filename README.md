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

**Moving / Inserting Items**

- Insert child at end of parent

```javascript
parent.appendChild(el);
```

- Inserting a child at the beginning of the parent

```javascript
parent.insertBefore(el, parent.firstChild);
```

- insertAdjacent

The insertAdjacentHTML method allows you to insert arbitrary HTML anywhere in the document, including between nodes!

```javascript
elem.insertAdjacentHTML(where, html);
```

`html` HTML string to insert
`where` - where in relation to elem to insert a line. Only four options:

1. `beforeBegin` -- before `elem`.
2. `afterBegin` -- after `elem`, to the beginning.
3. `beforeEnd` -- inside `elem`, in the end.
4. `afterEnd` -- after `elem`.

**Clone element**

```javascript
el.cloneNode(true);
```

**Delete item**

```javascript
el.remove(); // not supported in IE
el.parentNode.removeChild(el); // IE 8+
```

**Get internal HTML**

```javascript
el.innerHTML;
```

**Get the HTML serialized elem**

```javascript
element.outerHTML;
```

**Get text node**

```javascript
el.textContent;
```

---

### Manipulation of properties and attributes

**Input/Textarea**

```javascript
el.value;
```

Get e.currentTarget index among the same items

```javascript
Array.prototype.indexOf.call(
  document.querySelectorAll("element"),
  e.currentTarget
);
```

**Get attribute value and change it**

```javascript
el.getAttribute("foo");
el.setAttribute("foo", "bar");
```

### CSS and class operation

**Get styles**

```javascript
getComputedStyle(el)[ruleName];
```

**Adding style**

```javascript
el.style.color = "#f01";
```

**Add/Remove/Toggle/Check if contains className**

```javascript
el.classList.add(className);
el.classList.remove(className);
el.classList.toggle(className);
el.classList.contains(className);
```

### Width and Height

**Window width**

```javascript
// together with scroll bar
window.document.documentElement.clientHeight;

// without scrollbar, behaves like jQuery
window.innerHeight;
```

** Window height **

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

** Element height **

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

// Up to an integer （when `border-box` is` height - border`; when `content box` is` height + padding`）
el.clientHeight;

// Accurate to the tenth （when `border-box` is` height`; when `content-box` is` height + padding + border`）
el.getBoundingClientRect().height;
```

### Position and offset

** Get the current coordinates of the element relative to the offset of its parent **

```javascript
  { left: el.offsetLeft, top: el.offsetTop }
```

** Get the current coordinates of the item relative to the document **

```javascript
function getOffset(el) {
  const box = el.getBoundingClientRect();
  return {
    top: box.top + window.pageYOffset - document.documentElement.clientTop,
    left: box.left + window.pageXOffset - document.documentElement.clientLeft
  };
}
```

** Scroll Position **

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

### Utilities

- Trim

```javascript
string.trim();
```

- isArray

```javascript
Array.isArray(array);
```

- Check for the contents of the child element itself

```javascript
el !== child && el.contains(child);
```

- Check for the content of the selector element

```javascript
el.querySelector(selector) !== null;
```

- Filtering

```javascript
Array.prototype.filter.call(document.querySelectorAll(selector), filterFn);
```
