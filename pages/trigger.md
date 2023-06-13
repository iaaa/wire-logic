---
layout: page
title: Триггер
---
# {{ page.title }}

Триггер, институтская *любовь* всех студентов...

Триггер, это элементарное запоминающее устройство. Если вы заметили, то выходы всех предыдущих логических элементом строго зависели от их входов.
Зная, где на каких входах есть питание мы могли точно сказать, не смотря глазами, будет ли питание на выходе. Эти элементы "не имели памяти".

Триггер же память имеет. Это значит, что если закрыть выход и показать вам только вход - вы не скажете точно что на выходе (точнее - не всегда не скажете, но об этом как раз дальше). И это очень хорошо! Ведь мы строим компьютер, а не логарифмическую линейку.

Итак, триггер (в рассматриваемом случае - RS-триггер, по именам входов; желающим почитать про остальные - [википедия](https://ru.wikipedia.org/wiki/Триггер){:target="_blank"}) - это элементарное запоминающее устройство, которое сохраняет состояние своего выхода, когда на входах у него нули (не подано питание). То есть мы подаем питание, записываем нужное в триггер и отключаемся. А триггер помнит и выдает на выход то, что мы перед этим в него записали.

Cоберем его из привычных логических элементов - двух NAND с обратно заведенными на вход выходами.

Триггер переключает выходной сигнал по выбору одного из входных. Если запитать вход S (от set) - то выход (Q) переключится в 1. Если выбрать R (reset) - то в 0.
Вариант с одновременным включением обоих входов является запрещенным. Почему запрещенным - попробуйте сами (поставьте на паузу, включите оба входа, снимите с паузы; ttl в этом схеме остается обычный, 5 тактов). Кто не попробовал - тому tldr: потому, что триггер начинает колбасить и он переключается попеременно то туда то сюда.

Часто для удобства второй выход (инверсный к первому) тоже выводят и используют.

RS-триггер
{: .ui .pointing .below .label}
```layout
:o white
:O white
:i cyan :* cyan
:y orange
+-------------------------------------+
|                                     |
| oooo                          oo    |
|                              o  o   |
|  ooo                         o  o   |
| o      OO    OO              o oo   |
|  oo  iiO ooooO oo             oo o  |
|    o   OO    OO OO  OO              |
| ooo             O ooO ooooooooyyy   |
|              OO OO  OO   o          |
|            ooO oo        o          |
|            o OO          o          |
|            o             o   oooo   |
|            ooooooooooooo o          |
|                        o o    oo    |
|            oooooooooooo oo   o  o   |
|            o           o     o  o   |
|            o OO        o     o oo   |
| oooo       ooO oo      o      oo o  |
|              OO OO  OO o            |
| ooo             O ooO oooooooo      |
| o  o   OO    OO OO  OO              |
| ooo  *iO ooooO oo                   |
| o o    OO    OO                     |
| o  o                                |
|                                     |
+-------------------------------------+
```
{:animated="auto"}

Это был обычный триггер, который реагировал сразу же, как на вход подать сигнал. Что называется - асинхронный.

Но что, если мы хотим управлять не только установкой значения триггера, но и моментом времени, когда устанавливать? Зачем? Потому, что в будущем мы захотим синхронизировать разные блоки нашего процессора (об этом еще будет). То есть нам нужен триггер синхронный.

Чуть позже я добавлю сюда большую полноценную схему с синхронным триггером для того, чтобы ее внимательно рассмотреть и пощупать. А пока у нас - оптимизированный, красивый синхронный триггер с одним входом. Почему одним? Читаем дальше:

Можно заметить, что полезные (устанавливающие) сигналы входов всегда противоположны. Если мы хотим записать 1, мы включаем S и выключаем R. А если 0 - наоборот. Так почему бы не завести только один вход, который будет передавать прямой сигнал на R и инвертированный на S? При этом добавив отдельный управляющий сигнал (обычно называемый C, или CLK), который будет разрешать запись в триггер и позволит нашему задающему значение сигналу проходить только тогда, когда он включен.

Вот эта схема. Компактная и красивая. Голубой - задающий сигнал (1 - значит запомнить "1", 0 - значит запомнить "0"). Желтый - разрешающий запись (наш clk). А зеленый - выход триггера, его "запомненное" состояние.

На схеме триггер изначально с "0". Чтобы выставить его в "1" надо выставить голубую и желтую линии в активное состояние. Чтобы сбросить - достаточно снова включить желтую линию при неактивной голубой. Я увеличу ttl до трех секунд, чтобы было легче переключать.
```layout
:x white
:o yellow :* yellow
:c cyan
:g green
+--------------------+
| c        *         |
| c        o         |
| cc xx xx o    gggg |
| c xx xx x xxx gggg |
| cc xx xx o x ggggg |
| c     x  o xx gggg |
| c    o ooo x  gggg |
| c    ooo   x ggggg |
| c    o o   xx gggg |
| cc    x  xx  ggggg |
| c xxxxxxxx ggggggg |
| cc       xx gggggg |
|                 g  |
+--------------------+
```
{:animated="auto" ttl="60"}

Прошу внимательнее посмотреть на схему. Видно, что неактивный желтый sync блокирует сигналы входа (белые сигналы к зеленому выходу), а вот выключенный - наоборот, пропускает. При этом включенный голубой вход выключает нижний белый, который в свою очередь включает выход. А выключенный голубой вход - наоборот, включает инвертированный верхний белый и на выход не подается вообще никакого сигнала, выключая его. Не забываем, что проводник переходит в активное состояние (в "1", под "напряжение") если на него хоть где-то это напряжение подать. Если не подавать нигде, то он и не включится сам.

Вот, чтобы удобнее было рассмотреть я размещу хорошо замедленную увеличенную копию (до 1 такта в секунду), ttl обычный, 5 тактов.
```layout
:x white
:o yellow :* yellow
:c cyan
:g green
+--------------------+
| c        *         |
| c        o         |
| cc xx xx o    gggg |
| c xx xx x xxx gggg |
| cc xx xx o x ggggg |
| c     x  o xx gggg |
| c    o ooo x  gggg |
| c    ooo   x ggggg |
| c    o o   xx gggg |
| cc    x  xx  ggggg |
| c xxxxxxxx ggggggg |
| cc       xx gggggg |
|                 g  |
+--------------------+
```
{:animated="auto" fps="1" scale="20"}

Выход у нас такой большой только для красоты - чтобы его было хорошо видно на большой схеме. Ведь это же самый настоящий бит! А из битов собираются байты, из байтов - память, а память... Но про память тоже дальше, в соответствующем разделе.

Впрочем, таки давайте соберем один байтик. Люблю, знаете ли, поиграться с этими схемами: ставьте паузу, выбирайте значение байта, включайте clk, снимайте паузу и наслаждайтесь.

Ячейка памяти
{: .ui .pointing .below .label}
```layout
:x white
:o yellow :* yellow
:c cyan :с red
:g green
+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                                                                                                                                                                |
| ccccc             ccccc               ccccc               ccccc               ccccc               ccccc               ccccc               ccccc                |
| ccccc             ccccc               ccccc               ccccc               ccccc               ccccc               ccccc               ccccc                |
| ccccc             ccccc               ccccc               ccccc               ccccc               ccccc               ccccc               ccccc                |
| c                   c                   c                   c                   c                   c                   c                   c                  |
| c                   c                   c                   c                   c                   c                   c                   c  oooooooooooooo  |
| c                   c                   c                   c                   c                   c                   c                   c  oooooooooooooo  |
| c                   c                   c                   c                   c                   c                   c                   c  oo   o oo o oo  |
| c                   c                   c                   c                   c                   c                   c                   c  oo ooo oo  ooo  |
| c                   c                   c                   c                   c                   c                   c                   c  oo   o  o o oo  |
| c                   c                   c                   c                   c                   c                   c                   c  oooooooooooooo  |
| c                   c                   c                   c                   c                   c                   c                   c  oooooooooooooo  |
| c                   c                   c                   c                   c                   c                   c                   c        o         |
| c        ooooooooooo ooooooooooooooooooo ooooooooooooooooooo ooooooooooooooooooo ooooooooooooooooooo ooooooooooooooooooo ooooooooooooooooooo oooooooo*         |
| c        o          c        o          c        o          c        o          c        o          c        o          c        o          c        o         |
| c        o          c        o          c        o          c        o          c        o          c        o          c        o          c        o         |
| cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg  cc xx xx o    gggg |
| c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg  c xx xx x xxx gggg |
| cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg  cc xx xx o x ggggg |
| c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg  c     x  o xx gggg |
| c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg  c    o ooo x  gggg |
| c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg  c    ooo   x ggggg |
| c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg  c    o o   xx gggg |
| cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg  cc    x  xx  ggggg |
| c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg  c xxxxxxxx ggggggg |
| cc       xx gggggg  cc       xx gggggg  cc       xx gggggg  cc       xx gggggg  cc       xx gggggg  cc       xx gggggg  cc       xx gggggg  cc       xx gggggg |
|                 g                   g                   g                   g                   g                   g                   g                   g  |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
```
{:animated="auto" scale="3"}

А сейчас, наигравшись с битами и байтами, перейдем к еще одному элементарному блоку нашей схемы - [тактовому генератору...](clock-generator.html).
