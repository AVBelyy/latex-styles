# itmo-student-thesis.cls

Данный стилевой файл для LaTeX предлагается для использования на кафедре "Компьютерные технологии"
для оформления пояснительных записок к бакалаврским работам и магистерским диссертациям.

В частности, его можно использовать взамен решения с папкой sty, имеющего хождение на указанной кафедре приблизительно с 2006 года.

Преимущества:
* гораздо лучше соответствует ГОСТ Р 7.0.1-2011 (на диссертации), в том числе в части списка литературы;
* существует в виде одного cls-файла, который надо включать через `\documentclass`;
* устроен не так ужасно, как sty-папка (например, текст в нем набирается размером `\normalsize`);
* генерирует титульные страницы и даже аннотации.

Недостатки:
* использует довольно свежие возможности LaTeX (например, библиография делается связкой biblatex/biber), поэтому может не заработать на, скажем, MikTeX версии 2005 года;
* написан за два дня (не считая уже существующего примерно с июля 2014 года гостификатора), поэтому могут быть баги, несовместимости или некрасивости;
* включает довольно много пакетов, что может сковывать руки тем, кто использует альтернативы;
* все еще оформлен не по правилам оформления пакетов для LaTeX --- в частности, сообщения об ошибках не оптимизировались вообще.

## Опции пакета
* `annotation` --- если эта опция присутствует, кроме титульной страницы генерируется еще и аннотация. Если аннотация у Вас уже готова и подписана, а копировать ее содержимое
                   в LaTeX-команды лениво, опцию указывать не нужно, и тогда стилевик не будет жаловаться на отсутствие соответствующих команд.
* `times` --- если эта опция присутствует, использует близкое подобие шрифта Times New Roman для набора текста. Для этого требуется довольно старый и кривой пакет `pscyr`, который не
              устанавливается по умолчанию в большинстве дистрибутивов.

## Миграция с предыдущего стилевого пакета
* скачивается свежая версия стилевика;
* из преамбулы убирается все, что вы сами туда не вносили;
* пишется `\documentclass{itmo-student-thesis}` с желаемыми опциями;
* данные о Вас, Вашей теме и научном руководителе (а также содержимое аннотации, если Вы хотите ее собирать этим стилевиком) 
  вносятся, как указано в примерах, после чего делается `\makebachelortitle` или `\makemastertitle`;
* у `\printbibliography` ставится аргумент, как в примере;
* все остальное остается без изменений, и в этом состоянии все уже должно собираться.

## Возможные проблемы при миграции
* скорее всего "поедут" таблицы. В качестве быстрого исправления   помогает поставить `\small` перед `\begin{tabular}`, а если она все еще
  слишком широкая --- может помочь поставить `\setlength{\tabcolsep}{0.2em}` там же.
* если Вы использовали `\usepackage{algorithmic}`, то у Вас в псевдокоде используются `\STATE`, `\IF`, `\WHILE` и так далее. Чтобы это
  работало с `algorithmicx`, надо заменить все конструкции на `\State`, `\If`, `\While` и так далее. Можно сделать это через пачку `\newcommand`, и,
  возможно, я это когда-нибудь сделаю в самом стилевике.
* Конструкции вида
```
@misc{example,
    url="\url{http://www.example.com/}"
}
```
некорректно работают с `biblatex`+`biber`, потому что они корректно обрабатывают поле `url`, и надо писать:
```
@misc{example,
    url={http://www.example.com/}
}
```
* Скрипты, подобные `latexmk`, предназначенные для автоматизации сборки документов LaTeX, могут перестать работать. В частности, такое случилось
с утилитой [`latex-makefile`](https://github.com/shiblon/latex-makefile), которая не в курсе о существовании `biber`. Для этого случая
известно [исправление](https://code.google.com/p/latex-makefile/issues/detail?id=113).

## Известные баги
* В аннотации название темы не подчеркивается, в отличие от остальных полей. К сожалению, используемый для подчеркивания пакет `ulem`
  запрещает перенос слов (а стандартный `\underline` вообще запрещает переход на новую строку). Это с высокой вероятностью приводит к тому,
  что Ваша тема будет написана в одну строчку и вылезет за границу страницы. Если кто-нибудь подскажет мне, как это делать правильно,
  всем будет счастье.
* Предыдущая проблема может всплыть в первозданном виде, если Вы впишете всю известную информацию о научном руководителе, и она будет слишком
  длинной, чтобы влезть в одну строчку. В качестве решения проблемы можно писать только степень и звание, как и требуется в оригинальном шаблоне аннотации.
* При использовании опции `times` (то есть, пакета `pscyr`) и сборки текста через `latex` (но не `pdflatex`) из текста пропадают кавычки-елочки
  и еще некоторые символы. Это, вероятнее всего, баг пакета `pscyr`. Можно пофиксить это путем применения `sed` к файлу `*.dvi`, но гораздо приятнее
  все-таки использовать `pdflatex`.

## Часто задаваемые вопросы
* *Правда ли, что официальное название университета начинается с маленькой буквы? (федеральное...)* Да, правда, уже примерно три года как.

## Обратная связь
Все вопросы, пожелания, замечания, исправления пишите почтой (которую здесь не указываю) или средствами GitHub.