#settings #настройки #pydantic #FastAPI
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
    

	class Config:  
	    env_file = ".env"  
	    env_file_encoding = "utf-8"  
	    case_sensitive = True
```
Наследуемся от базового класса BaseSettings, прописываем все поля которые хотим получить из .env файла и в Config мы записываем информацию про .env файл

```
settings = Settings()
```
Потом для удобства взаимодействия создаем объект настроек который будем в дальнейшем импортировать по программе

```
@property  
def DATABASE_URL(self) -> str:  
    return (f"postgresql+asyncpg://{self.POSTGRES_USER}:{self.POSTGRES_PASSWORD}@"  
            f"{self.POSTGRES_HOST}:{self.POSTGRES_PORT}/{self.POSTGRES_DB}")  
  
@property  
def SYNC_DATABASE_URL(self) -> str:  
    return (f"postgresql://{self.POSTGRES_USER}:{self.POSTGRES_PASSWORD}@"  
            f"{self.POSTGRES_HOST}:{self.POSTGRES_PORT}/{self.POSTGRES_DB}")
```
Также можем в классе прописать propery поля для того чтобы генерировалась ссылка на подключение