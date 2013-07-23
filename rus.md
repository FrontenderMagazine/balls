# Мяч на чистом CSS

Недавно я описал как на CSS можно создать [треугольник][1] с закруглёнными 
уголками используя `border-radius`. Теперь попробуем создать объект в сферической формы. 

## Плоский дизайн

Сферу на CSS можно сделать двумя способами. 

Первый: создать трёхмерную сферу используя множество элементов. Есть много 
[красивых][2] [примеров][3] созданных таким образом. Однако у этого способа есть 
недостаток: браузеру нужно отобразить большое число элементов, что может 
негативно сказаться на производительности. Кроме того, созданные таким 
способом сферы, часто выглядят немного грубовато, ведь чтобы сделать сферу 
действительно гладкой потребуется огромное количество элементов.

Я же попробую второй способ, который состоит в добавлении теней и создании 
3D-эффекта для одного элемента с помощью градиентов CSS. 

## Демо и исходный код

Все приведённые ниже примеры можно найти в [моём профиле на Codepen][4] или 
кликнув по ссылке «Edit on Codepen» в каждом из примеров. 

## Базовая форма

Перед тем как добавлять детали, создадим базовый объект круглой формы. Начнём с 
HTML:

    <figure class="circle"></figure>

Элемент можно взять любой, мы будем использовать элемент `figure`. В HTML5 
элемент `figure` используется для представления изображения или рисунка, который 
является частью контента и может быть удалён без ущерба для остального контента. 

Чтобы сделать из элемента `figure` круг, я задам ему ширину, высоту и 
`border-radius: 50%`. Любое значение для `border-radius` больше 50% даст 
полностью закруглённые уголки. 

    .circle {
      display: block;
      background: black;
      border-radius: 50%;
      height: 300px;
      width: 300px;
      margin: 0;
    }

Вуаля, получаем простой круг.

<iframe id="cp_embed_sdLhc" src="http://codepen.io/donovanh/embed/sdLhc?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

Теперь у нас есть круг, с которым можно дальше работать и мы будем 
превращать его в нечто более напоминающее сферу. 

## Добавляем тень

Одним из первых шагов в большинстве уроков по созданию pureCSS-сфер является 
добавление одного радиального градиента с наиболее светлым участком смещённым 
немного вверх и влево от центра круга.

Это можно сделать с помощью следующего CSS кода:

    .circle {
      display: block;
      background: black;
      border-radius: 50%;
      height: 300px;
      width: 300px;
      margin: 0;
      background: -webkit-radial-gradient(100px 100px, circle, #5cabff, #000);
      background: -moz-radial-gradient(100px 100px, circle, #5cabff, #000);
      background: radial-gradient(100px 100px, circle, #5cabff, #000);
    }

Должно получиться что-то вроде этого:

<iframe id="cp_embed_zDqAs" src="http://codepen.io/donovanh/embed/zDqAs?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

### Радиальные градиенты

Свойство `radial-gradient` принимает несколько параметров. Первый — размещение 
центра (начала) градиента. Затем указывается его форма (круг или эллипс). И 
наконец, последовательность цветов. Можно указать больше чем два цвета, но в 
таком случае нужно также задать расстояние для каждого чтобы было понятно, когда 
каждый из цветов должен переходить в другой.

В этом примере указаны только два цвета. Это позволяет браузеру предположить,
что у первого координаты 0%, а у второго — 100% и отрисовать переход цвета между 
ними. Если мы хотим чтобы координаты цветов были другими, их следует указать в 
пикселях или процентах, как вы увидите позже.

Теперь у нас есть более-менее объемный объект. Он *неплох*, но давайте улучшим 
его. 

## Тени и 3D

Внешний вид сферы можно менять, используя разные виды тени. Однако, сначала 
давайте создадим 3D-среду для нашего мяча. 

В код HTML добавляем ещё несколько элементов:

    <section class="stage">
      <figure class="ball"><span class="shadow"></span></figure>
    </section>

Элемент-мяч мы обернули в `div` с классом `stage` и внутри него поместили `span`, 
который будем использовать для создания тени. Блочный элемент с классом `stage` 
можно использовать для добавления перспективы и позиционирования тени для 
большей объемности.

Стилизируем элемент с классом `stage` и позиционируем тень чтобы создать 
3D-среду. 

    .stage {
      width: 300px;
      height: 300px;
      display: inline-block;
      margin: 20px;
      -webkit-perspective: 1200px;
      -webkit-perspective-origin: 50% 50%;
    }
    .ball .shadow {
      position: absolute;
      width: 100%;
      height: 100%;
      background: -webkit-radial-gradient(50% 50%, circle, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.1) 40%, rgba(0, 0, 0, 0) 50%);
      -webkit-transform: rotateX(90deg) translateZ(-150px);
      z-index: -1;
    }

Обратите внимание, что в коде перспектива прописана только с префиксом `-webkit`.
В коде примеров на Codepen присутствуют все префиксы. Выше я для элемента 
`stage` задал перспективу 1,200 пикселей. Свойство `perspective` создаёт центр 
перспективы в 3D-среде. 

Затем добавлена тень для мяча с помощью радиального градиента со смещённым 
позиционированием заданным через `transform`. Трансформации `transform` в  CSS 
позволяют вращать, масштабировать и наклонять объекты в 3D-пространстве. Тень 
повёрнута на 90 градусов по оси Х и смещена на 150 пикселей вниз к основе мяча. 

Так как мы прописали значение перспективы для контейнера `stage`, то в
результате мы смотрим на круглый элемент `shadow` как бы сверху вниз и видим его
в форме вытянутого овала.

<iframe id="cp_embed_pwGxn" src="http://codepen.io/donovanh/embed/pwGxn?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

Уже выглядит получше. Теперь добавим ещё немного тени на сам мяч.

## Несколько теней

В реальном мире очень трудно встретить объекты, освещённые только с одной 
стороны. Поверхности отражают свет на другие поверхности и в конце концов 
получается смешение нескольких источников света. Чтобы сделать наш мяч более 
реалистичным, мы добавим два градиента используя псевдо-элемент для того,
чтобы казалось, что свет падает из двух источников. 

    .ball {
      display: inline-block;
      width: 100%;
      height: 100%;
      margin: 0;
      border-radius: 50%;
      position: relative;
      background: -webkit-radial-gradient(50% 120%, circle cover, #81e8f6, #76deef 10%, #055194 80%, #062745 100%);
      );
    }
    .ball:before {
      content: "";
      position: absolute;
      top: 1%;
      left: 5%;
      width: 90%;
      height: 90%;
      border-radius: 50%;
      background: -webkit-radial-gradient(50% 0px, circle, #ffffff, rgba(255, 255, 255, 0) 58%);
      -webkit-filter: blur(5px);
      z-index: 2;
    }

Как и раньше в этом примере я использовал только префикс `-webkit`. Теперь у 
нас есть два более сложных градиента. 

Первый градиент, прописанный для элемента `ball`, создаёт впечатление лёгкой 
подсветки снизу. Центр градиента расположен посередине по горизонтали и в точке 
120% от высоты мяча. Таким образом, он расположен за пределами поверхности мяча. 
Я сделал так, чтобы чёткий конечный цвет не был виден и градиент был более 
плавным. 

Второй градиент создаёт эффект яркой подсветки сверху. Его размер равен 90% от 
ширины мяча и 90% от его высоты. Центр градиента находится в верхней части мяча, 
а сам градиент заканчивается примерно по центру мяча.

Вместо того чтобы создавать новый элемент для подсветки, я использовал 
псевдо-элемент `before`. 

Так как у второго градиента острый угол, я его немного смягчил с помощью эффекта 
`blur` с префиксом `-webkit`. К сожалению этот эффект доступен только для 
браузеров на движке webkit (Chrome и Safari), но возможно в будущем станет 
доступным и для других. 

Сочетание двух градиентов даёт очень приятный эффект:

<iframe id="cp_embed_fEaru" src="http://codepen.io/donovanh/embed/fEaru?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

## Больше блеска

Пока что игра света не очень эффектна, так что давайте добавим немного глянца 
чтобы наш мяч стал похожим на бильярдный шар.

Чтобы добиться такого впечатления мы будем используем мягкую подсветку снизу как
и раньше, а вот верхнюю сделаем меньше и чётче. Нам понадобятся два 
псевдо-элемента чтобы задать цвет мяча, нижнюю подсветку и блик.

    .ball {
      display: inline-block;
      width: 100%;
      height: 100%;
      margin: 0;
      border-radius: 50%;
      position: relative;
      background: -webkit-radial-gradient(50% 120%, circle cover, #323232, #0a0a0a 80%, #000000 100%);
    }
    .ball:before {
      content: "";
      position: absolute;
      background: -webkit-radial-gradient(50% 120%, circle cover, rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0) 70%);
      border-radius: 50%;
      bottom: 2.5%;
      left: 5%;
      opacity: 0.6;
      height: 100%;
      width: 90%;
      -webkit-filter: blur(5px);
      z-index: 2;
    }
    .ball:after {
      content: "";
      width: 100%;
      height: 100%;
      position: absolute;
      top: 5%;
      left: 10%;
      border-radius: 50%;
      background: -webkit-radial-gradient(50% 50%, circle cover, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.8) 14%, rgba(255, 255, 255, 0) 24%);
      -webkit-transform: translateX(-80px) translateY(-90px) skewX(-20deg);
      -webkit-filter: blur(10px);
    }

В этом коде для самого элемента `ball` задан базовый цвет в виде едва заметного 
градиента. Псевдоэлемент `before` имитирует более слабую подсветку, градиент 
снова начинается под мячом и создаёт эффект отражения света от поверхности. 

Новым для нас в этом коде является псевдо-элемент `after`. Он содержит круговой 
градиент, который начинается с почти непрозрачного белого цвета по центру и 
переходит в прозрачный в отметке 24%. Это создаёт эффект белого блика, но чтобы 
сделать его более трёхмерным, мы добавляем свойство `transform`. 

С помощью `transform` сдвигаем блик на 80 пикселей влево и на 90 пикселей
вправо, и добавляем наклон. Эффект наклона вытягивает круг по оси Х, так что
блик более похож на тот что можно увидеть на гладком шаре. 

<iframe id="cp_embed_LuEAB" src="http://codepen.io/donovanh/embed/LuEAB?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

### Шар №8

Раз уж мы создаём шар для бильярда, давайте ещё поместим на него цифру 8.

Для цифры 8 и её стилизации нам потребуется дополнительный элемент.

    <section class="stage">
      <figure class="ball">
        <span class="shadow"></span>
        <span class="eight"></span>
      </figure>
    </section>

    .ball .eight {
      width: 110px;
      height: 110px;
      margin: 30%;
      background: white;
      border-radius: 50%;
      -webkit-transform: translateX(68px) translateY(-60px) skewX(15deg) skewY(2deg);
      position: absolute;
    }
    .ball .eight:before {
      content: "8";
      display: block;
      position: absolute;
      text-align: center;
      height: 80px;
      width: 100px;
      left: 50px;
      margin-left: -40px;
      top: 44px;
      margin-top: -40px;
      color: black;
      font-family: Arial;
      font-size: 90px;
      line-height: 104px;
    }

Мы опять используем `border-radius: 50%` чтобы создать круг и позиционируем этот 
круг в верхней части справа с помощью свойства `transform`. Вместо того чтобы 
добавить цифру 8 в разметку, я добавляю её через CSS используя псевдо-элемент 
`before` и затем наклоняю таким же образом, как и круг, в который она помещена.

Получаем глянцевый шар с цифрой 8.

<iframe id="cp_embed_nzmJq" src="http://codepen.io/donovanh/embed/nzmJq?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

### Я слежу за тобой

Огромное преимущество трансформаций в CSS — это то что их можно анимировать. С 
помощью `keyframes` можно описать последовательность трансформаций элемента как 
анимацию. Чтобы это продемонстрировать, я создам глазное яблоко и применю для 
него анимацию.

Первым делом нужно подкорректировать некоторые цвета, использованные в примере с 
бильярдным шаром с цифрой 8. Несколько правок и он будет напоминать глаз. 
Сначала HTML:

    <section class="stage">
      <figure class="ball">
        <span class="shadow"></span>
        <span class="iris"></span>
      </figure>
    </section>

Большинство кода CSS остаётся таким же как и в примере с бильярдным шаром, за 
исключением кода для радужки и зрачка.

    .iris {
      width: 40%;
      height: 40%;
      margin: 30%;
      border-radius: 50%;
      background: -webkit-radial-gradient(50% 50%, circle cover, #208ab4 0%, #6fbfff 30%, #4381b2 100%);
      -webkit-transform: translateX(68px) translateY(-60px) skewX(15deg) skewY(2deg);
      position: absolute;
      -webkit-animation: move-eye-skew 5s ease-out infinite;
    }
    .iris:before {
      content: "";
      display: block;
      position: absolute;
      width: 37.5%;
      height: 37.5%;
      border-radius: 50%;
      top: 31.25%;
      left: 31.25%;
      background: black;
    }
    .iris:after {
      content: "";
      display: block;
      position: absolute;
      width: 31.25%;
      height: 31.25%;
      border-radius: 50%;
      top: 18.75%;
      left: 18.75%;
      background: rgba(255, 255, 255, 0.2);
    }

Для цветной части радужки используем голубой градиент, затем используем 
псевдо-элементы для создания зрачка и блика. Для элемента `iris` ( *англ.: 
iris — радужка прим. переводчика* ) я также добавил свойство `animation`. 
Анимация для элементов задаётся в таком формате: 

    animation: animation-name 5s ease-out infinite;

В этом случае мы будем применять анимацию `animation-name` продолжительностью 5 
секунд, бесконечным повторением и функцией смягчения `ease-out`. При функции 
`ease-out` анимация замедляется достигая конца, что создаёт впечатление большей 
естественности. 

Без анимации мы получаем очень статическое глазное яблоко.

<iframe id="cp_embed_nwsqa" src="http://codepen.io/donovanh/embed/nwsqa?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

Создадим несколько правил `keyframes` с описанием движений глазного яблока.

    @-webkit-keyframes move-eye-skew {
      0% {
        -webkit-transform: none;
      }
      20% {
        -webkit-transform: translateX(-68px) translateY(30px) skewX(15deg) skewY(-10deg) scale(0.95);
      }
      25%, 44% {
        -webkit-transform: none;
      }
      50%, 60% {
        -webkit-transform: translateX(68px) translateY(-40px) skewX(5deg) skewY(2deg) scaleX(0.95);
      }
      66%, 100% {
        -webkit-transform: none;
      }
    }

Правила `keyframes` могут на первый взгляд показаться очень сложными. Их суть в 
описании состояния элемента на каждом этапе последовательности. Для каждого 
состояния указано время в процентах. В этом случае в начале анимации для `iris` 
никакие трансформации не применены. После проигрывания 20% анимации будет 
применена трансформация перемещения и наклона влево. В промежутке между 0% и 20% 
браузером автоматически рассчитывает каким должно быть состояние элемента для 
плавного перехода от одной точки к другой. 

Так продолжается для всех `keyframes`, полная продолжительность анимации 
составляет 5 секунд, как уже упоминалось ранее. 

Здесь представлена версия с префиксом `-webkit`, однако следует создать также 
версии с префиксами `moz`, `ms` и без префикса.

<iframe id="cp_embed_iBolr" src="http://codepen.io/donovanh/embed/iBolr?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

## Пузырьки

Используя комбинацию теней и анимации можно создать различные интересные эффекты.

Как насчёт пузырьков?

Процесс создания пузырька напоминает то, что мы делали раньше, потребуется 
больше прозрачности основного цвета и два псевдо-элемента чтобы добавить больше 
свечения.

В анимации используется трансформация `scale` чтобы пузырёк колебался.

    @-webkit-keyframes bubble-anim {
      0% {
        -webkit-transform: scale(1);
      }
      20% {
        -webkit-transform: scaleY(0.95) scaleX(1.05);
      }
      48% {
        -webkit-transform: scaleY(1.1) scaleX(0.9);
      }
      68% {
        -webkit-transform: scaleY(0.98) scaleX(1.02);
      }
      80% {
        -webkit-transform: scaleY(1.02) scaleX(0.98);
      }
      97%, 100% {
        -webkit-transform: scale(1);
      }
    }

Анимация применяется для всего пузырька и его псевдо-элементов. 

<iframe id="cp_embed_pvrzK" src="http://codepen.io/donovanh/embed/pvrzK?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

## Использование изображений

Пока что мы не использовали ни одного изображения при создании шаров. 
Использование фонового изображения позволяет сделать внешний вид шара сложнее и 
в то же время использовать преимущества добавления тени с помощью 
псевдо-элементов. К примеру, вот текстура теннисного мяча без теней: 

![мяч][Изображение теннисного мяча без теней]

Добавление градиентов в CSS поможет добиться иллюзии объёма. 

<iframe id="cp_embed_vsEIK" src="http://codepen.io/donovanh/embed/vsEIK?type=result&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

### Вокруг света

Анимацию можно применять и к размещению фоновых изображений. Таким образом можно 
создать вращающийся глобус. 

Это плоское изображение было немного вытянуто сверху и снизу чтобы его можно 
было использовать как фоновое. 

![карта][Плоская карта мира]

С помощью теней и анимации можно создать 3D-глобус. Кликните по Result в этом 
демо с codepen.io чтобы увидеть глобус в движении. Я установил отображение HTML
по умолчанию, так как воспроизведение этого примера сильно влияет на 
производительность, в результате чего вентилятор на моём рабочем ноутбуке 
отбросил коньки. 

Приметка: Большое спасибо Сидорук Сергею ([@Sidoruk_SV][5]) за улучшение этого 
глобуса. Теперь он выглядит потрясающе.

<iframe id="cp_embed_GBIiv" src="http://codepen.io/donovanh/embed/GBIiv?type=html&amp;height=400&amp;safe=true" data-height="400" allowtransparency="true" class="cp_embed_iframe" style="width: 100%;border: none; overflow: hidden;" frameborder="0" height="400" scrolling="no"></iframe>

## Дополнительные материалы

[Полезная информация о радиальных градиентах][6] если вы хотели бы узнать о них 
больше. 

Ищете другие примеры создания 3D-объектов? Взгляните на [«Zelda — связь с CSS»][7] и [«Воссоздание сцены из обучающего видео игры PORTAL с помощью CSS»][8]. 

[1]: http://hop.ie/blog/zelda/
[2]: http://codepen.io/peterwestendorp/pen/wGECk
[3]: http://www.cssplay.co.uk/menu/cssplay-3D-sphere.html
[4]: http://codepen.io/donovanh
[5]: https://twitter.com/Sidoruk_SV
[6]: https://developer.mozilla.org/en-US/docs/Web/CSS/radial-gradient
[7]: http://hop.ie/blog/zelda/
[8]: http://frontender.info/portal/

[Изображение теннисного мяча без теней]: img/tennisball.png
[Плоская карта мира]: img/world-map-one-color.png
