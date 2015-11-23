Как обмениваться данными со статистиками 
===========

Это руководство для каждого, кому необходимо обмениваться данными со статистиками. 
Целевую аудиторию данного документа я предполагаю следующую:

* Научные сотрудники, которым нужны статистики для анализа данных
* Студенты и постодоки в научных дисциплинах, которые ищут квалифицированный совет 
* Студенты, только начинающие работать со статистикой, чей работой является сборка и очистка наборов данных

Цель этого руководства предоставить некоторую инструкцию по лучшему способу обмениваться данными, чтобы избежать наиболее 
распространенных ошибок и различных задержек при переходе от сбора данных к анализу данных. [Leek group](http://biostat.jhsph.edu/~jleek/) 
работает с большим количеством научных сотрудников и одним источником данных для скорости, и в результате получаем измененное состояние данных, когда они поступают в Leek group.
Основываясь на моих беседах с другими статистиками, это верно практически повсеместно.

Мое твердое мнение, что статистики должны обрабатывать данные в каком бы состоянии эти данные не были получены. Важно увидеть сырые данные, понять шаги, 
которые были выполнены при обработке и иметь возможность включить скрытые источники данных в этот анализ данных.
С другой стороны, для многих типов данных шаги обработки хорошо задокументированы и стандартизированы.Таким образом работа по переводу данных из 
сырой формы в непосредственно в анализируемую форму может быть выполнена до вызова статистика. Это может значительно ускроить сроки выполнения работ, поскольку 
статистик не должен участвовать в предварительной обработке данных.


Что вы должны донести до статистика
====================

Для максимальной скорости анализа данных, вы должны предоставить статиску следующую информацию:

1. Сырые данные.
2. Набор обработанных данных [Описание обработанных данных](http://vita.had.co.nz/papers/tidy-data.pdf) 
3. Справочник, описывающий каждую переменную и ее значения в наборе обработанных данных
4. Наличие четкой инструкции, рассказывающей о том как вы перешли от 1 -> 2,3 

Давайте рассмотрим каждую часть пакета данных, которую вы будете передавать.


### Сырые данные

Очень важно, что вы включаете самые сырые/необработанные данные, к которым у вас есть доступ. Это некоторые примеры сырой формы данных:

* Ваши измерения в виде [бинарного файла](http://en.wikipedia.org/wiki/Binary_file), который выдает машина
* Неотформатированный файл Excel с 10 вкладками-листами, который вам направила компания с которой у вас есть договор
* Сложные данные в формате [JSON](http://en.wikipedia.org/wiki/JSON)Ю которые вы получили через [Twitter API](https://twitter.com/twitterapi)
* Введенные вручную цифры, которые вы собрали наблюдая через микроскоп.

Вы знаете, что сырые данные находяться в правильном формате, если вы:

1. Не запускали программного обеспечения на этих данных
1. Не манипулировали любыми числами в этих данных
1. Не удаляли какие-либо данные из рассматриваемого набора данных
1. Вы не обобщали данные каким-либо способом

Если вы манипулировали данными в наборе, то это уже будут не сырые данные
If you did any manipulation of the data at all it is not the raw form of the data. Манипулирование сырыми данными очень распространный способ 
замедлить процесс анализа, так как аналитик будет часто проводить пристальное изучение данных, чтобы выяснить почему они выглядят странно.

### The Набор обработанных данных

Общие принципы обработанных данных изложены [Hadley Wickham](http://had.co.nz/) в [его работе](http://vita.had.co.nz/papers/tidy-data.pdf)
и [этом видео](http://vimeo.com/33727555). И работа и видео сфокусированы на [R](http://www.r-project.org/) пакете, который вы можете знать, 
а можете не знать как использовать. Несмотря на это, есть четыре принципа, на которые стоит обратить внимание:

1. Каждая переменная, которую вы измеряете, должна быть в отдельной колонке
1. Каждое наблюдение должно быть в отдельной строке
1. Должна быть одна таблица на каждый вид переменной
1. Если у вас несколько таблиц, то они должны содержать дополнительную колонку, которая позволит им быть связанными между собой

Кроме этих жестких правил есть ряд других вещей, которые позволят обрабатывать ваши данные гораздо проще.
Во-первых, необходимо включить строку на верх каждой таблицы с данными/листа, которая будет содержать полные имена колонок.
Так что если вы измерили возраст на момент постановки диагноза у пациентов, вы бы знали, что колонка с именем `AgeAtDiagnosis` вместо чего-то вроде `ADx` 
или другой аббревиатуры, которая может быть трудна для понимания для другого человека. 

Это пример того, как это бы работало в геномике. Предположим, что для 20 человек вы собрали измерения экспрессии генов в [РНК-последовательности]
(http://en.wikipedia.org/wiki/RNA-Seq). Вы также собрали демографические и клинические данные о пациентах, включая их возраст, лечение и диагнозы. 
У вас бы получилась одна таблица/лист, который содержал бы клиническую/демографическую информацию. Она имела бы 4 колонки (ИД пациента, возраст, лечение, диагнозы) 
и 21 строку (строка с именами переменных и одна строка для каждого пациента). У вас также был бы один лист для обобщения данных генома.
Обычно этот тип данных обощается на уровне числа импульсов в экзоне. Предположим, у вас есть 100,000 екзонов, то вы бы имели таблицу/лист, в котором бы 
была 21 строка (строка для имен геномов, и одна строка для каждого пациента) и 100,001 колонки (одна колонка для ids пациента и одна колонка для каждого типа данных)
  
Если вы обмениваетесь данными с коллегой в формате Excel, обработанные данные должны быть в одной таблице в одном Excel-файле. Они
 не должны быть на нескольких листах, к ним не должны быть применены макросы, а колонки/ячейки недолжны быть выделены.
Альтернативный способ обмениваться данными в [CSV](http://en.wikipedia.org/wiki/Comma-separated_values) или 
[TAB-delimited](http://en.wikipedia.org/wiki/Tab-separated_values) файле.


### Справочник

Почти для любого набора данных действия, которые вы выполнили, над этим набором должны быть описаны более подробно, чем у вас
в листе. Справочник содержит данную информацию. Как минум он должен содержать:

1. Инфорация о переменных (включая единицы измерения) в наборах данных, не вошедших в обработанные данные 
1. Информация о выполенных допущениях
1. Информация о структуре экспериментального исследования, которую вы использовали

В нашем геномном примере аналитик может захотеть узнать, какие единицы измерения для каждой 
клинической/демографической переменной использовались (возраст в годах, лечение по наименованию препарата/доза, уровень диагностики).
Также он захочет узнать, как вы отобрали экзонов, которых вы использовали для обобщения данных генома (UCSC/Ensembl, etc.). Он также 
захочет узнать любую другую информацию о том, как вы сделали сбор данных/структуру исследования. Например, эти первые 20 пациентов, 
которые гуляли в клинике? Или это 20 тщательно отобранных пациентов на основании какой-либо характеристики, например, возраст? Они 
рандомизированы для лечения?

Общий формат для такого документа это word файл. В документе должна быть секция "Структура исследования", в котором необходимо 
описать, как были собраны данные. Также в документе должнабыть секция "Справочник", в которой необходимо описать каждую переменную 
и ее единицы измерения. 

### How to code variables

When you put variables into a spreadsheet there are several main categories you will run into depending on their [data type](http://en.wikipedia.org/wiki/Statistical_data_type):

1. Continuous
1. Ordinal
1. Categorical
1. Missing 
1. Censored

Continuous variables are anything measured on a quantitative scale that could be any fractional number. An example
would be something like weight measured in kg. [Ordinal data](http://en.wikipedia.org/wiki/Ordinal_data) are data that have a fixed, small (< 100) number of levels but are ordered. 
This could be for example survey responses where the choices are: poor, fair, good. [Categorical data](http://en.wikipedia.org/wiki/Categorical_variable) are data where there
are multiple categories, but they aren't ordered. One example would be sex: male or female. [Missing data](http://en.wikipedia.org/wiki/Missing_data) are data
that are missing and you don't know the mechanism. You should code missing values as `NA`. [Censored data](http://en.wikipedia.org/wiki/Censoring_(statistics\)) are data
where you know the missingness mechanism on some level. Common examples are a measurement being below a detection limit
or a patient being lost to follow-up. They should also be coded as `NA` when you don't have the data. But you should
also add a new column to your tidy data called, "VariableNameCensored" which should have values of `TRUE` if censored 
and `FALSE` if not. In the code book you should explain why those values are missing. It is absolutely critical to report
to the analyst if there is a reason you know about that some of the data are missing. You should also not [impute](http://en.wikipedia.org/wiki/Imputation_(statistics\))/make up/
throw away missing observations.

In general, try to avoid coding categorical or ordinal variables as numbers. When you enter the value for sex in the tidy
data, it should be "male" or "female". The ordinal values in the data set should be "poor", "fair", and "good" not 1, 2 ,3.
This will avoid potential mixups about which direction effects go and will help identify coding errors. 

Always encode every piece of information about your observations using text. For example, if you are storing data in Excel and use a form of colored text or cell background formatting to indicate information about an observation ("red variable entries were observed in experiment 1.") then this information will not be exported (and will be lost!) when the data is exported as raw text.  Every piece of data should be encoded as actual text that can be exported.  

### The instruction list/script

You may have heard this before, but [reproducibility is kind of a big deal in computational science](http://www.sciencemag.org/content/334/6060/1226).
That means, when you submit your paper, the reviewers and the rest of the world should be able to exactly replicate
the analyses from raw data all the way to final results. If you are trying to be efficient, you will likely perform
some summarization/data analysis steps before the data can be considered tidy. 

The ideal thing for you to do when performing summarization is to create a computer script (in `R`, `Python`, or something else) 
that takes the raw data as input and produces the tidy data you are sharing as output. You can try running your script
a couple of times and see if the code produces the same output. 

In many cases, the person who collected the data has incentive to make it tidy for a statistician to speed the process
of collaboration. They may not know how to code in a scripting language. In that case, what you should provide the statistician
is something called [pseudocode](http://en.wikipedia.org/wiki/Pseudocode). It should look something like:

1. Step 1 - take the raw file, run version 3.1.2 of summarize software with parameters a=1, b=2, c=3
1. Step 2 - run the software separately for each sample
1. Step 3 - take column three of outputfile.out for each sample and that is the corresponding row in the output data set

You should also include information about which system (Mac/Windows/Linux) you used the software on and whether you 
tried it more than once to confirm it gave the same results. Ideally, you will run this by a fellow student/labmate
to confirm that they can obtain the same output file you did. 




What you should expect from the analyst
====================

When you turn over a properly tidied data set it dramatically decreases the workload on the statistician. So hopefully
they will get back to you much sooner. But most careful statisticians will check your recipe, ask questions about
steps you performed, and try to confirm that they can obtain the same tidy data that you did with, at minimum, spot
checks.

You should then expect from the statistician:

1. An analysis script that performs each of the analyses (not just instructions)
1. The exact computer code they used to run the analysis
1. All output files/figures they generated. 

This is the information you will use in the supplement to establish reproducibility and precision of your results. Each
of the steps in the analysis should be clearly explained and you should ask questions when you don't understand
what the analyst did. It is the responsibility of both the statistician and the scientist to understand the statistical
analysis. You may not be able to perform the exact analyses without the statistician's code, but you should be able
to explain why the statistician performed each step to a labmate/your principal investigator. 


Contributors
====================

* [Jeff Leek](http://biostat.jhsph.edu/~jleek/) - Wrote the initial version.
* [L. Collado-Torres](http://bit.ly/LColladoTorres) - Fixed typos, added links.
* [Nick Reich](http://people.umass.edu/nick/) - Added tips on storing data as text.


