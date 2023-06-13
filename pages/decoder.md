---
layout: page
title: Декодер
---
# {{ page.title }}

Логические элементы, конечно, хорошо. Но скучно. Они очень простые и совсем не интересные.

Так что давайте с помощью вышеперечисленной элементарной логики сделаем что-нибудь посложнее и попрактичнее? Что-нибудь полезное, что на самом деле используется в схемотехнике? И что мы точно будем использовать в нашем микропроцессоре.

В тех же институтах почти сразу рассказывают, например, о (де)кодерах. Декодер (схемотехнический), это такое устройство, которое принимает на вход кодированный сигнал (двоичное число, например), а на выход выдает линейный, "декодированный" (подает напряжение на линию номер "входное число"). То есть, если на трех проводках входа закодировано бинарное число 3 (011<sub>2</sub>), то декодер подаст напряжение только на 4-ый выход (четвертый, потому что 0 - первый, 1 - второй и т.д.). А если число 1 (001<sub>2</sub>) - то на второй.

Нам декодер будет нужен, и не раз. Например, чтобы активировать N-й провод шины по взятому из ПЗУ битовому числу.

На схеме такой декодер. Слева - вход(ы), бинарное число; младший бит, нулевой, вверху; старший внизу. Справа - выходы, подписанные контактные площадки, первый выход (номер "ноль") вверху, последний внизу.

Попереключайте входы, я выставил ttl на 30 секунд, чтобы не спешить.

Двоичный декодер
{: .ui .pointing .below .label}
```layout
:o white :c cyan :x orange :r red
+------------------------------------------------+
|                                                |
|         o   o   o  oo oo        ooooooooooo    |
|         oooo ooo ooo oo oo      ooooooooooo    |
|         o   o   o  oo oo o      oooo   oooo    |
| ooo     o   o   o        o      ooo ooo ooo    |
| o o     o   o   o  oo oo oo     ooo ooooooo    |
| o o     o   oooo ooo oo oo oooooooo ooo oxxxx  |
| o o     o   o   o  oo oo oo     ooooooo ooo    |
| ooo     o   o   o        o      ooo ooo ooo    |
|ccccoooooo   o   o  oo oo o      oooo   oooo    |
|         o   o   oooo oo oo      ooooooooooo    |
|  o      o   o   o  oo oo        ooooooooooo    |
| oo      o   o   o                              |
|  o      o   o   o     oo        ooooooooooo    |
|  o      oooo ooo oooooo oo      ooooooooooo    |
|  o      o   o   o     oo o      ooooo ooooo    |
|ccccooooo oooo   o        o      oooo  ooooo    |
|         o   o   o  oo oo oo     ooooo ooooo    |
| ooo     o   oooo ooo oo oo oooooooooo oooxxxx  |
|   o     o   o   o  oo oo oo     ooooo ooooo    |
| ooo     o   o   o        o      ooooo ooooo    |
| o       o   o   o  oo oo o      ooo     ooo    |
| ooo     o   o   oooo oo oo      ooooooooooo    |
|ccccooooo ooo oooo  oo oo        ooooooooooo    |
|         o   o   o                              |
|         o   o   o  oo oo        ooooooooooo    |
|         oooo ooo ooo oo oo      ooooooooooo    |
|         o   o   o  oo oo o      oooo   oooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o     oo oo     ooooooo ooo    |
|         o   oooo oooooo oo oooooooooo  ooxxxx  |
|         o   o   o     oo oo     oooo oooooo    |
|         o   o   o        o      ooo ooooooo    |
|         o   o   o  oo oo o      ooo     ooo    |
|         o   o   oooo oo oo      ooooooooooo    |
|         o   o   o  oo oo        ooooooooooo    |
|         o   o   o                              |
|         o   o   o     oo        ooooooooooo    |
|         oooo ooo oooooo oo      ooooooooooo    |
|         o   o   o     oo o      oooo   oooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o     oo oo     ooooooo ooo    |
|         o   oooo oooooo oo oooooooooo  ooxxxx  |
|         o   o   o     oo oo     ooooooo ooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o  oo oo o      oooo   oooo    |
|         o   o   oooo oo oo      ooooooooooo    |
|         o   o   o  oo oo        ooooooooooo    |
|         o   o   o                              |
|         o   o   o  rr rr        rrrrrrrrrrr    |
|         oooo ooo ocr rr rr      rrrrrrrrrrr    |
|         o   o   o  rr rr r      rrrrrr  rrr    |
|         o   o   o        r      rrrrr r rrr    |
|         o   o   o  rr rr rr     rrrr rrrrrr    |
|         o   oooo ocr rr rr rrrrrrrr rrr rxxxx  |
|         o   o   o  rr rr rr     rrr     rrr    |
|         o   o   o        r      rrrrrrr rrr    |
|         o   o   o     rr r      rrrrrrr rrr    |
|         o   o   oocrrrr rr      rrrrrrrrrrr    |
|         o   o   o     rr        rrrrrrrrrrr    |
|         o   o   o                              |
|         o   o   o     oo        ooooooooooo    |
|         oooo ooo oooooo oo      ooooooooooo    |
|         o   o   o     oo o      ooo     ooo    |
|         o   o   o        o      ooo ooooooo    |
|         o   o   o  oo oo oo     ooo    oooo    |
|         o   oooo ooo oo oo oooooooooooo oxxxx  |
|         o   o   o  oo oo oo     ooooooo ooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o     oo o      oooo   oooo    |
|         o   o   ooooooo oo      ooooooooooo    |
|         o   o   o     oo        ooooooooooo    |
|         o   o   o                              |
|         o   o   o  oo oo        ooooooooooo    |
|         oooo ooo ooo oo oo      ooooooooooo    |
|         o   o   o  oo oo o      ooooo  oooo    |
|         o   o   o        o      oooo oooooo    |
|         o   o   o     oo oo     ooo ooooooo    |
|         o   oooo oooooo oo oooooooo    ooxxxx  |
|         o   o   o     oo oo     ooo ooo ooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o     oo o      oooo   oooo    |
|         o   o   ooooooo oo      ooooooooooo    |
|         o   o   o     oo        ooooooooooo    |
|         o   o   o                              |
|         o   o   o     oo        ooooooooooo    |
|         oooo ooo oooooo oo      ooooooooooo    |
|         o   o   o     oo o      ooo     ooo    |
|         o   o   o        o      ooo ooo ooo    |
|         o   o   o     oo oo     ooooooo ooo    |
|         o   oooo oooooo oo ooooooooooo ooxxxx  |
|         o   o   o     oo oo     ooooo ooooo    |
|         o   o   o        o      ooooo ooooo    |
|         o   o   o     oo o      ooooo ooooo    |
|         o   o   ooooooo oo      ooooooooooo    |
|         o   o   o     oo        ooooooooooo    |
|                                                |
+------------------------------------------------+
```
{:animated="auto" ttl="600"}

Как работает этот декодер?

* Во-первых, в нем можно выделить 8 горизонтальных логических блоков (я выделил красненьким один из них) на три входа каждый (там же, голубеньким) и со своим подписанным выходом.

* Во-вторых, на каждый из этих входов навешены инвертеры в бинарном виде (000, 001, 010, 011, 100, 101, 110, 111, младший вверху; 1 - NOT, 0 - NOT+NOT), которые заведены на элемент AND из прошлого раздела. При этом пары инверторов (NOT + NOT) играют роль диодов, не позволяя сигналам с элемента AND распространятся обратно к другим входам других элементов AND, нарушая логику, при этом не меняя сам сигнал. Вот и первое практическое применение "диодов", о которых мы упоминали ранее.

* В-третьих, три главных входа выводятся на общую вертикальную шину, от которой одинаково размножаются на каждый логический блок (здесь как раз хорошо видно правило "пересечение проводников является пересечением проводников").

Вот и все. При кажущемся засилии вентилей все довольно просто.

Поиграйтесь с декодером, это забавно. Я даже сейчас, когда перепроверяю этот написанный текст, с удовольствем их переключаю.
При желании, количество входов можно увеличивать многократно. Правда, количество выходов будет расти экспоненциально - увеличиваем входы на 1, увеличиваем выходы в 2 раза. Вот она - наглядная разница между позиционными счислением и счетным )).

А дальше у нас на очереди - страшные [триггеры...](trigger.html)
