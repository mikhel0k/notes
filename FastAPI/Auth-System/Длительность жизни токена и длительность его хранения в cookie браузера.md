#JWT_токен #авторизация #регистрация #cookie #длительность_жизни_токена
Существует два разных понятия, это: длительность жизни jwt токен и длительность его хранения в браузере. 

Как понятно из названия длительность хранения в браузере - то значение времени после которого браузер автоматически сотрет данную cookie.
```
response.set_cookie(  
    key="access_token",  
    value=access_token,  
    httponly=False,  
    secure=False,  
    samesite="none", 
    max_age=settings.ACCESS_TOKEN_EXPIRE_MINUTES * 60,  # <-
    path="/",  
    domain="localhost" 
)
```
Вот, при установке cookie задается переменная max_age в секундах которая и задает длительно хранения в браузере.

Длительность жизни токена это уже координально другое значение значение которое проверяется уже сервером когда к нему пришел токен, если время вышло то токен недействителен.
```
expire = datetime.now(timezone.utc) + timedelta(minutes=settings.ACCESS_TOKEN_EXPIRE_MINUTES)  
data_dict["exp"] = expire
```
Чтобы ее задать надо просто в шифруемый словарь передать в поле exp значение datetime.
