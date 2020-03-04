---
marp: true
theme: custom
paginate: true
header: Programowanie i architektura aplikacji w chmurze
footer: © 2020 Dariusz Sobczyk
---

# Tworzenie aplikacji w Node.js

---

**Node.js** to otwarto źródłowe i międzyplatformowe środowisko uruchomieniowe dla języka JavaScript.

*https://nodejs.org*

---

![center](images/architektura-nodejs.jpg)

---

**NVM** (ang. Node Version Manager) to otwarto źródłowe oprogramowanie do instalacji i zarządzania wersjami Node.js.

*https://github.com/nvm-sh/nvm*

---

## Przykłady użycia polecenia NVM

```sh
# Zainstaluj Node.js LTS
nvm install --lts
# Zainstaluj Node.js v10.15
nvm install 10.15
# Odinstaluj Node.js LTS
nvm uninstall --lts
# Odinstaluje Node.js v10.15
nvm uninstall 10.15
# Aktywuj Node.js LTS
nvm use --lts
# Aktywuj Node.js v10.15
nvm use 10.15
# Uruchom Node.js LTS
nvm run --lts
# Uruchom Node.js v10.15
nvm run 10.15
```

---

## Sewer HTTP w Node.js

```js
// app.js
const http = require('http')

const host = '127.0.0.1'
const port = process.env.PORT

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('Hello World\n')
})

server.listen(port, host)

// node app.js
```

---

**NPM** (ang. Node Package Manager) to menadżer pakietów dla języka programowania JavaScript. Jest domyślnym menadżerem pakietów dla środowiska Node.js.

*https://npmjs.org*

---

## Przykłady użycia polecenia NPM

```sh
# Utwórz projekt interaktywnie
npm init
# Utwórz projekt z domyślną konfiguracją
npm init -y
# Zainstaluj zależności lokalnie zdefiniowane w package.json
npm install
# Zainstaluj pakiet globalnie
npm install -g <nazwa-pakietu>
# Zainstaluj pakiet lokalnie i dodaj go do package.json
npm install --save <nazwa-pakietu>
# Odinstaluj pakiet zainstalowany globalnie
npm uninstall -g <nazwa-pakietu>
# Odinstaluj pakiet zinstalowany lokalnie
npm uninstall <nazwa-pakietu>
# Uruchom skrypt użytkownika zdefiniowany w package.json
npm run <nazwa-skryptu>
```

---

## package.json

```json
{
  "name": "test",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "node bin/www",
    "dev": "./node_modules/.bin/nodemon bin/www",
    "prd": "pm2 start bin/www",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "koa": "^1.4.0"
    ...
  },
  "devDependencies": {
    ...
  }
}
```

---

## package-lock.json

```json
{
  "name": "test",
  "version": "0.1.0",
  "lockfileVersion": 1,
  "requires": true,
  "dependencies": {
    "koa": {
      "version": "1.7.0",
      "resolved": "https://registry.npmjs.org/koa/-/koa-1.7.0.tgz",
      "integrity": "sha512-bgKsbYjJac0E8O6ya+m6KosXXUigJ15N4XFCnCA0P/kNViu9OnMLv5WcnEeQ5q1SeuKqlqcf0WiroZQBiPHp8Q==",
      "requires": {
        ...
      }
      ...
    }
    ...
  }
  ...
}
```

---

## Pierwszy projekt

```sh
# 1. Zainstaluj Node.js w wersji LTS
nvm install --lts

# 2. Utwórz katalog projektu
mkdir test && cd test

# 3. Zainicjalizuj projekt
npm init -y

# 4. Utórz nowy plik JavaScript
touch app.js && nano app.js

# 5. Uruchom aplikację
node app.js
```

---

## Skrypt uruchamiający w package.json

```json
{
  "scripts": {
    "start": "node app.js"
    ...
  }
  ...
}
```

```sh
# 6. Uruchom aplikację
npm run start
```

---

## Serwer HTTP koa

```js
// app.js
const Koa = require('koa')

const port = process.env.PORT
const app = new Koa()

app.use(async ctx => {
  ctx.body = 'Hello World'
})

app.listen(port)
```

---

## Serwer HTTP koa-router

```js
// app.js
const Koa = require('koa')
const Router = require('koa-router')

const port = process.env.PORT
const app = new Koa()
const router = new Router()

router.get('/', (ctx, next) => {
  ctx.body = 'Hello World'
})

app
  .use(router.routes())
  .listen(port)

// node app.js
```

---

## Generowanie projektu z użyciem koa-generator

```sh
# Utwórz katalog na projekt
mkdir test && cd test

# Zainstaluj pakiet koa-generator globalnie
npm install -g koa-generator

# Wygeneruj pliki projektu w bieżącym katalogu
koa2 .

# Zainstaluj zależności z package.json lokalnie
npm install

# Uruchom aplikację
npm run start
```