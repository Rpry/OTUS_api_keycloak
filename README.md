Окружение для этого проекта запускает компоуз application.yml из папки Depoly

Для работы с приложением нужно:
1. Предварительные действия:
  - Убедиться, что у вас установлен postgres, если нет - раскоментитровать в application.yml блок постгреса и его волюм ниже
  - Убедиться что в postgres есть БД keycloak, если нет но есть postgres - создать (create database keycloak), если нет postgres - идти дальше, компоуз ее установит
  - Стянуть проект UI https://github.com/Rpry/OTUS_UI_keycloak (в любое место) 
2. Стянуть это приложение (OTUS_API_KEYCLOAK), найти application.yml, поменять ui_keycloak:build:context в соответствии с тем, где лежит стянутый вами проект UI из первого пункта
3. Запустить application-start.bat так чтобы он поднял все без ошибок
  
- Если во время выполнения пункта 3 будет ошибка UI_keycloak в консоли, значит возможно некорректно сделано действие 2
- Если во время выполнения пункта 3 будет ошибка самого keycloak в консоли, значит возможно проблема в несоответствии следующих параметров постгреса:
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/keycloak
      KC_DB_USERNAME: postgres
      KC_DB_PASSWORD: password 
4. Выполнить в БД keycloak скрипт update realm set ssl_required='NONE' where name = 'master';
Перезапустить контейнер
5. Проверка: Зайти в http://localhost:8080/admin/master/console/ ==> должно открыться окно логина
6. Зайти в keycloak http://localhost:8080/admin/master/console/, создать realm 'OTUS', в его настройках найти security-defenses и выставить в поле Content-Security-Policy следующее содержимое: 
frame-src http:; frame-ancestors http://localhost:3001; object-src http:;
7. Выполнить в БД keycloak скрипт update realm set ssl_required='NONE' where name = ‘OTUS';
Перезапустить контейнер
6. Создать в Realm OTUS нового клиента webapp, выставить у него следующие параметры:
![image](https://github.com/Rpry/OTUS_api_keycloak/assets/13750284/e2dc362b-0311-4797-ab5b-e8dc935d5fbd)
Обратите внимание на знак "плюс" в поле Web Origins. Опции Authentication flow оставить по умолчанию.
7. Создать в Realm OTUS нового юзера, указать пароль во вкладке Credentials. Запомнить логин/пароль для входа в приложение. В дальнейшем при первом логине keycloak может попросить дополнить данные или сменить пароль.
8. Проходим Client Scopes, открыть roles, там Mappers, далее realm roles и там переименовать token claim name из в realm_access.roles в role
9. Проверка:
   - Зайти на http://localhost:3001 => должна открыться страничка без ошибок
   - Залогиниться кредами нового юзера => логин должен пройти без ошибок
  
   
