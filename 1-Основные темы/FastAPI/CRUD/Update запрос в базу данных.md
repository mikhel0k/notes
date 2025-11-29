#базы_данных #SQLAlchemy #Update
Запрос на обновление записи в базе данных делается следующим образом:
Объявляется асинхронная функция в которую передаются аргументы: pydantic схема для обновления записи в базе данных, в ней не должен упоминаться id и будет передаваться информация которую надо обновить в конкретной записи, id самой записи или любое другое поле по которому будет вестись поиск, а также объект AsyncSession который будет являться сессией в базе данных.
```
async def update_author(  
        author_id: int,  
        author_update: AuthorUpdate,  
        session: AsyncSession  
):
```

Дальше необходимо получить запись из базы данных которая будет обновляться:
```
author = await session.get(Author, author_id)
```

После чего необходимо распаковать информацию из pydantic схемы которую мы получили в функцию и внести изменения в существующую запись через setattr()
```
updated_data = author_update.model_dump(exclude_unset=True)  
for key, value in updated_data.items():  
    setattr(author, key, value)
```

Ну и дальше необходимо сохранить все изменения в базу данных, обновить информацию в переменной и вернуть ее из функции
```
await session.commit()  
await session.refresh(author)  
return AuthorResponse.model_validate(author)
```