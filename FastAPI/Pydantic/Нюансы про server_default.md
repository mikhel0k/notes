#миграции #server_default
При автоматическом создании миграций [[alembic, Миграции баз данных]] могут возникать ошибки, например. Миграции автоматически не делают поля со значениями server default, такие значения нужно прописывать явно в коде самой миграции, например

```
op.alter_column('books', 'created_at', server_default=sa.text('NOW()'))  
op.alter_column('books', 'updated_at', server_default=sa.text('NOW()'))
```
