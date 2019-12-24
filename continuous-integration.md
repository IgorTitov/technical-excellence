# Непрерывная интеграция / Continuous Integration

| Источник | Статус |
| - | - |
| [Ссылка](https://less.works/ru/less/technical-excellence/continuous-integration.html) | *В работе* |

## Содержание

- [Введение](#Введение)
- [Практики непрерывной интеграции](#Практики-непрерывной-интеграции)
- [Заключение](#Заключение)
- [Полезные ссылки](#Полезные-ссылки)
- [Сноски](#Сноски)

## Введение

> Использование COBOL калечит разум; исходя из этого, обучение этому языку должно быть признано уголовно наказуемым
> преступлением.
> --Эдсгер Дейкстра

Непрерывная интеграция (CI) является основой для масштабирования бережливой и гибкого разработки ПО:

> Мы пришли к выводу, что нет никаких причин, по которым процессы непрерывной интеграции и автоматической сборки не будут
> масштабироваться на команды любого размера. На самом деле ... [они] становятся более важными, чем когда-либо.
> [Magennis07](http://blog.aspiring-technology.com/file.axd?file=Continuous+Integration+at+Enterprise+Scale.pdf)

С помощью CI разработчики постепенно развивают стабильную систему, работая небольшими порциями и короткими циклами. Это
позволяет командам работать над общим кодом, улучшает его качество и повышает прозрачность разработки.

Существует неправильное представление о CI; это кажется простой концепцией, но на практике это не так. Попробуем избавиться
от одного из частых заблуждений: *Непрерывная Интеграция это не автоматизация сборки или запуск тестов*.

[Классическая статья о **CI**](https://martinfowler.com/articles/continuousIntegration.html) постулирует:

> Непрерывная интеграция ― это практика разработки программного обеспечения, в которой участники команды часто выполняют
> интеграцию своих изменений. Как правило, каждый участник выполняет интеграцию как минимум раз в день, и в итоге
> достигается такой режим работы, при котором интеграция выполняется несколько раз в день. Каждая интеграция проверяется
> путем автоматической сборки (включающей тестирование), что позволяет находить ошибки интеграции как можно скорее.

## Практики непрерывной интеграции

Непрерывная интеграция...

- это практика разработки...
- сохраняет работоспособность системы
- приветствует небольшие изменения
- способствует "выращиванию" системы
- проводится по крайней мере ежедневно
- сохраняет основную ветку в рабочем состоянии
- поддерживается CI-системами
- поддерживается большим количеством автоматических тестов

### Практика разработки

Слишком часто дискуссии о CI касаются инструментов автоматизации. Хотя CI по сути является практикой разработки. Оуэн
Роджерс, один из первых создателей CruiseControl.NET<sup>1</sup>, пишет:

> Непрерывная интеграция - это практика, и речь идет о том, что люди делают, а не о том, какие инструменты они используют.
> Когда проект начинает масштабироваться, легко попасть в ловушку, думая, что команда практикует непрерывную интеграцию
> только потому, что все инструменты настроены и работают. Если разработчики не взяли себе за правило регулярной
> интегрировать свои изменения или поддерживать интеграционную среду в рабочем состоянии, то они не практикуют непрерывную
> интеграцию. > И точка. [[Rogers04](https://link.springer.com/chapter/10.1007%2F978-3-540-24853-8_8)]

Разбиение изменений в коде на небольшие порции, их интеграция, не менее 1 раза в день, и не ломающаяся сборка - все это
выполняется каждым разработчиком. Для этого нужен навык работы с небольшими приращениями и постоянной синхронизацией своей
собственной копии системы (или ее части) с общей версии.

Внедрение CI требует изменения *поведения людей*. Мы работали в несколькими крупных продуктах с отлично автоматизированной
сборкой, но разработчики не часто интегрировали свой код. Хуже того, культура "не ломайте сборку" активно насаждалась, и в
том числе через позор людей, сломавших сборку. Как вы думаете, к какому результату это привело? Разработчики стали
откладывать интеграции из-за страха что-то сломать. Несмотря на их превосходную *всегда зеленую* (всегда проходящую)
автоматизированную сборку, они делают вещи, прямо противоположные практике CI.

Помогает разработка через тестирование (TDD) с постоянным рефакторингом. Когда разработчик тестирует свой код модульным
тестом, он гарантирует, что его локальная копия всегда работает. Все тесты должны проходить все время. Теоретически он может
интегрировать общий код каждый цикл TDD (около десяти минут<sup>2</sup>), но на практике достаточно это делать после пары
циклов.

CI для больших продуктов сложен именно потому, что это практика разработки. Если бы речь шла только об инструментах
автоматизации, вы могли бы просто начать проект по внедрению CI или нанять другую компанию для этой работы. Но как практика
разработки, CI требует изменения в повседневных привычках всех разработчиков. Для многих людей это сложно, требует времени и
постоянных тренировок.

При наличии правильных привычках, разработчики будут...

## Держать систему в рабочем состоянии

По аналогии с
[концептом бережливого производства - **Дзидока(jidoka)**](https://less.works/ru/less/principles/lean-thinking.html), CI
подразумевает всегда держать систему в рабочем состоянии. Если тест не пройден, при локальном прогоне или в CI-инструменте,
разработчик исправляет его немедленно и, следовательно, всегда сохраняет систему в рабочем состоянии.

Традиционный последовательный процесс разработки постоянно имеет дезинтегрированную незавершенную работу (work in progress,
WIP). Никто не знает, работают ли эти части системы вместе и есть ли дефекты. WIP затрудняет прогнозирование того, когда
система может быть готова. CI повышает прозрачность, устраняя этот WIP, всегда сохраняя интегрированным все части между
собой и обеспечивая контроль и предсказуемость процесса.

___
**Примечание: CI и итеративно-инкрементальное разработка в Scrum имеют одинаковую стратегию. Тем не менее, CI является более
конкретной практикой, чем итерация в Scrum. Оба уменьшают изменчивость, неопределенность и риск, работая c небольшими
порциями итеративно.**
___

Такая система, находящаяся постоянно в рабочем состоянии, развивается небольшими шагами, создается с помощью ...

### "Резиновая курица"

Разработчик Джеймс Шор, автор книги "Искусство гибкой разработки" (The Art of Agile Development)
[[SW07](https://www.amazon.com/Art-Agile-Development-Pragmatic-Software/dp/0596527675)],
подчеркивает, что CI это практика разработки. В своей отличной статье "Непрерывная интеграция за 1 доллар в день"
[[Shore06](https://www.jamesshore.com/Blog/Continuous-Integration-on-a-Dollar-a-Day.html)], он объясняет, как сделать CI из
старого компьютера, резиновой курицы, настольного звонка и автоматизированной сборки. Старый компьютер используется в
качестве сервера интеграции. резиновая курица это *маркер интеграции* - только тот, у кого она, может интегрировать код.
Настольный звонок объявляет об успешной интеграции. Но самый важный шаг в его описание CI состоит в том, чтобы собрать
разработчиков в одной комнате и дать им возможность договориться друг с другом: "Отныне наш код в системе контроля версий
всегда будет успешно собираться и проходить все тесты". Резиновая курица не подходит для крупных продуктов. Тем не менее,
это история хороший способ запомнить, что CI - это практика разработки.

![continuous-integration-chicken.png](img/continuous-integration-chicken.png)

## Небольшие изменения

Однажды мы работали с продуктовой группой в Финляндии, которая разрабатывала шлюз. Им нужно было внести изменения в их стек
протоколов. Они настаивали на том, что его нельзя разделить на небольшие изменения. Они внесли много правок и потратили три
месяца, пытаясь заставить систему снова работать. После этого мучительного опыта они согласились никогда не вносить такие
большие изменения "за один раз".

Большие изменения в работающей системе в значительной степени дестабилизируют и разрушат ее. Чем больше изменение, тем
больше времени требуется, чтобы стабилизировать ее состояние.

Избегайте больших изменений. Вместо этого разбивайте каждое изменение на мелкие части - небольшие партии, одна из концепций
бережливого производства. Каждое изменение легко интегрируется в систему.

С небольшими изменениями вы будете ...

## "Выращивать" систему

*Выращивание*, а не строительство - важная смена парадигмы мышления. Фредерик Брукс в своей знаменитой статье
[No Silver Bullet](http://www.cs.nott.ac.uk/~cah/G51ISS/Documents/NoSilverBullet.html) отражает свой опыт:

> Метафора "строительства" исчерпала свою полезность... Если, как я полагаю, концептуальные структуры, которые мы строим
> сегодня, слишком сложны, чтобы их можно было точно определить заранее, и слишком сложны, чтобы их можно было безошибочно
> построить, тогда мы должны использовать радикально иной подход... Секрет в том, что это **выращено, а не построено**...
> Харлан Миллс предложил, чтобы любая программная система росла путем инкрементальной разработки... Ничто за последнее
> десятилетие так радикально не изменило мою собственную практику или ее эффективность... Моральные эффекты поражают.
> Энтузиазм резко возрастает, когда есть работающая система, пусть и самая простая... На каждом этапе процесса всегда есть
> работающая система. Я нахожу, что команды могут "вырастить" гораздо более сложные структуры за четыре месяца, чем они могут
> "построить".

*Строительство* системы подразумевает сборку отдельных компонентов и, когда они закончены, их совместную сборку.
*Выращивание* системы подразумевает ее развитие и превращение в более крупную систему (см. "Выращивание против
строительства").

![continuous-integration-grow-versus-bolt.png](https://less.works/img/technical-excellence/xcontinuous-integration-grow-versus-bolt.png.pagespeed.ic.edbUunw5nz.webp)

выращивание против строительства
___

Возможно ли это в больших системах с унаследованными кодом? Нас часто спрашивают об этом. Почти в каждом случае ответ - да.
Если ваши разработчики или архитекторы не могут этого сделать или утверждают, что это невозможно, воспринимайте это как
признак отсутствия у них соответствующих навыков.

Разработчик постоянно интегрирует свою работу, работая над задачей. Он не ждет завершения задачи или всей задачи, чтобы затем
"прикрутить" ее к системе. Скорее, всякий раз, когда небольшой объем работы может быть интегрирован без разрушения системы,
он интегрирует ее...

## По крайней мере ежедневно
"Непрерывно" это как часто? Как можно чаще! Это может быть ограничено лишь:

- способностью делить большие изменения на части
- скоростью интеграции
- скоростью цикла обратной связи

**Способность делить большие изменения на части** - разбивать большие изменения на более мелкие, сохраняя при этом
работоспособность старой функционала - это навык, который необходимо освоить. Чем лучше разработчики умеют декомпозировать,
тем чаще они могут интегрироваться. TDD в коротких десятиминутных циклах, является отличной техникой для этого.

**Скорость интеграции** - чем больше времени требуется для интеграции изменений в репозиторий кода, тем реже разработчики
будут это делать. Изменения объединяются ради эффективности. На усилия по интеграции влияют накладные расходы 
(транзакционные издержки), такие как согласования и проверки, необходимые для того, чтобы разработчикам было разрешено
интегрироваться. Уменьшите эти накладные расходы или найдите новые креативные способы сделать что-то по-другому. Например,
мы работали с командой продуктовой группы, состоящей из 40 человек, где в сообщении при вливании изменений в основную ветку
должно было быть упомянут человек, просмотревший этот код. Как вы думаете, какой результат? Разработчики собирали все
изменения в одну пачку, чтобы сделать обзор кода более "эффективными" и, следовательно, откладывали интеграцию до последнего.
Что является локальной оптимизацией. Какое решение? Вместо этого обзор изменений может выполняться на уже интегрированном
коде, что не откладывает интеграцию.

**Скорость цикла обратной связи** - разработчик должен интегрировать только те изменения, которые не нарушают существующие
тесты. В идеале он запускает все тесты перед интеграцией. Чтобы это было возможно, тесты должны выполняться очень быстро.
Если они будут медленными, разработчик отложит интеграцию, чтобы "работать более эффективно". Однако быстро выполнить все
тесты непростая задач для больших систем. Поэтому разработчики запускают только часть тестов перед слиянием кода, а
CI-система запускает остальные тесты. CI-система действует как система безопасности, предоставляя разработчику информацию о
тех тестах, которые он не проводил. Что происходит, когда CI-система работает медленно? Во-первых, в течение одного цикла
произойдет достаточно много изменений, что увеличит вероятность того, что сборка сломается. Во-вторых, разработчики не
будут интегрировать свои изменения в сломанную сборку; Скорее, они будут их накапливать. Наконец, когда сборка исправлена,
все разработчики интегрируют свои накопленные изменения, что повышает вероятность повторного обрушения сборки. Поэтому цикл
проверки изменений должен быть быстрым. Это уменьшает вероятность поломки билда и увеличивает возможность более частого
слияния кода.

Практическое правило для крупных продуктов, переходящих на гибкую и бережливую разработку: все разработчики интегрируются по
крайней мере ежедневно. Несмотря на то, что "*ежедневная сборка для слабаков*"
[Jeffries04](http://www.amazon.com/Extreme-Programming-Adventures-Developer-Reference/dp/0735619492), *интегрироваться
ежедневно* - это первый шаг для таких продуктов. Вспомните метафору "озеро и скалы", используемую в бережливом мышлении.
Большой камень - отсутствие возможности сделать сборку системы раз в день, *учтя изменения всех разработчиков* - является
достаточно тяжелым препятствием в больших старых системах с тремя сотнями программистов в четырех странах. В конце концов,
если удалить этот камень, то становятся возможными более короткие циклы обратной связи.

В больших продуктах потребуется время, чтобы научиться разбивать изменения, упростить процесс интеграции и настроить быструю
CI-систему, работающую...

## Заключение

## Полезные ссылки

## Сноски

1. [CruiseControl.NET](http://cruisecontrol.sourceforge.net/) - это CI-сервер для платформы for Microsoft .NET.
2. Для разработки на Java 10 минут - это слишком много. Для C++ в среднем нормально. Для C, возможно даже мало. 10 минут -
среднее время TDD-цикла независимо от языка и платформы.