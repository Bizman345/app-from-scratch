# HTTP методы и HTML

HTTP поддерживает множество методов:
+ get используется только для получения данных, может быть закэкширован
+ post - для создания новой сущности
+ patch - для частичного обновления сущности
+ put - для создания или полной замены сущности, upsert
+ delete - для удаления
+ и т.д.

Однако HTML умеет работать только с get и post.
Get используется при переходе по ссылкам, а post для отправки форм.

Если вы хотите создать кнопку, отправляющую некоторую команду серверу,
то придется реализовать ее в виде html формы, т.к. команда
будет отправлена с помощью post и не будет закэширована.

При проектировании маршрутов не стоит ограничивася только get и post,
используйте семантику протокола.
В этом поможет js.
В экосистеме Ruby on Rails есть проект
[rails-ujs](https://github.com/rails/rails/blob/master/actionview/app/assets/javascripts/README.md).
И он доступен отдельно от rails в виде [npm пакета](https://www.npmjs.com/package/rails-ujs).
Благодаря unpkg.com его очень просто добавить на страницу:

```html
<!-- content -->
    <script src="https://unpkg.com/rails-ujs@5.2.0/lib/assets/compiled/rails-ujs.js"></script>
  </body>
</html>
```

rails-ujs сканирует документ на предмет ссылок с data-атрибутом method:

```html
<a class="nav-item nav-link" data-method="delete" href="/some-url">
  some text
</a>
```

При клике на такую ссылку rails-ujs создает невидимую форму и отпраляет ее post запросом.
Метод устанавливается в поле формы `_method`.
Если вы используете отличные от get и post методы, то приложение должно
отслеживать параметр `_method` и подменять метод в запросе.
