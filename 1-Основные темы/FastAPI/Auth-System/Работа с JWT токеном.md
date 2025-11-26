
Чтобы работать с JWT токенами нужно использовать библиотеку jwt
```
import jwt
```

Сам токен создается при помощи функции, в которую передаем, словарь со значениями которые шифруются, приватный ключ и алгоритм
```
jwt.encode(data_dict, settings.JWT_PRIVATE_KEY.read_text(), algorithm=settings.ALGORITHM)
```

Чтобы дэшифровать используется уже публичный ключ
```
jwt.decode(token, settings.JWT_PUBLIC_KEY.read_text(), algorithm=settings.ALGORITHM)
```

Чтобы правильно работать с созданными файлами ознакомьтесь [[Настройка settings для использования токенов сохраненных в файлах]]