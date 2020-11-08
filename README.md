# Webpack starter kit &middot; [![Build Status](https://img.shields.io/travis/npm/npm/latest.svg?style=flat-square)](https://travis-ci.org/npm/npm) [![npm](https://img.shields.io/npm/v/npm.svg?style=flat-square)](https://www.npmjs.com/package/npm) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) [![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://github.com/your/your-project/blob/master/LICENSE)

## Developing

### Prerequisites

Для корректной работы SASS-компилятора и других инструментов, необходимо один
раз глобально поставить дополнительные пакеты, выполнив в терминале следующие
команды. Пользователям MacOS ничего делать не нужно.

Пользователям **Windows**, в режиме администратора.
[Как запусттить Powershell](https://youtu.be/p2tFnxcymwk) в режиме
администратора.

```shell
npm install --global --production windows-build-tools
```

Вот как выглядит процесс успешной установки для пользователей **Windows**.

![Установка windows-build-tools](https://user-images.githubusercontent.com/1426799/45007904-bde9f280-afb4-11e8-8a35-c77dffaffa2a.gif)

Пользователям **Linux**.

```shell
sudo apt-get install gcc g++ make
```

### Setting up Dev

Для быстрого старта необходимо склонировать репозиторий.

```shell
git clone https://github.com/luxplanjay/webpack-starter-kit.git
```

Переименовать папку сборки именем вашего проекта.

```shell
mv webpack-starter-kit имя_проекта
```

Затем перейти в папку проекта.

```shell
cd имя_проекта
```

Находясь в папке проекта удалить папку `.git` связанную с репозиторием сборки
выполнив следующую команду.

```shell
npx rimraf .git
```

Установить все зависимости.

```shell
npm install
```

И запустить режим разработки.

```shell
npm start
```

Во вкладке браузера перейти по адресу
[http://localhost:4040](http://localhost:4040).

### Building

Для того чтобы создать оптимизированные файлы для хостинга, необходимо выполнить
следующую команду. В корне проекта появится папка `build` со всеми
оптимизированными ресурсами.

```shell
npm run build
```

### Deploying/Publishing

Сборка может автоматически деплоить билд на GitHub Pages удаленного (remote)
репозитория. Для этого необходимо в файле `package.json` отредактировать поле
`homepage`, заменив имя пользователя и репозитория на свои.

```json
"homepage": "https://имя_пользователя.github.io/имя_репозитория"
```

После чего в терминале выполнить следующую команду.

```shell
npm run deploy
```

Если нет ошибок в коде и свойство `homepage` указано верно, запустится сборка
проекта в продакшен, после чего содержимое папки `build` будет помещено в ветку
`gh-pages` на удаленном (remote) репозитории. Через какое-то время живую
страницу можно будет посмотреть по адресу указанному в отредактированном
свойстве `homepage`.

---

---

Задание 1 Напиши функцию delay(ms), которая возвращает промис, переходящий в
состояние "resolved" через ms миллисекунд. Значением исполнившегося промиса
должно быть то кол-во миллисекунд которое передали во время вызова функции
delay.

const delay = ms => { // Твой код };

const logger = time => console.log(`Resolved after ${time}ms`);

// Вызовы функции для проверки delay(2000).then(logger); // Resolved after
2000ms delay(1000).then(logger); // Resolved after 1000ms
delay(1500).then(logger); // Resolved after 1500ms

Задание 2 Перепиши функцию toggleUserState() так, чтобы она не использовала
callback-функцию callback, а принимала всего два параметра allUsers и userName и
возвращала промис.

const users = [ { name: 'Mango', active: true }, { name: 'Poly', active: false
}, { name: 'Ajax', active: true }, { name: 'Lux', active: false }, ];

const toggleUserState = (allUsers, userName, callback) => { const updatedUsers =
allUsers.map(user => user.name === userName ? { ...user, active: !user.active }
: user, );

callback(updatedUsers); };

const logger = updatedUsers => console.table(updatedUsers);

/\*

- Сейчас работает так \*/ toggleUserState(users, 'Mango', logger);
  toggleUserState(users, 'Lux', logger);

/\*

- Должно работать так \*/ toggleUserState(users, 'Mango').then(logger);
  toggleUserState(users, 'Lux').then(logger); Задание 3 Перепиши функцию
  makeTransaction() так, чтобы она не использовала callback-функции onSuccess и
  onError, а принимала всего один параметр transaction и возвращала промис.

const randomIntegerFromInterval = (min, max) => { return
Math.floor(Math.random() \* (max - min + 1) + min); };

const makeTransaction = (transaction, onSuccess, onError) => { const delay =
randomIntegerFromInterval(200, 500);

setTimeout(() => { const canProcess = Math.random() > 0.3;

    if (canProcess) {
      onSuccess(transaction.id, delay);
    } else {
      onError(transaction.id);
    }

}, delay); };

const logSuccess = (id, time) => {
console.log(`Transaction ${id} processed in ${time}ms`); };

const logError = id => {
console.warn(`Error processing transaction ${id}. Please try again later.`); };

/\*

- Работает так _/ makeTransaction({ id: 70, amount: 150 }, logSuccess,
  logError); makeTransaction({ id: 71, amount: 230 }, logSuccess, logError);
  makeTransaction({ id: 72, amount: 75 }, logSuccess, logError);
  makeTransaction({ id: 73, amount: 100 }, logSuccess, logError); /_
- Должно работать так \*/ makeTransaction({ id: 70, amount: 150 })
  .then(logSuccess) .catch(logError);

makeTransaction({ id: 71, amount: 230 }) .then(logSuccess) .catch(logError);

makeTransaction({ id: 72, amount: 75 }) .then(logSuccess) .catch(logError);

makeTransaction({ id: 73, amount: 100 }) .then(logSuccess) .catch(logError);
