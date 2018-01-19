# The Clojure Style Guide

## Примечание переводчика

Это перевод [оригинального руководства](https://github.com/bbatsov/clojure-style-guide),
составленного Божидаром Бацовым ([Bozhidar Batsov](https://github.com/bbatsov)).
У меня возникли некоторые сложности с переводом некоторых терминов, поэтому я
сразу привожу их список:

* forms - формы (например, special forms - специальные формы)
* body - тело (как в выражении "тело функции").
* binding - связывание, связка (например, local binding - локальное связывание,
  two-way binding - двустороннее связывание, `let`-bindings - связки `let`)
* keywords - ключи (`:whatever`).
* hashmap, map - хеш (`{:key value}`)
* docstring - док. строка
* argument vector - параметры, список параметров (например, в определении
  `(defn f [x y] ...)` argument vector - это `[x y]`). **Важно!** в контексте
  определения функции я использую термин *параметр*, а в контексте вызова -
  *аргумент*.
* multi-arity - многоарность, многоарная (функция)
* top-level - верхний уровень, верхнеуровневый
* forward reference - раннее определение
* transient - изменяемые. см. [документацию](https://clojure.org/reference/transients)
* будет дополняться

Также я иногда буду оставлять в скобках оригинал выражения/термина.

## Введение

> Role models are important. <br/>
> -- Officer Alex J. Murphy / RoboCop

Данное руководство предлагает наилучшие рекомендации по оформлению Clojure кода
для того, чтобы Clojure-программисты могли писать код, который смогут
поддерживать другие Clojure-программисты. Руководство по оформлению кода,
отражающее практики, используемые в реальном мире, будет использоваться людьми,
а руководство, придерживающееся отвергнутых людьми, которым он должен помочь,
рискует не быть используемым ими вовсе (не важно, насколько он при этом хорош).

Руководство поделено на несколько разделов, объединяющих связанные между собой
правила. Я постарался добавить пояснения к правилам (если же они опущены,
значит, я предположил, что они и так очевидны и в пояснениях не нуждаются).

Я не взял все эти идеи с потолка. Они основаны по большей части на моем личном
обширном опыте разработки, комментариях и предложениях членов Clojure сообщества
и некоторых авторитетных источников информации по Clojure, например,
["Clojure Programming"](http://www.clojurebook.com/) и
["The Joy of Clojure"](http://joyofclojure.com/).

Руководство все еще разрабатывается: некоторые разделы отсутствуют, некоторые
все еще не завершены, каким-то правилам не хватает хороших
примеров. Когда-нибудь эти недостатки будут устранены, а пока просто держите
их в уме.

Обратите внимание, что разработчики Clojure также поддерживают список
[стандартов по разработке библиотек](http://dev.clojure.org/display/community/Library+Coding+Standards)

Вы можете сгенерировать PDF или HTML копию данного руководства с помощью
[Pandoc](http://pandoc.org/).

Переводы доступны на следующих языках:

* [Оригинал на английском](https://github.com/bbatsov/clojure-style-guide)
* [Китайский](https://github.com/geekerzp/clojure-style-guide/blob/master/README-zhCN.md)
* [Японский](https://github.com/totakke/clojure-style-guide/blob/ja/README.md)
* [Корейский](https://github.com/kwakbab/clojure-style-guide/blob/master/README-koKO.md)
* [Португальский](https://github.com/theSkilled/clojure-style-guide/blob/pt-BR/README.md)
  (дорабатывается).
* Русский - вы здесь.

## Содержание

* [Организация кода](#source-code-layout--organization)
* [Синтаксис](#syntax)
* [Именование (переменных, функций и т.д.)](#naming)
* [Коллекции](#collections)
* [Изменение состояния](#mutation) (ориг. Mutation)
* [Строки](#strings)
* [Исключения](#exceptions)
* [Макросы](#macros)
* [Комментарии](#comments)
    * [Аннотации](#comment-annotations) (ориг. Comment Annotations)
* [Документирование](#documentation)
* [Общие рекомендации](#existential) (ориг. Existential)
* [Инструменты](#tooling)
* [Тестирование](#testing)
* [Создание библиотек](#library-organization)

<a name="source-code-layout--organization"></a>
## Организация кода

> Почти каждый уверен, что все стили кода, кроме его собственного, - уродливые и
> нечитаемые. Если убрать "кроме его собственного", то, возможно, они
> правы... <br/>
> -- Jerry Coffin (об отступах)

* <a name="spaces"></a>
  Используйте **пробелы** для отступов. Никаких табов.
<sup>[[link](#spaces)]</sup>

* <a name="body-indentation"></a>
Используйте 2 пробела для тела формы. Формы с телом включают `def`-подобные
конструкции, специальные формы и макросы, вводящие локальное связывание
(например, `loop`, `let`, `when-let`) и многие макросы вроде `when`, `cond`,
`as->`, `cond->`, `case`, `with-*` и т.д.
<sup>[[link](#body-indentation)]</sup>


    ```Clojure
    ;; хорошо
    (when something
      (something-else))

    (with-out-str
      (println "Hello, ")
      (println "world!"))

    ;; плохо - четыре пробела
    (when something
        (something-else))

    ;; плохо - один пробел
    (with-out-str
     (println "Hello, ")
     (println "world!"))
    ```

* <a name="vertically-align-fn-args"></a>
  Выравнивайте по вертикали аргументы функции (макроса), если они расположены на
  нескольких строках.
<sup>[[link](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; хорошо
    (filter even?
            (range 1 10))

    ;; плохо
    (filter even?
      (range 1 10))
    ```

* <a name="one-space-indent"></a>
Используйте отступ в один пробел для аргументов функции (макроса), если на одной
строке с именем функции их (аргументов) нет.
<sup>[[link](#one-space-indent)]</sup>

    ```Clojure
    ;; хорошо
    (filter
     even?
     (range 1 10))

    (or
     ala
     bala
     portokala)

    ;; плохо - два пробела
    (filter
      even?
      (range 1 10))

    (or
      ala
      bala
      portokala)
    ```

* <a name="vertically-align-let-and-map"></a>
  Выравнивайте по вертикали `let`-связки (переменные) и ключи для хешей
<sup>[[link](#vertically-align-let-and-map)]</sup>

    ```Clojure
    ;; хорошо
    (let [thing1 "some stuff"
          thing2 "other stuff"]
      {:thing1 thing1
       :thing2 thing2})

    ;; плохо
    (let [thing1 "some stuff"
      thing2 "other stuff"]
      {:thing1 thing1
      :thing2 thing2})
    ```

* <a name="optional-new-line-after-fn-name"></a>
  Можете не использовать перенос строки между именем функции и ее параметрами
  в `defn`, если отсутствует док. строка.
<sup>[[link](#optional-new-line-after-fn-name)]</sup>

    ```Clojure
    ;; хорошо
    (defn foo
      [x]
      (bar x))

    ;; хорошо
    (defn foo [x]
      (bar x))

    ;; плохо
    (defn foo
      [x] (bar x))
    ```

* <a name="multimethod-dispatch-val-placement"></a>
  Размещайте `dispatch-val`
  (см. [`defmethod`](https://clojure.github.io/clojure/clojure.core-api.html#clojure.core/defmethod))
  на той же строке, что и имя функции.
<sup>[[link](#multimethod-dispatch-val-placement)]</sup>


    ```Clojure
    ;; хорошо
    (defmethod foo :bar [x] (baz x))

    (defmethod foo :bar
      [x]
      (baz x))

    ;; плохо
    (defmethod foo
      :bar
      [x]
      (baz x))

    (defmethod foo
      :bar [x]
      (baz x))
    ```

* <a name="oneline-short-fn"></a>
  Можете опустить перенос строки между списком параметров и коротким телом функции
<sup>[[link](#oneline-short-fn)]</sup>

    ```Clojure
    ;; хорошо
    (defn foo [x]
      (bar x))

    ;; хорошо для короткого тела функции
    (defn foo [x] (bar x))

    ;; хорошо для многоарных функций
    (defn foo
      ([x] (bar x))
      ([x y]
       (if (predicate? x)
         (bar x)
         (baz x))))

    ;; плохо
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* <a name="multiple-arity-indentation"></a>
  Для каждой арности функции расставляйте отступы в соответствии с параметрами.
  <sup>[[link](#multiple-arity-indentation)]</sup>

    ```Clojure
    ;; хорошо
    (defn foo
      "I have two arities."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; плохо - слишком большой отступ
    (defn foo
      "I have two arities."
      ([x]
        (foo x 1))
      ([x y]
        (+ x y)))
    ```

* <a name="multiple-arity-order"></a>
  Упорядочивайте определения арностей функции от наименьшего числа параметров к
  наибольшему. Частый случай многоарных функций - когда K параметров полностью
  определяют поведение функции, меньшая арность использует K-арность и большие
  (>K) арности предоставляют возможность передать функции переменное количество
  аргументов (например, с помощью `&`).
  <sup>[[link](#multiple-arity-order)]</sup>

    ```Clojure
    ;; хорошо - легко найти n-ую арность
    (defn foo
      "I have two arities."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; сойдет - остальные арности используют двуарный вариант
    (defn foo
      "I have two arities."
      ([x y]
       (+ x y))
      ([x]
       (foo x 1))
      ([x y z & more]
       (reduce foo (foo x (foo y z)) more)))

    ;; плохо - нет какого-либо осмысленного порядка
    (defn foo
      ([x] 1)
      ([x y z] (foo x (foo y z)))
      ([x y] (+ x y))
      ([w x y z & more] (reduce foo (foo w (foo x (foo y z))) more)))
    ```

* <a name="crlf"></a>
  Используйте Unix окончания строк. (Пользователи *BSD/Solaris/Linux/OSX
  используют их по-умолчанию, а пользователям Windows нужно проявлять
  осторожность).
<sup>[[link](#crlf)]</sup>
    * Если вы используете Git, то можете добавить следующую настройку в
      конфигурацию своего проекта, чтобы уберечь себя от Windows-переносов:

    ```
    bash$ git config --global core.autocrlf true
    ```

* <a name="bracket-spacing"></a>
  Если какой-либо текст предшествует открытой скобке (`(`, `{`, `[`)
  или следует за закрытой скобкой, то отделяйте его от скобки пробелом.
  И наоборот, не добавляйте пробелов между открытой скобки и последующим текстов
  и между текстом и последующей скобкой
  <sup>[[link](#bracket-spacing)]</sup>

    ```Clojure
    ;; хорошо
    (foo (bar baz) quux)

    ;; плохо
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

> Синтаксический сахар вызывает рак точек с запятой (semicolon cancer).
> -- Alan Perlis

* <a name="no-commas-for-seq-literals"></a>
  Не используйте запятые между элементами последовательностей (списки, векторы)
  <sup>[[link](#no-commas-for-seq-literals)]</sup>

    ```Clojure
    ;; хорошо
    [1 2 3]
    (1 2 3)

    ;; плохо
    [1, 2, 3]
    (1, 2, 3)
    ```

* <a name="opt-commas-in-map-literals"></a>
  Подумайте об улучшении читаемости хеш-литералов с помощью использования
  запятых и переносов.
  <sup>[[link](#opt-commas-in-map-literals)]</sup>

    ```Clojure
    ;; хорошо
    {:name "Bruce Wayne" :alter-ego "Batman"}

    ;; хорошо и в некотором роде более читаемо
    {:name "Bruce Wayne"
     :alter-ego "Batman"}

    ;; хорошо и при этом более компактно (чем предыдущий пример)
    {:name "Bruce Wayne", :alter-ego "Batman"}
    ```

* <a name="gather-trailing-parens"></a>
  Оставляйте конечные скобки на одной строке
  <sup>[[link](#gather-trailing-parens)]</sup>

    ```Clojure
    ;; хорошо - на одной строке
    (when something
      (something-else))

    ;; плохо - на разных строках
    (when something
      (something-else)
    )
    ```

* <a name="empty-lines-between-top-level-forms"></a>
  Оставляйте пустые строки между верхнеуровневыми формами
  <sup>[[link](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; хорошо
    (def x ...)

    (defn foo ...)

    ;; плохо
    (def x ...)
    (defn foo ...)
    ```

    Исключение из правил - группировка нескольких связанных `def`

    ```Clojure
    ;; хорошо
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* <a name="no-blank-lines-within-def-forms"></a>
  Не оставляйте пустые строки посреди определения функции или
  макроса. Исключением может быть группировки парных конструкций, например, в
  `let` и `cond`.
  <sup>[[link](#no-blank-lines-within-def-forms)]</sup>

* <a name="80-character-limits"></a>
  Старайтесь избегать написания строк длиннее 80 символов
  <sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  Избегайте лишних пробелов в конце (строк)
  <sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="one-file-per-namespace"></a>
  Для каждого пространства имен создавайте отдельный файл
  <sup>[[link](#one-file-per-namespace)]</sup>

* <a name="comprehensive-ns-declaration"></a>
  Определяйте каждое пространство имен с помощью `ns` с использованием `:refer`,
  `:require` и `:import`. Желательно, в таком порядке.
  <sup>[[link](#comprehensive-ns-declaration)]</sup>

    ```Clojure
    (ns examples.ns
      (:refer-clojure :exclude [next replace remove])
      (:require [clojure.string :as s :refer [blank?]]
                [clojure.set :as set]
                [clojure.java.shell :as sh])
      (:import java.util.Date
               java.text.SimpleDateFormat
               [java.util.concurrent Executors
                                     LinkedBlockingQueue]))
    ```

* <a name="prefer-require-over-use"></a>

  В `ns` предпочитайте `:require :as` вместо `:require :refer` и, тем более,
  `:require :refer :all`. Также предпочитайте `:require` вместо `:use`.
  Последнее должно считаться устаревшим в новом коде.
  <sup>[[link](#prefer-require-over-use)]</sup>

    ```Clojure
    ;; хорошо
    (ns examples.ns
      (:require [clojure.zip :as zip]))

    ;; хорошо
    (ns examples.ns
      (:require [clojure.zip :refer [lefts rights]]))

    ;; приемлемо
    (ns examples.ns
      (:require [clojure.zip :refer :all]))

    ;; плохо
    (ns examples.ns
      (:use clojure.zip))
    ```

* <a name="no-single-segment-namespaces"></a>
  Старайтесь не использовать односоставные пространства имен.
  <sup>[[link](#no-single-segment-namespaces)]</sup>

    ```Clojure
    ;; хорошо
    (ns example.ns)

    ;; плохо
    (ns example)
    ```

* <a name="namespaces-with-5-segments-max"></a>
  Избегайте не использовать слишком длинные названия пространства имен
  (т.е. состоящие более чем из 5 частей).
  <sup>[[link](#namespaces-with-5-segments-max)]</sup>

* <a name="10-loc-per-fn-limit"></a>
  Избегайте функций, состоящих из более 10 строк. В идеале, большинство функций
  должны быть менее 5 строк длиной.
  <sup>[[link](#10-loc-per-fn-limit)]</sup>

* <a name="4-positional-fn-params-limit"></a>
  Избегайте создания функций с более чем тремя-четырьмя позиционными параметрами.
  (прим. переводчика: `[x y & more]` - это всего три параметра, хотя аргументов
  можно передать гораздо больше)
  <sup>[[link](#4-positional-fn-params-limit)]</sup>

* <a name="forward-references"></a>
  Не используйте ранние определения (forward references). Иногда они необходимы,
  то это довольно редкий случай на практике.
  <sup>[[link](#forward-references)]</sup>

<a name="synax"></a>
## Syntax

* <a name="ns-fns-only-in-repl"></a>
  Избегайте использования функций, влияющих на пространство имен, например,
  `require` и `refer`. Они совершенно не нужны, если вы не находитесь в REPL.
  <sup>[[link](#ns-fns-only-in-repl)]</sup>

* <a name="declare"></a>
  Используйте `declare`, когда вам *необходимы* ранние определения.
  <sup>[[link](#declare)]</sup>

* <a name="higher-order-fns"></a>
  Старайтесь использовать функции высшего порядка (такие как `map`) вместо `loop/recur`.
  <sup>[[link](#higher-order-fns)]</sup>

* <a name="pre-post-conditions"></a>
  Старайтесь использовать пред- и пост- условия функции вместо проверок внутри
  тела функции
  <sup>[[link](#pre-post-conditions)]</sup>

    ```Clojure
    ;; хорошо
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; плохо
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException. "x must be a positive number!")))
    ```

* <a name="dont-def-vars-inside-fns"></a>
  Don't define vars inside functions.
  <sup>[[link](#dont-def-vars-inside-fns)]</sup>

    ```Clojure
    ;; очень плохо
    (defn foo []
      (def x 5)
      ...)
    ```

* <a name="dont-shadow-clojure-core"></a>
  Не перекрывайте имена из `clojure.core` локальными именами.
  <sup>[[link](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; плохо - если вам понадобится функция map, вам придется использовать ее
    ;; с помощью clojure.core/map
    (defn foo [map]
      ...)
    ```

* <a name="alter-var"></a>
  Используйте `alter-var-root` вместо `def` для изменения значения переменной.
  <sup>[[link]](#alter-var)</sup>

    ```Clojure
    ;; хорошо
    (def thing 1)
    ; какие-то действия над thing
    (alter-var-root #'thing (constantly nil)) ; значение thing теперь nil

    ;; плохо
    (def thing 1)
    ; какие-то действия над thing
    (def thing nil) ; значение thing теперь nil
    ```

* <a name="nil-punning"></a>
  Используйте `seq`, когда нужно проверить, что последовательность пуста (этот
  прием иногда называется *nil punning*).
  <sup>[[link](#nil-punning)]</sup>

    ```Clojure
    ;; хорошо
    (defn print-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; плохо
    (defn print-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* <a name="to-vector"></a>
  Предпочитайте `vec` вместо `into`, когда вам нужно превратить
  последовательность в вектор.
  <sup>[[link](#to-vector)]</sup>

    ```Clojure
    ;; хорошо
    (vec some-seq)

    ;; плохо
    (into [] some-seq)
    ```

* <a name="when-instead-of-single-branch-if"></a>
  Используйте `when` вместо `(if ... (do ...)`.
  <sup>[[link](#when-instead-of-single-branch-if)]</sup>

    ```Clojure
    ;; хорошо
    (when pred
      (foo)
      (bar))

    ;; плохо
    (if pred
      (do
        (foo)
        (bar)))
    ```

* <a name="if-let"></a>
  Используйте `if-let` вместо `let` + `if`.
  <sup>[[link](#if-let)]</sup>

    ```Clojure
    ;; хорошо
    (if-let [result (foo x)]
      (something-with result)
      (something-else))

    ;; плохо
    (let [result (foo x)]
      (if result
        (something-with result)
        (something-else)))
    ```

* <a name="when-let"></a>
  Используйте `when-let` вместо `let` + `when`.
  <sup>[[link](#when-let)]</sup>

    ```Clojure
    ;; хорошо
    (when-let [result (foo x)]
      (do-something-with result)
      (do-something-more-with result))

    ;; плохо
    (let [result (foo x)]
      (when result
        (do-something-with result)
        (do-something-more-with result)))
    ```

* <a name="if-not"></a>
  Используйте `if-not` вместо `(if (not ...) ...)`.
  <sup>[[link](#if-not)]</sup>

    ```Clojure
    ;; хорошо
    (if-not pred
      (foo))

    ;; плохо
    (if (not pred)
      (foo))
    ```

* <a name="when-not"></a>
  Используйте `when-not` вместо `(when (not ...) ...)`.
  <sup>[[link](#when-not)]</sup>

    ```Clojure
    ;; хорошо
    (when-not pred
      (foo)
      (bar))

    ;; плохо
    (when (not pred)
      (foo)
      (bar))
    ```

* <a name="when-not-instead-of-single-branch-if-not"></a>
  Используйте `when-not` вместо `(if-not ... (do ...)`.
  <sup>[[link](#when-not-instead-of-single-branch-if-not)]</sup>

    ```Clojure
    ;; хорошо
    (when-not pred
      (foo)
      (bar))

    ;; плохо
    (if-not pred
      (do
        (foo)
        (bar)))
    ```

* <a name="not-equal"></a>
  Используйте `not=` Вместо `(not (= ...))`.
  <sup>[[link](#not-equal)]</sup>

    ```Clojure
    ;; хорошо
    (not= foo bar)

    ;; плохо
    (not (= foo bar))
    ```

* <a name="printf"></a>
  Используйте `printf` вместо `(print (format ...))`.
  <sup>[[link](#printf)]</sup>

    ```Clojure
    ;; хорошо
    (printf "Hello, %s!\n" name)

    ;; сойдет
    (println (format "Hello, %s!" name))
    ```

* <a name="multiple-arity-of-gt-and-ls-fns"></a>
  При сравнениях не забывайте, что функции `<`, `>` и т.д. принимают переменное
  количество аргументов.
  <sup>[[link](#multiple-arity-of-gt-and-ls-fns)]</sup>

    ```Clojure
    ;; хорошо
    (< 5 x 10)

    ;; плохо
    (and (> x 5) (< x 10))
    ```

* <a name="single-param-fn-literal"></a>
  Используйте `%` вместо `%1` в литералах функции с одним параметром
  <sup>[[link](#single-param-fn-literal)]</sup>

    ```Clojure
    ;; хорошо
    #(Math/round %)

    ;; плохо
    #(Math/round %1)
    ```

* <a name="multiple-params-fn-literal"></a>
  Используйте `%1` вместо `%` в литералах функций с несколькими параметрами.
  <sup>[[link](#multiple-params-fn-literal)]</sup>

    ```Clojure
    ;; хорошо
    #(Math/pow %1 %2)

    ;; плохо
    #(Math/pow % %2)
    ```

* <a name="no-useless-anonymous-fns"></a>
  Не оборачивайте функции в анонимные функции, если это не требуется.
  <sup>[[link](#no-useless-anonymous-fns)]</sup>

    ```Clojure
    ;; хорошо
    (filter even? (range 1 10))

    ;; плохо
    (filter #(even? %) (range 1 10))
    ```

* <a name="no-multiple-forms-fn-literals"></a>
  Не используйте литералы функций, если тело функции будет состоять из более чем
  одной формы.
  <sup>[[link](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; хорошо
    (fn [x]
      (println x)
      (* x 2))

    ;; плохо (you need an explicit do form)
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a>
  Предпочитайте `complement` вместо использования анонимной функции.
  <sup>[[link](#complement)]</sup>

    ```Clojure
    ;; хорошо
    (filter (complement some-pred?) coll)

    ;; плохо
    (filter #(not (some-pred? %)) coll)
    ```

    Это правило, очевидно, должно быть проигнорированно, если дополнение функции
    уже существует в форме другой функции (например, `even?` и `odd?`).

* <a name="comp"></a>
  Используйте `comp`, когда это упрощает код.
  <sup>[[link](#comp)]</sup>

    ```Clojure
    ;; Предполагается `(:require [clojure.string :as str])`...

    ;; хорошо
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; лучше
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* <a name="partial"></a>
  Используйте `partial`, когда это упрощает код.
  <sup>[[link](#partial)]</sup>

    ```Clojure
    ;; хорошо
    (map #(+ 5 %) (range 1 10))

    ;; (кажется) лучше
    (map (partial + 5) (range 1 10))
    ```

* <a name="threading-macros"></a>
  Используйте threading макрос `->` (thread-first) and `->>` (thread-last) в
  глубоко вложенных формах.
  <sup>[[link](#- `` - hreading-macros)]</sup>

    ```Clojure
    ;; хорошо
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; не так хорошо
    (prn (conj (reverse [1 2 3])
               4))

    ;; хорошо
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; не так хорошо
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```

* <a name="else-keyword-in-cond"></a>
  Используйте `:else`, когда нужно перехватить значение, не проходящее остальные
  условия.
  <sup>[[link](#else-keyword-in-cond)]</sup>

    ```Clojure
    ;; хорошо
    (cond
      (neg? n) "negative"
      (pos? n) "positive"
      :else "zero")

    ;; плохо
    (cond
      (neg? n) "negative"
      (pos? n) "positive"
      true "zero")
    ```

* <a name="condp"></a>
  Предпочитайте `condp` вместо `cond`, когда предикатное выражение не меняется.
  <sup>[[link](#condp)]</sup>

    ```Clojure
    ;; хорошо
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :thirty
      :else :dunno)

    ;; намного лучше
    (condp = x
      10 :ten
      20 :twenty
      30 :thirty
      :dunno)
    ```

* <a name="case"></a>
  Предпочитайте `case` вместо `cond` и `condp`, когда условные выражения -
  константы, известные еще на этапе компиляции.
  <sup>[[link](#case)]</sup>

    ```Clojure
    ;; хорошо
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; лучше
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)

    ;; самый лучший
    (case x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* <a name="shor-forms-in-cond"></a>
  Используйте короткие формы в `cond` и подобных. Если это невозможно, то
  визуально разделите формы на пары с помощью комментариев и пустых строк.
  <sup>[[link](#shor-forms-in-cond)]</sup>

    ```Clojure
    ;; хорошо
    (cond
      (test1) (action1)
      (test2) (action2)
      :else   (default-action))

    ;; наверное, сойдет
    (cond
      ;; test case 1
      (test1)
      (long-function-name-which-requires-a-new-line
        (complicated-sub-form
          (-> 'which-spans multiple-lines)))

      ;; test case 2
      (test2)
      (another-very-long-function-name
        (yet-another-sub-form
          (-> 'which-spans multiple-lines)))

      :else
      (the-fall-through-default-case
        (which-also-spans 'multiple
                          'lines)))
    ```

* <a name="set-as-predicate"></a>
  Используйте `set` как предикат, если возможно.
  <sup>[[link](#set-as-predicate)]</sup>

    ```Clojure
    ;; хорошо
    (remove #{1} [0 1 2 3 4 5])

    ;; плохо
    (remove #(= % 1) [0 1 2 3 4 5])

    ;; хорошо
    (count (filter #{\a \e \i \o \u} "mary had a little lamb"))

    ;; плохо
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "mary had a little lamb"))
    ```

* <a name="inc-and-dec"></a>
  Используйте `(inc x)` и `(dec x)` вместо `(+ x 1)` и `(- x 1)`.
  <sup>[[link](#inc-and-dec)]</sup>

* <a name="pos-and-neg"></a>
  Используйте `(pos? x)`, `(neg? x)` и `(zero? x)` вместо `(> x 0)`,
  `(< x 0)` и `(= x 0)`.
  <sup>[[link](#pos-and-neg)]</sup>

* <a name="list-star-instead-of-nested-cons"></a>
  Используйте `list*` вместо последовательных вызовов `cons`.
  <sup>[[link](#list-star-instead-of-nested-cons)]</sup>

    ```Clojure
    ;; хорошо
    (list* 1 2 3 [4 5])

    ;; плохо
    (cons 1 (cons 2 (cons 3 [4 5])))
    ```

* <a name="sugared-java-interop"></a>
  Используйте синт. сахар для работы с Java.
  <sup>[[link](#sugared-java-interop)]</sup>

    ```Clojure
    ;;; создание объекта
    ;; хорошо
    (java.util.ArrayList. 100)

    ;; плохо
    (new java.util.ArrayList 100)

    ;;; вызов статического метода
    ;; хорошо
    (Math/pow 2 10)

    ;; плохо
    (. Math pow 2 10)

    ;;; вызов метода объекта
    ;; хорошо
    (.substring "hello" 1 3)

    ;; плохо
    (. "hello" substring 1 3)

    ;;; статические поля
    ;; хорошо
    Integer/MAX_VALUE

    ;; плохо
    (. Integer MAX_VALUE)

    ;;; поля объекта
    ;; хорошо
    (.someField some-object)

    ;; плохо
    (. some-object someField)
    ```

* <a name="compact-metadata-notation-for-true-flags"></a>
  Используйте краткий синтаксис для описания метаданных, если они содержат
  только ключи и только значения `true`.
  <sup>[[link](#compact-metadata-notation-for-true-flags)]</sup>

    ```Clojure
    ;; хорошо
    (def ^:private a 5)

    ;; плохо
    (def ^{:private true} a 5)
    ```

* <a name="private"></a>
  Помечайте приватные части кода.
  <sup>[[link](#private)]</sup>

    ```Clojure
    ;; хорошо
    (defn- private-fun [] ...)

    (def ^:private private-var ...)

    ;; плохо
    (defn private-fun [] ...) ; не приватная

    (defn ^:private private-fun [] ...) ; слишком многословно

    (def private-var ...) ; не приватная
    ```

* <a name="access-private-var"></a>
  Для доступа к приватной переменной (например, в рамках тестирования)
  используйте форму `@#'some.ns/var`.
  <sup>[[link](#access-private-var)]</sup>

* <a name="attach-metadata-carefully"></a>
  Будьте внимательны с тем, к чему конкретно вы привязываете метаданные.
  <sup>[[link](#attach-metadata-carefully)]</sup>

    ```Clojure
    ;; мы привязываем метаданные к переменной `a`
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; мы привязываем метаданные к хешу
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

<a name="naming"></a>
## Именование

> Единственными сложностями программирования являются проверка актуальности
> кеша и именование.
> -- Phil Karlton

* <a name="ns-naming-schemas"></a>
  Когда именуете пространство имен, предпочитайте следующие шаблоны:
  <sup>[[link](#ns-naming-schemas)]</sup>
    * `проект.модуль`
    * `компания.проект.модуль`

* <a name="lisp-case-ns"></a>
  Используйте `lisp-case` в отдельных частях пространства имен (например, `bruce.project-euler`)
  <sup>[[link](#lisp-case-ns)]</sup>

* <a name="lisp-case"></a>
  Используйте `lisp-case` для имен функций и переменных.
  <sup>[[link](#lisp-case)]</sup>

    ```Clojure
    ;; хорошо
    (def some-var ...)
    (defn some-fun ...)

    ;; плохо
    (def someVar ...)
    (defn somefun ...)
    (def some_fun ...)
    ```

* <a name="CamelCase-for-protocols-records-structs-and-types"></a>
  Используйте `CamelCase` для протоколов, записей, структур и типов. Сохраняйте
  акронимы вроде HTTP, RFC, XML в верхнем регистре.
  <sup>[[link](#CamelCase-for-protocols-records-structs-and-types)]</sup>

* <a name="pred-with-question-mark"></a>
  Имена предикатов (функций, возвращающих булевое значение) должны оканчиваться
  вопросительным знаком (например, `even?`).
  <sup>[[link](#pred-with-question-mark)]</sup>

    ```Clojure
    ;; хорошо
    (defn palindrome? ...)

    ;; плохо
    (defn palindrome-p ...) ; стиль Common Lisp
    (defn is-palindrome ...) ; стиль Java
    ```

* <a name="changing-state-fns-with-exclamation-mark"></a>
  Имена функций/макросов, которые небезопасны в транзакциях
  [STM](http://en.wikipedia.org/wiki/Software_transactional_memory)
  должны оканчиваться восклицательным знаком (например, `reset!`)
<sup>[[link](#changing-state-fns-with-exclamation-mark)]</sup>

* <a name="arrow-instead-of-to"></a>
  Используйте `->` вместо `to` в названиях приводящих (к типу) функций.
  <sup>[[link](#arrow-instead-of-to)]</sup>

    ```Clojure
    ;; хорошо
    (defn f->c ...)

    ;; не так хорошо
    (defn f-to-c ...)
    ```

* <a name="earmuffs-for-dynamic-vars"></a>
  Используйте `*звездочки*` (в ориг. earmuffs) для того, что планируется
  переопределить.
  <sup>[[link](#earmuffs-for-dynamic-vars)]</sup>

    ```Clojure
    ;; хорошо
    (def ^:dynamic *a* 10)

    ;; плохо
    (def ^:dynamic a 10)
    ```

* <a name="dont-flag-constants"></a>
  Не используйте никакой особенной записи для констант - предполагается, что все
  есть константы, если не указано обратное.
  <sup>[[link](#dont-flag-constants)]</sup>

* <a name="underscore-for-unused-bindings"></a>
  Используйте `_` при именовании параметров (в т.ч. при деструктурировании),
  которые не будут использоваться в коде.
  <sup>[[link](#underscore-for-unused-bindings)]</sup>

    ```Clojure
    ;; хорошо
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; плохо
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hello!"))
    ```

* <a name="idiomatic-names"></a>
  Следуйте примеру `clojure.core` и используйте типичные имена вроде `pred` и
  `coll`.
  <sup>[[link](#idiomatic-names)]</sup>

    * в функциях:
        * `f`, `g`, `h` - для функций
        * `n` - целочисленный параметр (обычно размер)
        * `index`, `i` - целочисленный индекс
        * `x`, `y` - числа
        * `xs` - последовательность
        * `m` - хеш
        * `s` - строка
        * `re` - регулярное выражение
        * `coll` - коллекция
        * `pred` - предикат
        * `& more` - при переменном числе аргументов
        * `xf` - xform, transducer
    * в макросах:
        * `expr` - выражение
        * `body` - тело макроса
        * `binding` - связки макроса (список параметров)

<a name="collections"></a>
## Коллекции

> Лучше иметь 100 функций, работающих с одной структурой данных
> нежели 10 функций, работающих с 10 структурами <br/>
> -- Alan J. Perlis

* <a name="avoid-lists"></a>
  Избегайте использования списков для хранения данных (разве что список - это
  именно то, что вам нужно)
  <sup>[[link](#avoid-lists)]</sup>

* <a name="keywords-for-hash-keys"></a>
  Старайтесь использовать ключевые слова (ключи, keywords) в качестве ключей
  хеша.
  <sup>[[link](#keywords-for-hash-keys)]</sup>

    ```Clojure
    ;; хорошо
    {:name "Bruce" :age 30}

    ;; плохо
    {"name" "Bruce" "age" 30}
    ```

* <a name="literal-col-syntax"></a>
  Старайтесь использовать литералы коллекций, если возможно. Однако для множеств
  используйте литералы только в том случае, когда значениями являются константы,
  известные на этапе компиляции.
  <sup>[[link](#literal-col-syntax)]</sup>

    ```Clojure
    ;; хорошо
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; значения определяются во время выполнения

    ;; плохо
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; во время выполнения выбросит исключение, если (func1) = (func2)
    ```

* <a name="avoid-index-based-coll-access"></a>
  Старайтесь не обращаться к членам коллекций по индексу, если возможно.
  <sup>[[link](#avoid-index-based-coll-access)]</sup>

* <a name="keywords-as-fn-to-get-map-values"></a>
  Prefer the use of keywords as functions for retrieving values from
  maps, where applicable.
  Старайтесь использовать ключи как функции для доступа к значениям хешей, если
  возможно.
  <sup>[[link](#keywords-as-fn-to-get-map-values)]</sup>

    ```Clojure
    (def m {:name "Bruce" :age 30})

    ;; хорошо
    (:name m)

    ;; слишком многословно
    (get m :name)

    ;; плохо - возможна ошибка NullPointerException
    (m :name)
    ```

* <a name="colls-as-fns"></a>
  Используйте факт, что большинство коллекций являются функциями.
  <sup>[[link](#colls-as-fns)]</sup>

    ```Clojure
    ;; хорошо
    (filter #{\a \e \o \i \u} "this is a test")

    ;; плохо - слишком страшно, чтобы показывать
    ```

* <a name="keywords-as-fns"></a>
  Используйте факт, что ключи могут быть использованы как функции над
  коллекцией.
  <sup>[[link](#keywords-as-fns)]</sup>

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* <a name="avoid-transient-colls"></a>
  Не используйте изменяемые (transient) коллекции, если только это
  не критическая часть кода, требующая максимального быстродействия.
  <sup>[[link](#avoid-transient-colls)]</sup>

* <a name="avoid-java-colls"></a>
  Избегайте использования коллекций из Java.
  <sup>[[link](#avoid-java-colls)]</sup>

* <a name="avoid-java-arrays"></a>
  Avoid the use of Java arrays, except for interop scenarios and
  performance-critical code dealing heavily with primitive types.
  Избегайте использования Java массивов. Разве что для случаев интеграции и
  критического кода, много работающего с примитивами.
  <sup>[[link](#avoid-java-arrays)]</sup>

<a name="mutation"></a>
## Изменение состояния

### Транзакционные ссылки (Refs)

* <a name="refs-io-macro"></a>
  Подумайте над оборачиванием всех IO вызовов макросом `io!`, чтобы избежать
  гадких сюрпризов, если вы случайно запустите этот код внутри транзакции
  <sup>[[link](#refs-io-macro)]</sup>

* <a name="refs-avoid-ref-set"></a>
  Избегайте использования `ref-set`, если возможно.
  <sup>[[link](#refs-avoid-ref-set)]</sup>

    ```Clojure
    (def r (ref 0))

    ;; хорошо
    (dosync (alter r + 5))

    ;; плохо
    (dosync (ref-set r 5))
    ```

* <a name="refs-small-transactions"></a>
  Try to keep the size of transactions (the amount of work encapsulated in them)
as small as possible.
<sup>[[link](#refs-small-transactions)]</sup>

* <a name="refs-avoid-short-long-transactions-with-same-ref"></a>
  Избегайте наличия коротких и длительных транзакций, работающих с одной и той
  же ссылкой.
  <sup>[[link](#refs-avoid-short-long-transactions-with-same-ref)]</sup>

### Агенты

* <a name="agents-send"></a>
  Используйте `send` только для действий, привязанных к процессору и не
  блокирующих IO или другие потоки.
  <sup>[[link](#agents-send)]</sup>

* <a name="agents-send-off"></a>
  Используйте `send-off` для действий, которые могут заблокировать, "усыпить"
  (sleep) или как-то иначе подвесить поток.
  <sup>[[link](#agents-send-off)]</sup>

### Атомы

* <a name="atoms-no-update-within-transactions"></a>
  Избегайте обновления атомов внутри STM транзакции.
  <sup>[[link](#atoms-no-update-within-transactions)]</sup>

* <a name="atoms-prefer-swap-over-reset"></a>
  Старайтесь использовать `swap!` вместо `reset!`, где возможно.
  <sup>[[link](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; хорошо
    (swap! a + 5)

    ;; Не так хорошо
    (reset! a 5)
    ```

<a name="strings"></a>
## Строки

* <a name="prefer-clojure-string-over-interop"></a>
  Старайтесь использовать для работы со строками  функции из `clojure.string`
  вместо интеграции с Java или введения собственных.
  <sup>[[link](#prefer-clojure-string-over-interop)]</sup>

    ```Clojure
    ;; хорошо
    (clojure.string/upper-case "bruce")

    ;; плохо
    (.toUpperCase "bruce")
    ```

<a name="exceptions"></a>
## Исключения

* <a name="reuse-existing-exception-types"></a>
  Используйте существующие исключения. Хороший Clojure код выбрасывает
  исключения стандартных типов (если выбрасывает).
  Например, `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`.
  <sup>[[link](#reuse-existing-exception-types)]</sup>

* <a name="prefer-with-open-over-finally"></a>
  Используйте `with-open` вместо `finally`.
  <sup>[[link](#prefer-with-open-over-finally)]</sup>

<a name="macros"></a>
## Макросы

* <a name="dont-write-macro-if-fn-will-do"></a>
  Не пишите макросы, если хороша и функция.
  <sup>[[link](#dont-write-macro-if-fn-will-do)]</sup>

* <a name="write-macro-usage-before-writing-the-macro"></a>
  Перед макросом напишите пример его использования.
  <sup>[[link](#write-macro-usage-before-writing-the-macro)]</sup>

* <a name="break-complicated-macros"></a>
  Разбивайте сложные макросы на меньшие функции.
  <sup>[[link](#break-complicated-macros)]</sup>

* <a name="macros-as-syntactic-sugar"></a>
  Макрос обычно должен быть всего-лишь синтаксическим сахаром над обычной
  функций. В таком случае вам будет легче комбинировать функции.
  <sup>[[link](#macros-as-syntactic-sugar)]</sup>

* <a name="syntax-quoted-forms"></a>
  Старайтесь использовать syntax-quote (\`) вместо того, чтобы создавать список
  "вручную".
  <sup>[[link](#syntax-quoted-forms)]</sup>

<a name="comments"></a>
## Комментарии

> Хороший код является лучшей документацией для самого себя. Если вы собираетесь
> добавить комментарий, то спросите себя: "Как я могу улучшить свой код, чтобы
> этот комментарий был не нужен?". Улучшите код и задокументируйте его, чтобы он
> стал еще более чистым <br/>
> -- Steve McConnell

* <a name="self-documenting-code"></a>
  Старайтесь писать самодокументируемый код.
  <sup>[[link](#self-documenting-code)]</sup>

* <a name="four-semicolons-for-heading-comments"></a>
  Пишите заглавный комментарий минимум с четырьмя точками с запятой (`;;;;`).
  <sup>[[link](#four-semicolons-for-heading-comments)]</sup>

* <a name="three-semicolons-for-top-level-comments"></a>
  Пишите комментарии верхнего уровня с тремя точками с запятой (`;;;`).
  <sup>[[link](#three-semicolons-for-top-level-comments)]</sup>

* <a name="two-semicolons-for-code-fragment"></a>
  Пишите комментарии к фрагменту кода перед ним и выравнивайте их, используя две
  точки с запятой (`;;`).
  <sup>[[link](#two-semicolons-for-code-fragment)]</sup>

* <a name="one-semicolon-for-margin-comments"></a>
  Пишите боковые комментарии, начиная с одной точки с запятой (`;`).
  <sup>[[link](#one-semicolon-for-margin-comments)]</sup>

* <a name="semicolon-space"></a>
  Всегда оставляйте как минимум один пробел после точек с запятой.
  <sup>[[link](#semicolon-space)]</sup>

    ```Clojure
    ;;;; Frob Grovel

    ;;; This section of code has some important implications:
    ;;;   1. Foo.
    ;;;   2. Bar.
    ;;;   3. Baz.

    (defn fnord [zarquon]
      ;; If zob, then veeblefitz.
      (quux zot
            mumble             ; Zibblefrotz.
            frotz))
    ```

* <a name="english-syntax"></a>
  Комментарии, состоящие из более чем одного слова, начинаются с большой буквы и
  используют пунктуацию. Отделяйте предложения
  [одним пробелом](http://en.wikipedia.org/wiki/Sentence_spacing).
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  Не оставляйте очевидных комментариев
  <sup>[[link](#no-superfluous-comments)]</sup>

    ```Clojure
    ;; плохо
    (inc counter) ; увеличивает counter на единицу
    ```

* <a name="comment-upkeep"></a>
  Комментарии должны быть актуальными. Устаревший комментарий хуже, чем
  отсутствие комментария вообще.
  <sup>[[link](#comment-upkeep)]</sup>

* <a name="dash-underscore-reader-macro"></a>
  Используйте макрос `#_` вместо обычного комментирования, когда вам
  нужно закомментировать форму.
  <sup>[[link](#dash-underscore-reader-macro)]</sup>

    ```Clojure
    ;; хорошо
    (+ foo #_(bar x) delta)

    ;; плохо
    (+ foo
       ;; (bar x)
       delta)
    ```

> Хороший код, как и хорошая шутка, не требует объяснения.  <br/>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  Старайтесь не писать коммментарии, чтобы объяснить плохой код. Измените код,
  чтобы он стал понятным ("Делай или не делай, не надо пытаться." --Йода).
  <sup>[[link](#refactor-dont-comment)]</sup>

<a name="comment-annotations"></a>
### Аннотации (Comment Annotations)

* <a name="annotate-above"></a>
  Аннотации обычно должны размещаться непосредственно над связанным кодом.
  <sup>[[link](#annotate-above)]</sup>

* <a name="annotate-keywords"></a>
  Между ключевым словом аннотации и описанием должны стоять двоеточие и
  пробел. Например: `TODO: add something cool`.
  <sup>[[link](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a>
  Если описание занимает несколько строк, то каждая строка должна быть выровнена
  в соответствии с первой (т.е. после ключевого слова).
  <sup>[[link](#indent-annotations)]</sup>

* <a name="sing-and-date-annotations"></a>
  Помечайте аннотацию своими инициалами и датой, чтобы актуальность было легче
  отследить.
  <sup>[[link](#sing-and-date-annotations)]</sup>

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```

* <a name="rare-eol-annotations"></a>
  В случае, когда проблема настолько очевидна, что ее описание будет
  лишним, аннотации могут быть оставлены в конце проблемной строки в виде
  ключевого слова без пояснения. Такое использование должно быть исключением, но
  не правилом.
  <sup>[[link](#rare-eol-annotations)]</sup>

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* <a name="todo"></a>
  Используйте `TODO`, чтобы отметить недостающие фичи или функциональность,
  которую нужно будет добавить позже.
  <sup>[[link](#todo)]</sup>

* <a name="fixme"></a>
  Используйте `FIXME`, чтобы отметить сломанный код, который нужно исправить.
  <sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  Используйте `OPTIMIZE`, чтобы отметить медленный или неэффективный код,
  который может вызвать проблемы с производительностью.
  <sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  Используйте `HACK`, чтобы отметить "пахнущий" код (костыли,
  прим. переводчика), который использует сомнительные приемы и должен быть
  отрефакторен.
  <sup>[[link](#hack)]</sup>

* <a name="review"></a>
  Используйте `REVIEW`, чтобы отметить то, с чем нужно внимательнее
  ознакомиться, чтобы убедиться, что оно работает так, как нужно. Например:
  `REVIEW: Are we sure this is how the client does X currently?`
  <sup>[[link](#review)]</sup>

* <a name="document-annotations"></a>
  Используйте другие ключевые слова для аннотаций, если они кажутся приемлемыми.
  Но не забудьте добавить их в `README` вашего проекта.
  <sup>[[link](#document-annotations)]</sup>

<a name="documentation"></a>
## Документирование

Док. строки - основной способ документирования Clojure кода. Многие
формы-определения (например, `def`, `defn`, `defmacro`, `ns`) поддерживают
док. строки и использовать их - хорошая идея вне зависимости от того, является
ли описываемое публичным или приватным.

Если форма-определение не поддерживает док. строки, то вы все равно можете
добавить документация с помощью аттрибута метаданных `:doc`.

В этом разделе выделены некоторые общие практики по документированию Clojure кода.

* <a name="prefer-docstrings"></a>
Если возможно, используйте док. строки вместо `:doc`.
<sup>[[link](#prefer-docstrings)]</sup>

```clojure
;; хорошо
(defn foo
  "This function doesn't do much."
  []
  ...)

(ns foo.bar.core
  "That's an awesome library.")

;; плохо
(defn foo
  ^{:doc "This function doesn't do much."}
  []
  ...)

(ns ^{:doc "That's an awesome library.")
  foo.bar.core)
```

* <a name="docstring-summary"></a>
  Пишите док. строки законченными предложениями с большой буквы, разумно
  описывающими сущность, к которой привязаны. Благодаря этому ваши инструменты
  (текстовые редакторы и IDE) смогут отображать краткое описание сущности во
  многих местах.
  <sup>[[link](#docstring-summary)]</sup>

```clojure
;; хорошо
(defn frobnitz
  "This function does a frobnitz.
  It will do gnorwatz to achieve this, but only under certain
  cricumstances."
  []
  ...)

;; плохо
(defn frobnitz
  "This function does a frobnitz. It will do gnorwatz to
  achieve this, but only under certain cricumstances."
  []
  ...)
```

* <a name="document-pos-arguments"></a>
  Документируйте все позиционные параметры и оборачивайте их с помощью обратных
  ковычек (\`). Таким образом, текстовые редакторы и IDE смогут определять их
  (параметры) и предоставлять дополнительную функциональность для них.
  <sup>[[link](#document-pos-arguments)]</sup>

```clojure
;; хорошо
(defn watsitz
  "Watsitz takes a `frob` and converts it to a znoot.
  When the `frob` is negative, the znoot becomes angry."
  [frob]
  ...)

;; плохо
(defn watsitz
  "Watsitz takes a frob and converts it to a znoot.
  When the frob is negative, the znoot becomes angry."
  [frob]
  ...)
```

* <a name="document-references"></a>
  Оборачивайте ссылки на другие сущности (функции, константы) в обратные
  \``ковычки`\`, чтобы ваши инструменты могли определять их.
  <sup>[[link](#document-references)]</sup>

```clojure
;; хорошо
(defn wombat
  "Acts much like `clojure.core/identity` except when it doesn't.
  Takes `x` as an argument and returns that. If it feels like it."
  [x]
  ...)

;; плохо
(defn wombat
  "Acts much like clojure.core/identity except when it doesn't.
  Takes `x` as an argument and returns that. If it feels like it."
  [x]
  ...)
```

* <a name="docstring-grammar"></a>
  Док.строки должны быть грамотно оформленными предложениями на английском
  языке, т.е. каждое предложение должно начинаться с прописной буквы и
  заканчиваться точкой (или другим подходящим знаком препинания). Предложения
  должны отделяться пробелом.
  <sup>[[link](#docstring-grammar)]</sup>

```clojure
;; хорошо
(def foo
  "All sentences should end with a period (or maybe an exclamation mark).
  And the period should be followed by a space, unless it's the last sentence.")

;; плохо
(def foo
  "all sentences should end with a period (or maybe an exclamation mark).
  And the period should be followed by a space, unless it's the last sentence")
```

* <a name="docstring-indentation"></a>
  Оставляйте отступ в два пробела в многострочных док. строках.
  <sup>[[link](#docstring-indentation)]</sup>

```clojure
;; хорошо
(ns my.ns
  "It is actually possible to document a ns.
  It's a nice place to describe the purpose of the namespace and maybe even
  the overall conventions used. Note how _not_ indenting the doc string makes
  it easier for tooling to display it correctly.")

;; плохо
(ns my.ns
  "It is actually possible to document a ns.
It's a nice place to describe the purpose of the namespace and maybe even
the overall conventions used. Note how _not_ indenting the doc string makes
it easier for tooling to display it correctly.")
```

* <a name="docstring-leading-trailing-whitespace"></a>
  Не начинайте и не заканчивайте док. строки пробелом.
  <sup>[[link](#docstring-leading-trailing-whitespace)]</sup>

```clojure
;; хорошо
(def foo
  "I'm so awesome."
  42)

;; плохо
(def silly
  "    It's just silly to start a doc string with spaces.
  Just as silly as it is to end it with a bunch of them.      "
  42)
```

* <a name="docstring-after-fn-name"></a>
  Когда пишете док. строку, размещайте его *после* имени функции, а не
  после списка параметров. Последнее не является нарушением синтаксиса и не
  вызовет никаких ошибок, но включает строку как часть тела функции без привязки
  к документации.
  <sup>[[link](#docstring-after-fn-name)]</sup>

```Clojure
;; хорошо
(defn foo
  "docstring"
  [x]
  (bar x))

;; плохо
(defn foo [x]
  "docstring"
  (bar x))
```

<a name="existential"></a>
## Общие рекомендации

* <a name="be-functional"></a>
  Пишите в функциональном стиле, используя изменяемое состояние тогда, когда это
  необходимо.
  <sup>[[link](#be-functional)]</sup>

* <a name="be-consistent"></a>
  Будьте последовательны. В идеале, будьте последовательны по этому руководству.
  <sup>[[link](#be-consistent)]</sup>

* <a name="common-sense"></a>
  Используйте здравый смысл.
  <sup>[[link](#common-sense)]</sup>

<a name="tooling"></a>
## Инструменты

Есть несколько полезных штук, созданных Clojure сообществом, которые могут
помочь вам в вашем стремлении писать хороший Clojure код.

* [Slamhound](https://github.com/technomancy/slamhound) - инструмент, который
  будет генерировать верные `ns` определения из существующего кода.

* [kibit](https://github.com/jonase/kibit) - статический анализатор кода для
  Clojure, который использует [core.logic](https://github.com/clojure/core.logic)
  для поиска кода, который можно переписать с помощью существующей функции или
  макроса.

<a name="testing"></a>
## Тестирование

 * <a name="test-directory-structure"></a>
   Храните ваши тесты в отдельной директории, обычно `test/yourproject` (в
   контрасте с `src/yourproject`). Ваш инструмент сборки (build tool) должен
   обеспечить доступ к ним, когда это необходимо. Большинство шаблонов работают
   с тестами автоматически.
   <sup>[[link](#test-directory-structure)]</sup>

 * <a name="test-ns-naming"></a>
   Именуйте пространство имен теста как `yourproject.something-test`, файл
   обычно будет выглядеть как `test/yourproject/something_test.clj` (или
   `.cljc`, `cljs`).
   <sup>[[link](#test-ns-naming)]</sup>

 * <a name="test-naming"></a>
   При использовании `clojure.test`, определяйте ваши тесты с помощью `deftest`
   и называйте их `something-test`.
   <sup>[[link](#test-naming)]</sup>

   ```clojure
   ;; хорошо
   (deftest something-test ...)

   ;; плохо
   (deftest something-tests ...)
   (deftest test-something ...)
   (deftest something ...)
   ```

<a name="library-organization"></a>
## Создание библиотек

 * <a name="lib-coordinates"></a>
   Если вы публикуете библиотеки, используемые другими людьми, обязательно
   следуйте
   [руководству Central Repository](http://central.sonatype.org/pages/choosing-your-coordinates.html)
   при выборе `groupId` и `artifactId`. Это позволит предотвратить конфликты
   имен и облегчить широкое ее использование. Хорошим примером является
   [Component](https://github.com/stuartsierra/component).
   <sup>[[link](#lib-coordinates)]</sup>

 * <a name="lib-min-dependencies"></a>
   Избегайте ненужных зависимостей. Например, трехстрочная вспомогательная
   функция, скопированная в проект обычно лучше, чем зависисмость, тянущая за
   собой сотни наименований, которые вы не собираетесь использовать.
   <sup>[[link](#lib-min-dependencies)]</sup>

 * <a name="lib-core-separate-from-tools"></a>
   Предоставляйте основную функциональность и различные интеграции в отдельных
   артефактах. В таком случае, пользователи смогут воспользоваться вашей
   библиотекой, не ограничивая себя вашими инструментальными предпочтениями.
   Например, [Component](https://github.com/stuartsierra/component)
   предоставляет основной функционал, а
   [reloaded](https://github.com/stuartsierra/reloaded) предоставляет интеграцию
   с leiningen.
   <sup>[[link](#lib-core-separate-from-tools)]</sup>

# Помощь проекту

Написанное в этом руководстве не является истинной последней инстанции. Я хочу
работать совместно с каждым, кто заинтересован в хорошем оформлении кода
Clojure, чтобы мы смогли создать ресурс, полезный для всего Clojure сообщества.

Не стесняйтесь открывать тикеты и создавать pull-реквесты с улучшениями. Заранее
спасибо за помощь!

Также вы можете помочь руководству финансово с помощью
[gittip](https://www.gittip.com/bbatsov).

[![Support via Gittip](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/bbatsov)

Примечание переводчика:

Данный репозиторий содержит лишь перевод
[оригинального руководства](https://github.com/bbatsov/clojure-style-guide).
Соответственно, все вопросы и предложения, не касающиеся конкретно перевода,
должны направляться в оригинальный репозиторий.

# License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
Данное руководство использует лицензию
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)

# Расскажите всем

Руководство, написанное сообществом, не может оказывать много помощи сообществу,
которое не знает о его существовании. Расскажите о данном гайде друзьям и
коллегам. Каждый комментарий, каждое предложение и мнение, которые мы получаем,
делают это руководство немного лучше. А мы хотим иметь наилучшее возможное
руководство, не правда ли?

Удачи,<br/>
[Божидар](https://twitter.com/bbatsov)
