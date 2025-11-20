При помощи объекта FastAPI создается приложение в которое потом добавляются endpoints 

```
from fastapi import FastAPI

app = FastAPI()
```

И в дальнейшем можно в этом приложении создавать ручки как описано [[Написание endpointов]]

Но когда-то надо будет и ручки делить по файлам тогда просто объявим APIRouter который потом подключим в основное приложение

```
from fastapi import APIRouter  

router = APIRouter(prefix="/authors", tags=["authors"])
```

Потом просто импортируешь этот роутер 
```
from authors import router

app.include_router(touter)
```
