#pydantic #Python #базы_данных #аннотирование
Чтобы создать pydantic схему надо создать класс наследник от `pydantic.BaseModel`
```
from pydantic import BaseModel

class BaseSchema(BaseModel):
```

Дальше прописываем все поля, аннотируем их как обычные поля класса. Также присваиваем туда
```
Field(  
    ...,  
    и какие то параметры, например min_lenght, max_lenght, description  
)
```

Только вот тут надо понимать что должны быть разные схемы, должна быть схема для ответа при поиске объекта, должна быть схема для добавления или изменения объекта, должна быть схем также для запроса информации с какими-то зависимостями.
Пример:
```
from datetime import date, datetime  
  
from pydantic import BaseModel, Field, ConfigDict  
  
  
class BookBase(BaseModel):  
    title: str = Field(  
        ...,  
        min_length=1,  
        max_length=128,  
        description="The title of the book"  
    )  
    isbn: str = Field(  
        ...,  
        min_length=10,  
        max_length=13,  
        pattern=r"^\d{10,13}$",  
        description="ISBN of the book"  
    )  
    publication_year: date = Field(  
        ...,  
        description="year of the book publication",  
    )  
  
  
class BookCreate(BookBase):  
    pass  


class class BookUpdate(BaseModel):  
    title: str | None = Field(  
        ...,  
        min_length=1,  
        max_length=128,  
        description="The title of the book"  
    )  
    isbn: str | None = Field(  
        ...,  
        min_length=10,  
        max_length=13,  
        pattern=r"^\d{10,13}$",  
        description="ISBN of the book"  
    )  
    publication_year: date | None = Field(  
        ...,  
        description="year of the book publication",  
    )  
  
  
class BookResponse(BookBase):  
    model_config = ConfigDict(from_attributes=True)  
  
    id: int  
    created_at: datetime  
    updated_at: datetime
```

Это те схемы которые прописываются чаще всего, конечно для книг обновление информации не актуально, но для примера пойдет.