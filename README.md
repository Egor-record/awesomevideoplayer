# Awesome Video Player

Родительский компонент: <b>TimelineGroup</b>.
В нем происходит запрос к серверу, парсится programs.json и разносится информация по компонентам.

В файле <b>TimelineTitles</b> находятся титры к передаче: название и время.

<b>CurrentPlayerPosition</b> - бегунок лайф трансляции.

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

