_Скрипт оболочки_ - это набор команд, функций, переменных или почти все, что можно задействовать из оболочки  
  
Скрипт может быть запущен одним из двух способов:  
* Имя файла используетс в оболочке в качестве аргумента (как в _bash myscript_)  
* В первой строке скрипта оболочки может стоять имя интерпретатора, перед которым указаны символы _#!_ (как в #!/bin/bash) и флаг исполнения файла с набором скриптов. Затем можно запустить свой скрипт так же, как и любую другую программу в пути, просто введя его имя в командной строке  
  
При выполнении скриптов любым способом параметры программы можно указать в командной строке. Все, что следует за именем скрипта, называется _аргументом командной строки_.  
  
Знак фунта _(#)_ указывается перед комментарием и может занимать целую строку или стоять в строке после кода скрипта.  
  
Имена переменных в скриптах оболочки **чувствительны к регистру** и могут быть определены следующим образом: _ИМЯ=значение_. Значениями переменных могут быть константы, такие как текст, цифры и символы подчеркивания  
  
Переменные могут содержать выходные данные команды или последовательности команд. Для этого нужно поставить перед командой знак доллара и открыть скобку, а после нее набрать закрывающую скобку. В этом случае команда выполняется при введении переменной, а не при каждом ее чтении.  
  
Знак доллара, обратный тик, звездочка, восклицательный знак и др. имеют особые значения в оболочке. Например, для набора текста _$HOME_ _(/$HOME\)_, этот текст следует экранировать, чтобы интерпретатор не воспринимал его как получение значения переменной окружения. С двойными кавычками все немного сложнее. Например, если текст окружен двойными кавычками, знаки доллара, обратные тики и восклицательные знаки интерпретируются по особенному, а другие символы - как обычно.  
  
Существуют специальные переменные, которые назначает оболочка. Набор часто используемых символов называется _позиционными параметрами аргументов командной строки_. Они указываются в виде значений $0, $1.. . $0 - особенный, ему присваивается имя, используемое для вызова скрипта (полный или относительный путь), остальным присваиваются значения параметров, передаваемых в командной строке в том порядке, в котором они появились. $# показывает сколько всего параметров было задано в скрипте. $@ содержит все аргументы, введенные в командной строке. $? получает статус выхода последней выполенной команды (0 - успешно, остальное как правило - ошибка)  
  
С помощью команды ___read___ можно запросить у пользователя информацию и сохранить ее для последующего использования в скрипте. В описание команды входит название переменных, которые затем хранят введенные значения.  
  
Все приведенные ранее расширения переменных являются сокращением от ___${..name..}___. Этот вариант используется, когда значение переменной не должно быть отделено от другого текста пробелом. У оболочки _bash_ есть специальные правила, которые позволяют расширять переменные различными способами. Например, $(var:-значение) устанавливает значение _значение_ переменной, если та пуста или не задана.  
  
Bash использует _нетипизированные_ переменные. Арифметика с целыми числами выполняется с помощью встроенной команды _let_, или внешних команд _expr_, или _bc_ - приложение калькулятора.  
  
Другой способ увеличения значения переменной - использовать _$(())_ с добавлением _++_ для увеличения значения _I_: _$((++I))_. Постфиксный инкремент так же работает как и в С++  
  
**Примечание**  
Большинство элементов в скриптах оболочки имеют относительно свободную форму (где пробелы или отступы не важны), тогда как для команд _let_ и _expr_ интервалы особенно важны. Первая требует, чтобы не было пробелов между каждым аргументов операции (операндом) и математическим оператором, в то время как синтаксис второй команды требует указание пробелов между каждый операндом и его оператором.  
  
Синтактис условного оператора _if..then_:  
if [_expression_ _flag or (=/!=)_ **value**] ; then
  operators.. (_elif/else_)
fi  

_help test_ для перечисления условий, которые можно использовать в условном выражении  
Обратите внимание, что строки нужно помещать в двойные кавычки!!  
  
Существует также сокращенный метод проверки, полезный для простых _однокомандных действий_  
_Комментарий:_ выполнить однокомандное действие, если тест неверен  
_[ test ]_ || _action_  
Вместо знаков конвейера можно использовать два амперсанда _&&_. Можно комбинировать логические операторы _&& ||_ чтобы ускорить проверку с помощью if..then..else  
  
Часто используется и команда _case_. Подобно оператору _switch_ в языках программирования, он может заменить несколько операторов _if_.  
Общая форма выражения _case_:  
>case "VAR" in
>  Result1)
>    { body };;
>  Result2);;
>    { body };;
>  *)
>    { body } ;;
>esac  
  
Звездочка используется в качестве ключевого слова, аналогичного слову _default_ в языке программирования С.  
  
Цикл _for..do_ перебирает список значений, выполняя тело цикла для каждого элемента в списке. Синтаксис цикла выглядит так:  
>for _VAR_ in _LIST_  
>do  
>  { body }  
>done  
  
Цикл присваивает значения, указанные в _LIST_ для _VAR_, по одному за раз. Затем для каждого значения выполняется _body_. _VAR_ может быть именем переменной, а _LIST_ - состоять практически из любого списка значений или всего, что генерирует список. (_for VAR in LIST ; do_). Каждый элемент в _LIST_ отделен от следующего пробелом. Заядлый программист на языке С может использовать его синтаксис:   
>LIMIT=10  
>  
>for ((a=1; a <= LIMIT ; a++)) ; do  
>  echo "$a"  
>done  
  
Еще два популярных цикла - это _while..do_ и _until..do_. Вот их структуры:  

>while _condition_  
>do  
>  { body }  
>done  
>  
>until _condition_  
>do  
>  { body }  
>done  
  
Цикл while выполняется, пока условие выражения истинно; until выполняется до тех пор, пока условие не станет истинным, другими словами, пока оно ложно.  
  
Одни из самх популярных программ для работы с текстом - это _grep, cut, tr, awk_ и _sed_. Большинство этих программ предназначены для работы со стандартными вводом\выводом и позволяют использовать конвейеры и скрипты.  
  
_grep ^HO_ - поиск текста, который начинается с _HO_  
Команда _cut_ извлекает поля из строки текста или файлов  
Команда _tr_ - это символьный переводчик, который используется для замены одного символа или набора символов другим или для удаления символа из строки текста  
Команда _sed_ - это простой редактор скриптов  
  
_shift_ - сдвигает позиционные параметры влево на 1