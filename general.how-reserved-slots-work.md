Каждая игра имеет свой список зарезервированных игроков.
Список очищается после того, как стартует игра.
Вы можете добавлять в лист игроков, используя команду !hold.
Когда игрок пытается присоединиться в игру, Бот считает игрока зарезервированным если:

<ol>
 <li>Найдёт игрока в списке.</li>
 <li>Если игрок является Главным Администратором на любом из определённых серверов.</li>
 <li>Если игрок является Администратором на любом из определённых серверов.</li>
 <li>Если игрок Владелец игры.</li>
</ol>

<div class="note">Вам не нужно проходить проверку spoofcheck чтобы стать зарезервированным игроком.</div>

Если игрок зарезервирован, ему будет выделятся место по следующим предпочтениям:
<ol>
 <li>Если есть открытый слот.</li>
 <li>Если есть закрытый слот.</li>
 <li>Если слот занят не зарезервированным игроком, он займёт его место.</li>
</ol>

Дополнительно, если игрок является Владельцем игры, ему будет выделятся место по следующим предпочтениям:

<ol>
 <li>Игрок с самым низким номером слота будет выкинут и владелец игры займёт его место</li>
 <li>Компьютер в слоте 0 (первый слот) будет выкинут и владелец игры присоединиться к игре.</li>
</ol>