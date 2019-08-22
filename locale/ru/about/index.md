---
layout: about.hbs
title: О проекте
trademark: Торговая марка
---

# О Node.js&reg;

Как асинхронное событийное JavaScript-окружение, Node.js спроектирован для построения
масштабируемых сетевых приложений. Ниже приведен пример "hello world", который
может одновременно обрабатывать много соединений. Для каждого соединения вызывается
функция обратного вызова, однако, когда соединений нет Node.js засыпает.

```javascript
const http = require("http");

const hostname = "127.0.0.1";
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader("Content-Type", "text/plain");
  res.end("Hello World\n");
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

Этот подход контрастирует с более распространенной на сегодняшний день моделью параллелизма, в которой
используются параллельные OS потоки. Такой подход является относительно неэффективным и очень сложным
в использовании. Кроме того, пользователи Node.js могут не беспокоиться о блокировках процессов,
поскольку их не существует. Почти ни одна из функций в Node.js не работает напрямую с I/O,
поэтому поток никогда не блокируется. В следствии этого на Node.js легко разрабатывать масштабируемые системы.

Для более детального знакомства с этим подходом, можно ознакомится с полной статьей [Blocking vs Non-Blocking][].

---

Node.js создан под влиянием таких систем как [Event Machine][] в Ruby или
[Twisted][] в Python. Но при этом событийная модель, в нем, используется значительно шире, принимая
[event loop][] за основу окружения, а не в качестве отдельной библиотеки. В других системах всегда происходят
блокировки вызова, чтобы запустить цикл событий.

Обычно, поведение определяется через функции обратного вызова (callback) в начале
скрипта и дальнейшим его вызовом через блокирующий вызов, вроде `EventMachine::run()`.
В Node.js нет ничего похожего на вызов начала цикла событий, он автоматически входит в него после запуска скрипта.
Node.js выходит из событийного цикла тогда, когда не остается зарегистрированных функций обратного вызова.
Такое поведение похоже на поведение браузерного JavaScript, где событийный цикл скрыт от пользователя.

HTTP является объектом первого рода в Node.js, разработанным с поточностью и малой задержкой, что делает Node.js
хорошей основой для веб-библиотеки или фреймворка.

То что Node.js спроектирован без многопоточности, не означает, что в нем нет возможности
использовать нескольких ядер. Для работы с ними можно создавать и управлять дочерними процессами,
с помощью API [`Child_process.fork()`][]. Модуль [`cluster`][] построен на этом интерфейсе и позволяет делиться сокетами
между процессами и распределять нагрузку между ядрами.

[blocking vs non-blocking]: /ru/docs/guides/blocking-vs-non-blocking/
[`child_process.fork()`]: /api/child_process.html#child_process_child_process_fork_modulepath_args_options
[`cluster`]: /api/cluster.html
[event loop]: /ru/docs/guides/event-loop-timers-and-nexttick/
[event machine]: https://github.com/eventmachine/eventmachine
[twisted]: http://twistedmatrix.com/