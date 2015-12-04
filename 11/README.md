# Тема
На цьому занятті ми продовжуємо гратися з двома концепціями:
* Функції
* Кінцеві автомати

# Завдання для самостійних
* Скачуєш початковий код з [labirynth_game_over](labirynth_game_over)
В ньому герой рухається, малюється карта і при наступанні на воду закінчується гра
* Додаєш дракончика чи якогось антигероя за твоїм смаком
* Робиш щоб він бігав за нашим героєм коли його бачив
* Коли він доганяє героя - гра закінчується
* Коли він не бачить героя - він пробує іти в випадковому напрямі (гугли функцію random в Пітоні)

* Далі додаємо на карту артефакт, який додає герою додаткове життя і при стиканні з драконом нас просто телепортує на початок

# Що тобі може стати в нагоді
* Концепція MVC(Model View Controller) [тутай](../09)
* [Відеоінструкція за якою можна повторювати](https://www.youtube.com/watch?v=qDTuIHNQvFQ#t=884)

## Покрокове 
### 1. Додаємо дракона (в прикладах коду спеціально є невеличкі помилки, дивися уважно і запускай після кожного нового шматку коду)
Model: змінні для координат, вписуємо до інших змінних на початку програми
````
dragon_y = 9; # вкажи тута де на карті має почати гру дракон
dragon_y = 9;
````

View: малюємо дракона (ми це робимо тільки якщо ігра в процесі, тому додай цей код кудись всередину функції `draw_in_progress`
````
dragon_image = loadImage("dragon.gif")
draw_image(dragon_image, dragon_x, dragon_x) # зверни увагу що dragon_image не вбудована функція а нова створена нами на минулому занятті, вона розуміє ігрові, а не екранні координати
````

Controller: якщо зіткнулися з драконом завершуємо гру
1. цей код має бути десь після обробки всіх клавіш (було б дивно, якщо ми б помирали, а після того ходили - ми маємо помирати після ходіння!
2. цей код має виконуватися тільки коли гра в режимі гри 
3. 
````
if (dragon_x == hero_x) and (dragon_y == hero_y):
    game_state = game_over
````

### 2. Робимо щоб дракон просто йшов вправо
Додамо функцію move_dragon
````
def move_dragon():
    global dragon_y
    dragob_x = dragon_x + 1
````

Її викликаємо регулярно, для цього:
1. додаємо виклик цієї функції десь в методі draw(бо draw викликається на кожен кадр, або 30 раз в секунду)
2. в setup додаємо зміну кількості кадрів, щоб дракон бігав не так швидко: frameRate(2)

### 3. Робимо, щоб дракон ходив у випадковому напрямі якщо він не бачить героя
Створимо функцію can_see_hero() яка буде казати бачить він чи ні і зараз він завжди не буде бачити (для простоти)
````
def can_see_hero():
    return True    # це значення яке повертає функція
````

Додамо просту функцію для пересування, далі її покращимо
````
def move_dragon_randomly():
    pass # ця команда нічого не робить, але вона потрібна, бо в Пітоні в тілі функції має бути хоч одна команда
````

Переробимо функцію пересування дракону:
````
def move_dragon():
    global draagon_x, dragon_y
    if can_see_hero():
        move_dragon_randomly()
    else:
        move_dragon_to_hero()
````

Переробимо функцію випадкового пересування:
````
import random # це ми кажемо що хочемо використовувати бібліотеку, в якій є генератор випадкових чисел
direction = random.choice(["left", "right", "up", down])
if (direction == "lfet"):
    dragon_x = dragon_x - 1
# схожим чином для інших напрямів
````

### 4. Заборонимо дракону проходити кріз перешкоди
Код де дракон повзе вліво переробляємо додавши додаткову умову за допомогою операції `and` (логічне І)
````
# цікаво, в чому тут помилка?
if (direction == 'left') and (labirynth_map[dragon_y][hero_x - 1] != '#'):
    dragon_x = dragon_x - 1
````

Тим же самим чином переробляємо для інших напрямів


### 5. Робимо щоб дракон рухався до героя якщо він бачить його
[Відяшка](https://www.youtube.com/watch?v=qDTuIHNQvFQ#t=1800)