<em>Начиная с версии 14.0 GHost++ поддерживает передачу ограниченного числа данных карте которая поддерживает HCL стандарт.</em>
<p>Переданные данные используются в каждой карте по разному и в принципе не должны иметь одинаковые значения для разных карт.
Предполагается что карты будут использовать HCL систему чтобы позволить боту устанавливать моды или другую "стартовую" информацию которая не должна указываться игроками.
К примеру при автоматическом создании DotA игр желательно чтобы мод был установлен на боте а не указывался игроками.</p>

<ol>
<p>Теперь давайте разберём как это работает:</p>
<li> Найдите карту которая поддерживает HCL стандарт.
 <ol type="a"><li> На момент написания этого ни одна карта не поддерживает HCL стандарт хотя DotA будет поддерживать этот стандарт в будущем.</li></ol>
</li>
<li> Установите "HCL Коммандную строчку" (информация передаваемая карте) используя !hcl комманду в лобби игры.
<ol type="a">
 <li>  Коммандная строка HCL  может содержать только одно значение на игрока и/или компьютера, когда игра начинается.</li>
 <li> Она также может содержать ограниченный набор символов (буквы нижнего регистра, числа, и небольшое число спец символов).</li>
 <li> Для примера, с 4 игроками (или 3 игроками и  1 компьютером, и т.д.) Коммандная строка HCL может быть длинной в 4 символа.</li>
 <li> Коммандная строка HCL имеет разное значение для разных карт поэтому вам нужно узнать какие HCL данные для какой карты что значат прежде чем использовать.</li>
 <li> Используйте !clearhcl для очистки Коммандная строка HCL если вы указали данные и передумали.</li>
</ol>
</li>
<li> Начните игру.</li>
</ol>
<p>HCL система работает посредством информации хандикапа игрока таким образом что после загрузки карты они восстанавливаются.
Это означает что если вы попробуете установить HCL командную строку на карте не поддерживающей стандарт, бот радикально изменит хандикапы игроков.
В любом случае карта не поддерживающая стандарт не восстановит оригинальные хандикапы и игра будет испорчена.
Поэтому даже не пытайтесь установить HCL командную строку на карте которая не поддерживает HCL стандарт.</p>



<ol>
<p>Если вы хотите устанавливать HCL для каждой игры автоматически (к примеру когда используете автохостинг):</p>
 <li> Создайте фаил конфигурации карты для карты которую вы хотите использовать.</li>
 <li> Установите "map_defaulthcl = <чтонибудь>" в вашем файле конфигурации карты.</li>
 <li> Загрузите его используя !load комманду.</li>
 <li> Запустите автохостинг используя !autohost комманду. Бот будет использовать значение HCL указанное по умолчанию в вашем файле конфигурации карты.
<ol type="a"><li>Заметьте что данный метод установки HCL довольно неуклюж и может быть изменён в будущих версиях GHost++.</li></ol>
</li>
</ol>
<blockquote class="tip"><p><strong>Подсказка:</strong>Вы возможно захотите включить HCL систему в вашу карту, даже если вы не намерены использовать HCL Коммандную строку где либо.
Это объясняется тем что включение HCL системы защитит вашу карту от случайного нарушения хандикапов игроков установкой HCL Коммандной строки на не поддерживаемой карте.</p></blockquote>
```
    ///////////////////////////////////////////
    /// HostBot Command Library
    /// Last Modified: September 14, 2009
    /// Authors: Strilanc,
    /// v1.01
    ///////////////////////////////////////////
    /// Reads a command string transparently encoded into player handicaps by hostbots.
    /// Allows at most one character from "abcdefghijklmnopqrstuvwxyz0123456789 -=,." per player.
    /// Empty slots don't count towards the player count, but computers do.
    ///////////////////////////////////////////
    library HCL initializer init
        globals
            private string command = ""
        endglobals

        public function GetCommandString takes nothing returns string
            return command
        endfunction

        private function init takes nothing returns nothing
            local integer i
            local integer j
            local integer h
            local integer v
            local string chars = "abcdefghijklmnopqrstuvwxyz0123456789 -=,."
            local integer array map
            local boolean array blocked

            //precompute mapping [have to avoid invalid and normal handicaps]
            set blocked[0] = true
            set blocked[50] = true
            set blocked[60] = true
            set blocked[70] = true
            set blocked[80] = true
            set blocked[90] = true
            set blocked[100] = true
            set i = 0
            set j = 0
            loop
                if blocked[j] then
                    set j = j + 1
                endif
                exitwhen j >= 256
                set map[j] = i
                set i = i + 1
                set j = j + 1
            endloop

            //Extract command string from player handicaps
            set i = 0
            loop
                exitwhen i >= 12
                set h = R2I(100*GetPlayerHandicap(Player(i)) + 0.5)
                if not blocked[h] then
                    set h = map[h]
                    set v = h/6
                    set h = h-v*6
                    call SetPlayerHandicap(Player(i), 0.5 + h/10.0)
                    set command = command + SubString(chars, v, v+1)
                endif
                set i = i + 1
            endloop
        endfunction
    endlibrary
```