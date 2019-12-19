Документация на API
========================

API является экспериментальным.
Пожалуйста, уведомите support@joinrpg.ru, если вы используйте его, в противном случае мы не будем испытывать угрызений совести, если внезапно сломаем что-то.

`API задокументировано здесь. <https://dev.joinrpg.ru/swagger/ui/index>`_

Аутентификация
-----------------------------
::

    POST https://dev.joinrpg.ru/x-api/token
    Content-type: application/x-www-form-urlencoded

**Тело запроса:** ::

    grant_type=password&username=<percent encoded email>&password=<percent encoded password>
    
**Ответ:** ::

    {
    "access_token": "<very long token string>",
    "token_type": "bearer",
    "expires_in": 2591999
    }

Теперь ко всем запросам надо добавлять заголовок: ::

    Authorization: Bearer <very long token string>

Если заголовка не будет или токен неверный или истек, будет 401 Unauthorized

Токен действует ограниченное время (expires_in — время жизни токена в секундах) и может внезапно истечь вне всякой связи с вашими действиями. 

Все запросы доступны только людям с мастерскими правами!
