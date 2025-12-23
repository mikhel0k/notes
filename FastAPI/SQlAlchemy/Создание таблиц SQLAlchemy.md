  #SQLAlchemy #базы_данных #postgresql #модели_базы_данных
```
class Book(BaseModel):
```
или 
```
class Book(DeclarativeBase):
```
Тут мы наследуем все наши классы от какого-то конкретного общего класса который является классом SQLAlchemy, рекомендуется использовать вариант с созданием своего базового класса [[Как сделать базовый класс для таблиц SQLAlchemy]]

Потом прописываются любые поля вашей таблицы, например:
```  
id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)
title: Mapped[str] = mapped_column(String(128), nullable=False)  
description: Mapped[str | None] = mapped_column(Text(), nullable=True)  
publication_year: Mapped[int] = mapped_column(Integer, nullable=False)  
genre: Mapped[str] = mapped_column(String(128), nullable=False)
```
Сначала мы пишем название поля, потом аннотируем его для Python при помощи Mapped и стандартных типов данных в Python. Потом присваиваем туда функцию mapped_column, это уже какие-то нюансы этого поля в SQL базе данных, то есть прописываем тип данных для SQL, прописываем уникальное ли оно, автоинкриментируемое, может ли быть Nul, ключевое ли это поле(ForeignKey), связующее ли с другой таблицей(ForeignKey), значения по умолчанию и т.д..
В случае если вы сделали внешний ключ то необходимо SQLAlchemy явно рассказать что он существует [[relationship в SQLAlchemy или отношения баз данных]]

```
def __str__(self):  
    return (f"Title: {self.title} of {self.publication_year} year,"            f"Genre: {self.genre}")  
  
def __repr__(self):  
    return str(self)
```
Также в качестве рекомендации стоит переопределять данные магические методы для удобства отладки в дальнейшем

Чтобы применить изменения в базе данных стоит использовать alembic [[alembic, Миграции баз данных]]