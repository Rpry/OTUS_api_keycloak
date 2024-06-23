Окружение для этого проекта запускает компоуз application.yml из папки Depoly

Для работы с приложением нужно:
1. Убедиться, что у вас установлена БД постгрес, если нет - раскоментитровать в application.yml блок постгреса и его волюм ниже
2. Стянуть проект UI https://github.com/Rpry/OTUS_UI_keycloak
3. Найти application.yml, поменять ui_keycloak:build:context в соответствии с тем, где лежит стянутый вами проект UI из первого пункта
4. Запустить application-start.bat так чтобы он поднял все без ошибок
  
Если во время выполнения пункта 4 будет ошибка UI_keycloak, значит возможно некорректно сделано действие 3

Если во время выполнения пункта 4 будет ошибка самого keycloak, значит возможно проблема в несоответствии следующих параметров постгреса:
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/postgres
      KC_DB_USERNAME: postgres
      KC_DB_PASSWORD: password

5. Зайти в keycloak http://localhost:8080/, создать realm 'OTUS', в его настройках найти security-defenses и выставить в поле Content-Security-Policy следующее содердимое: 
frame-src http:; frame-ancestors http://localhost:3001; object-src http:;
6. Создать в Realm OTUS нового клиента webapp, выставить у него следующие параметры:
![image](https://github.com/Rpry/OTUS_api_keycloak/assets/13750284/e2dc362b-0311-4797-ab5b-e8dc935d5fbd)
Обратите внимание на знак "плюс" в поле Web Origins. Опции Authentication flow оставить по умолчанию.
7. Создать в Realm OTUS нового юзера, указать пароль во вкладке Credentials. Запомнить логин/пароль для входа в приложение. В дальнейшем при первом логине keycloak может попросить дополнить данные или сменить пароль.
   
