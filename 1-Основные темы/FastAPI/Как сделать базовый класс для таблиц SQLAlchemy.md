#базы_данных #FastAPI #SQLAlchemy

```
from datetime import datetime  
  
from sqlalchemy import Integer, DateTime  
from sqlalchemy.orm import DeclarativeBase, declared_attr, Mapped, mapped_column
```
Импортируем все необходимые библиотеки, для базовой настройки этого класса достаточно будет `from sqlalchemy.orm import DeclarativeBase`, этой командой мы импортируем класс от которого мы от наследуем базовый класс.

```
class Base(DeclarativeBase):  
    __abstract__ = True
```
Создаем тот самый базовый класс, от которого мы будем наследовать наши SQLAlchemy модели и все библиотеки работающие с базой данных буду по одному этому классу через metadata собирать классы-наследники. Но чтобы сам этот класс не создавался в базе данных мы явно указываем что он является абстрактным.

```
@declared_attr.directive  
def __tablename__(cls):  
    name = cls.__name__  
    if name == 'Base':  
        return None  
    snake_case = ''.join(['_' + i.lower() if i.isupper() else i for i in name]).lstrip('_')  
    return snake_case + 's'
```
`@declared_attr.directive` - это магический декоратор SQLALchemy, который заставляет метод выполняться отдельно для каждого класса-наследника.  `__tablename__` это специальный атрибут SQLAlchemy в который записывается имя базы данных. Если мы не напишем декоратор, то мы в атрибут запишем функцию, так делать не надо, благодаря этому декоратору все работает правильно. Ну и дальше через ссылку на класс (cls это ссылка на класс, а не на объект), через поле `__name__ ` мы получаем название класса, видоизменяем его как душе угодно и возвращаем строку которая будет названием это таблицы в базе данных.

```
id: Mapped[int] = mapped_column(  
    Integer, primary_key=True, autoincrement=True  
)  
created_at: Mapped[datetime] = mapped_column(  
    DateTime, default=datetime.utcnow  
)  
updated_at: Mapped[datetime] = mapped_column(  
    DateTime, default=datetime.utcnow, onupdate=datetime.utcnow  
)
```
Тут объявляются поля таблицы, у каждой таблицы которая будет наследоваться от этого класса, будут эти поля, а конкретно:
- id - айдишник записи, ключевое поле, автозаполняемое
- created_at - время когда была сделана запись
- updated_at - время когда вносились изменения в эту запись
Последние два поля таблицы необходимы для администрирования базы данных.
