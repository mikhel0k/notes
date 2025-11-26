
```
import os  
from pathlib import Path   
from pydantic_settings import BaseSettings  
  
BASE_DIR = Path(__file__).parent.parent  
  
  
class Settings(BaseSettings):    
    JWT_PRIVATE_KEY: Path = BASE_DIR / "pem_keys" / "jwt-private.pem"  
    JWT_PUBLIC_KEY: Path = BASE_DIR / "pem_keys" / "jwt-public.pem"  
    ALGORITHM: str = "RS256"  
```

Необходимо использовать правильно объект Path чтобы потом в коде при обращении к этой переменной можно было вызывать функцию `.read_text()`.
Чтобы правильно все сделать необходимо в переменную BASE_DIR передать объект Path и перейти в родительскую директорию, также в настройках обязательно аннотировать поля как Path. Также для удобства работы и соблюдения принципов полиморфизма и DRY стоит создавать поле в котором будет указываться алгоритм шифрования.