<h1>Імітаційне моделювання комп'ютерних систем</h1>

<h2>Лабораторна робота №2. Редагування імітаційних моделей у середовищі NetLogo</h2>

<h3>СПм-21-2, Понамарьов Владислав Олександрович</h3>

<h2>Модель:
<a href="http://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/Curricular%20Models/BEAGLE%20Evolution/Red%20Queen.nlogo">
Red Queen
</a>
</h2>

<h3>Внесені зміни до вихідної логіки моделі:</h3>

Збільшено опір змії до отрути при успішному поїданні жаби. - Для цього в процедуру полювання було внесено зміни коли при
успішному поїданні жаби опір змії збільшується на десяту частину початкового опору.

<pre>
set resistance resistance + (initial-resistance-mean / 10) 
</pre>

Зменшено отруйність жаби після невдалої (змії) спроби її з'їсти. Отруйність відновлюється до вихідної зі зміною тактів
модельного часу. - Для цього в процедуру полювання було внесено зміни коли при невдалому поїданні жаби її отруйність
зменшується на десяту частину початкового значення додано значення для збереження кількості отрути та кількості циклів.

<pre>
ask hunted [ 
    set poison poison - (initial-poison-mean / 10)
    set savePoison poison - (initial-poison-mean / 10)
    set cyclePoison 1
]
</pre>

Також додана процедура повернення отрути жабі.

<pre>
to change-poison
  if cyclePoison = 1 [
    if poison > savePoison [
      set cyclePoison 0
    ]
    
    if poison != savePoison [
      set poison poison + (initial-poison-mean / 100)
    ]
  ]
end
</pre>

Після успішного з'їдання отруйної жаби, змія не рухається деяке число тактів модельного часу, залежно від співвідношення
отрути з'їденої жаби та своєї опірності на момент поїдання. - Для цього в процедуру полювання було внесено зміни коли
при успішному поїданні жаби обчислює певну кількість тактів і записується в зазначені змінні.

<pre>
    set saveResistance resistance / [ poison ] of hunted
      set saveResistanceCount resistance / [ poison ] of hunted
      set cycleResistance 1    
</pre>

Також додано процедуру, яка визначає рух змії.

<pre>
to snake-position
  if cycleResistance = 1 [
    set saveResistance saveResistance + 1
    
    if saveResistance > saveResistanceCount [
      set cycleResistance 0
    ]
    
    fd 0
  ] 
  
  if cycleResistance = 0 [
    fd 1
  ]
end
</pre>

