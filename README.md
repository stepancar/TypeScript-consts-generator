# TypeScript-consts-generator


Если в вашем TypeScript приложении множество констант, которые должны иметь разные значения в зависимости от конфигурации сборки, это решение облегчит вам жизнь.
Почему?
 Плохое решение:
 Определять контсанту в конкретном участке кода:
 main1.ts
 ``` javascript 
  var sellerEmail = "";
  if (configuration === "Debug"){
      sellerEmail = "myDebugSeller@mail.com"
  } else if (configuration === "Test"){
      sellerEmail = "myTestSeller@mail.com"
  } else {
      sellerEmail = "seller@mail.com"
  }
  sendMessage(sellerEmail, "Hello");
 ```
 main2.ts 
 ``` javascript
  var sellerEmail = "";
  if (configuration === "Debug"){
      sellerEmail = "myDebugSeller@mail.com"
  } else if (configuration === "Test"){
      sellerEmail = "myTestSeller@mail.com"
  } else {
      sellerEmail = "seller@mail.com"
  }
  sendMessage(sellerEmail, "Hello, my friend, i wann buy your TV");
 ```
 Проблемы такого подхода
 ```
 1) Дублирование кода (если придется поменять тестовую почту - придется менять её в нескольких местах)
 2) Всем доступны константы в разных конфигурациях. Могут подглянуть какой почтой, к примеру, вы пользуетесь.
 3) Лишние проверки
 ```
 Очевидное решение первой проблемы - Вынести константы в отдельный файл.
 Стандартное решение проблемы с константами в typescript
 constants.ts:
``` javascript
  var sellerEmail = "";
  if (configuration === "Debug"){
      sellerEmail = "myDebugSeller@mail.com"
  } else if (configuration === "Test"){
      sellerEmail = "myTestSeller@mail.com"
  } else {
      sellerEmail = "seller@mail.com"
  }
  
  export const consts = {
      sellerEmail: sellerEmail
  } 
  ...
  
```
main.ts:
``` javascript
  import {consts} from 'path/to/consts'
  sendMessage(consts.sellerEmail);
```
Уже лучше, но проблемы
```
2) Всем доступны константы в разных конфигурациях. Могут подглянуть какой почтой, к примеру, вы пользуетесь.
3) Лишние проверки
```
все ещё актуальны.
Третья проблема имеет малое значение, потому что число проверок во всем файле конфигураций - это всего лишь количество поддерживаемых конфигураций - обычно 2, 3.

Если мы не хотим чтобы кто-то видел наши константы - давайте перед компиляцией просто подставим нужные нам значения.
Где хранить константы?
Будем хранить их в файле конфигурации. Обычно для файлов конфигурации используют XML или JSON форматы.
В нашем случае больше подходит JSON, так как он боллее близкий к JS/TS.
Сразу оговорим следующее:
```
1) Нужно предоставить возможность группировать константы
2) Предоставить возможность дать комментарий константе или группе констант
```
Пример конфига
``` javascript
  {
     "consts": {
        "comments": "Here you can find your application constants",
        "emails": {
            "comments": "emails for different configurations",
            "sellerEmail": {
                "comments": "seller email, if you testing your app don't use original email!!!",
                "release": "seller@mail.com",
                "debug": "myDebugSeller@mail.com",
                "test": "myTestSeller@mail.com"
            }
        },
        "apiKeys": {
            "comments": "api keys for different configurations",
            "googleMapsApiKey": {
                "comments": "Use another key for testing you google maps, because its very expensive",
                "release": "asd!22asdjk_eweqwerqw",
                "debug": "werwqj!2324324",
                "test": "rwer334225_3423n"
            }
        }
    }
  }
```

![try](https://github.com/stepancar/TypeScript-consts-generator/blob/master/docs/images/screen.png?raw=true)
 
  
