# Простой data binding
> Последняя лабораторная работа

Задача этой лабораторной работы &ndash; реализовать механизм, позволяющий делать такое: 
[https://yhaskell.github.io/simple-data-binding/](https://yhaskell.github.io/simple-data-binding/)

## Data binding
Data binding &ndash; механизм, позволяющий "связать" данные модели 
с интерфейсом пользователя.

Этот механизм широко используется при построении графических интерфейсов: 
WPF, Angular, Vue.js, и многие другие библиотеки используют data binding.

Data binding разделяют на два типа: one-way и two-way. 
И тот, и другой вид обеспечивает синхронизацию модели и интерфейса; 
но одностороннее связывание обеспечивает синхронизацию только в одну сторону
(интерфейс по модели, или модель по интерфейсу), в то время как двустороннее 
связывание, очевидно, делает и то, и другое.

## DOM и получение доступа к нему
*D*ocument *O*bject *M*odel (*DOM*) -- это программное представление графического 
интерфейса web-страницы, не зависящее от языка и платформы. 
Любой HTML-документ можно представить в виде дерева узлов, где каждому HTML-тэгу 
соответствует узел в DOM-дереве.

В JavaScript все DOM-элементы являются наследниками класса `HTMLElement`. 
Так, элементу `<div>` соответствует класс `HTMLDivElement`, элементу `<input>`
соответствует класс `HTMLInputElement`, и так далее.
_Более подробно связь тэгов и классов можно посмотреть, к примеру, 
[здесь](https://developer.mozilla.org/en/docs/Web/API/HTMLElement)_.

### DOM Traversal

**DOM Traversal**, или "навигация по DOM" &ndash; способ поиска конкретных элементов
внутри дерева документа. 

Корневым элементом DOM-дерева является элемент `document`, и у него, как и у других 
элементов DOM-дерева, имеются следующие поля:

* `childNodes` &ndash; набор детей текущего узла. Включает в себя как элементы, так и текст
* `children` &ndash; набор детей текущего узла. Включает в себя только элементы 
  (т.е. не текст, а только дочерние тэги)
* `firstChild` &ndash; первый ребёнок
* `lastChild` &ndash; последний ребёнок
* `localName` &ndash; имя тэга. (_Если узел текстовый, то это значение равно `#text`)
* `parentElement` или `parentNode` &ndash; родитель
* `nextSibling` и `previousSibling` &ndash; следующий и предыдущий родственник. Включает в себя как элементы (тэги), так и текстовые узлы.
  
  В случае `<p><span>test</span> hello</p>` следующим родственником тэга `span` будет текстовый узел `"hello"`
* `nextElementSibling` и `previousElementSibling` &ndash; аналогично предыдущему, но только для элементов (тэгов).

Ручной поиск элементов &ndash; не всегда удобный и быстрый способ найти элемент в дереве. 
Поэтому в реальном мире часто используются следующие функции:

* `document.getElementById(id)` из всего дерева выбирает элемент c соответствующим `id`;
* `el.querySelector(selector)` выбирает из детей элемента `el` первый элемент, удовлетворяющий CSS-селектору, 
  переданному в качестве параметра.
* `element.querySelectorAll(selector)` работает аналогично предыдущей функции, но возвращает массив всех элементов.

CSS-селекторы позволяют из множества элементов более узкое множество.
Подробно про них можно прочитать [здесь](https://www.w3schools.com/cssref/css_selectors.asp).

## Mount &ndash; Data Binding Library (наша)

Наша библиотека преставляет интерфейс в виде класса Mount. 

При инициализации наша библиотека обходит детей элемента, переданного в качестве параметра конструктору
и начинает следить за изменениями в тех элементах, в которых есть аргумент `data-bind`. 

Объекты типа `Mount` имеют следующие публичные методы:
* `subscribe(fn)` принимает параметром функцию с двумя параметрами `key, value`
  и добавляет её во внутренний список подписчисков. При изменении модели все подписчики
  уведомляются об изменениях (`key` и `value` содержат ключ и новое значение);
* `set(key, value)` устанавливает значение в модели по ключу `key`;
* `get(key)` возвращает значение модели по ключу;
* `toObject()` возвращает всю модель как объект 
  (изменения в этом объекте не должны менять модель).


## Задание

Реализовать библиотеку, осуществляющую связывание данных для элементов `input`, `select` и `span` (1-way).

Библиотека должна соотвестствовать описанию в предыдущем пункте.

## Тестирование

Для тестирования возьмите файл [index.html](https://github.com/yhaskell/simple-data-binding/blob/master/index.html) 
из текущего репозитория и замените [mount.min.js]() своей реализацией.

## Дополнительная информация

* Большинство DOM-объектов для элементов `<input>` и `<select>` содержат значение в поле `value`
* DOM-объект для флажка (`<input type="checkbox">`) содержит значение в поле `checked`
* DOM-объект для куска текста (`<span>`) содержит значение в поле `innerText`
* Текстовые поля при изменении выполняют событие `oninput`
* Флажки, и другие выборные элементы при изменении выполняют событие `onchange`
* Для регистрации обработчкиов событий существует функция `element.addEventListener`
