Начиная с версии 10.0, GHost++ может присоединяться к нескольким серверам одновременно.
Итак Как?:

<ol>
 <li>1.) Когда GHost++ запускается он считывает с файла конфигурации 9 наборов значений для подключения к Battle.net серверам.</li>
 <li>2.) Набор значения для соединения с Battle.net сервером содержит:<br/>
<ol>
 <li>  a.) *_server (<strong><em>обязательно</em></strong>)</li>
 <li>  b.) *_serveralias</li>
 <li>  c.) *_cdkeyroc (<strong><em>обязательно</em></strong>)</li>
 <li>  d.) *_cdkeytft (<strong><em>обязательно</em></strong>)</li>
 <li>  e.) *_username (<strong><em>обязательно</em></strong>)</li>
 <li>  f.) *_password (<strong><em>обязательно</em></strong>)</li>
 <li>  g.) *_firstchannel</li>
 <li>  h.) *_rootadmin</li>
 <li>  i.) *_commandtrigger</li>
 <li>  j.) *_custom_war3version</li>
 <li>  k.) *_custom_exeversion</li>
 <li>  l.) *_custom_exeversionhash</li>
 <li>  m.) *_custom_passwordhashtype</li>
 <li>  n.) *_custom_countryabbrev</li>
 <li>  o.) *_custom_country</li>
</ol>
</li>
 <li>3.) GHost++ будет искать информацию для соединения  с  Battle.net сервером , заменяя "*" на "bnet_" потом "bnet2_" после "bnet3_" и так далее до "bnet9_".
<div class="note"> GHost++ не ищет значений для "bnet1_" для обратной совместимости</div></li>
 <li>4.) Если GHost++ не найдёт *_server ключ он прекратит поиск информации для подключения к Battle.net серверу в текущем наборе значений.</li>
 <li>5.) Если обязательный ключ не задан GHost++ пропустит наборов значений и приступит к следующему.</li>
</ol>