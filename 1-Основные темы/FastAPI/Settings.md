При помощи библиотеки `pydantic_settings` можно считывать информацию из .env файла и работать с ней в дальнейшем как с классом

```
import os  
from pydantic_settings import BaseSettings, SettingsConfigDict
```
Делаем все импорты

```
class Settings(BaseSettings):  
    DB_HOST: str  
    DB_PORT: int  
    DB_NAME: str  
    DB_USER: str  
    DB_PASSWORD: str  
    model_config = SettingsConfigDict(  
        env_file=os.path.join(os.path.dirname(os.path.abspath(__file__)), "..", ".env")  
    )
```
Наследуемся от базового класса BaseSettings, прописываем все поля которые хотим получить из .env файла и в model_config мы записываем путь к .env файлу

```
settings = Settings()
```
Потом для удобства взаимодействия создаем объект настроек который будем в дальнейшем импортировать по программе

```
def get_db_url():  
    return (f"postgresql+asyncpg://{settings.DB_USER}:{settings.DB_PASSWORD}@"  
            f"{settings.DB_HOST}:{settings.DB_PORT}/{settings.DB_NAME}")
```
Также можем написать функцию, которая будет автоматически генерировать строку для выполнения подключения к базе данных