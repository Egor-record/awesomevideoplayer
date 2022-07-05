# Awesome Video Player

Родительский компонент: <a href="scr/components/TimelineGroup.vue">TimelineGroup</a>.
В нем происходит запрос к серверу, парсится programs.json и разносится информация по компонентам.

В файле [a TimelineTitles](scr/components/TimelineTitles.vue) находятся титры к передаче: название и время.

<a href="scr/components/CurrentPlayerPosition.vue">CurrentPlayerPosition</a> - бегунок лайф трансляции.

TODO:

<ul>
    <li>Счетчик до начала следующего фильма</li>
    <li>Реализовать перемотку фильма</li>
    <li>Переписать d-flex на float</li>
    <li>Добавить полифил для моделей младше webOS 4</li>

</ul>

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

