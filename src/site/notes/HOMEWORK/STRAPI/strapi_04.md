---
{"dg-publish":true,"permalink":"/homework/strapi/strapi-04/"}
---

# Реализация проекта, созданного в STRAPI через FastApi

## Установка FastApi и Uvicorn
![Снимок экрана 2024-10-07 в 22.22.10.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-10-07%20%D0%B2%2022.22.10.png)
Для начала я создал внутреннюю оболочку, установил необходимые пакеты, а именно fastapi uvicorn. Создал простой python файл с целью убедиться, что я могу запускать код и смотреть через 127.0.0.1 port 8000.
![Снимок экрана 2024-10-07 в 22.23.57.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-10-07%20%D0%B2%2022.23.57.png)
С помощью команды Unicorn 12:app --reload запустил, все заработало![Снимок экрана 2024-10-07 в 22.23.23.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-10-07%20%D0%B2%2022.23.23.png)

## Создание и реализация первой сущности User1 из проекта Strapi 

Прежде всего для реализации сущности я создал папку my_fastapi_project, куда добавил следующие питоновские файлы: 
![Pasted image 20241007231408.png](/img/user/Pasted%20image%2020241007231408.png)

Разбор начнем с основного кода : app.py

### APP.PY
Главный файл, который инициализирует FastAPI и содержит основные эндпоинты 

 ``` python 
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
from database import get_db
from crud import create_user, get_user
from schemas import UserCreate, User

app = FastAPI()

@app.post("/users/", response_model=User)
def create_user_endpoint(user: UserCreate, db: Session = Depends(get_db)):
    return create_user(db, user)

@app.get("/users/{user_id}", response_model=User)
def get_user_endpoint(user_id: int, db: Session = Depends(get_db)):
    db_user = get_user(db, user_id)
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return db_user
     ```

### MODELS.PY
Определяет SQLAlchemy модели (например, модель `User1`)
``` python
from sqlalchemy import Column, String, Integer, ForeignKey, Table
from sqlalchemy.orm import relationship
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

user_group_table = Table(
    'user_group', Base.metadata,
    Column('user_id', Integer, ForeignKey('users.id')),
    Column('group_id', Integer, ForeignKey('groups.id'))
)

class User1(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True, index=True)
    username = Column(String, unique=True, nullable=False)
    email = Column(String, unique=True, nullable=False)
    password = Column(String, nullable=False)
    user_id = Column(String, unique=True, nullable=False)

    comics = relationship('Comic', back_populates='user_1')
    comments = relationship('Comment', back_populates='sender')
    ratings = relationship('Rating', back_populates='user')
    groups = relationship('Group', secondary=user_group_table, back_populates='users')

class Comic(Base):
    __tablename__ = 'comics'
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, nullable=False)
    user_1_id = Column(Integer, ForeignKey('users.id'))
    user_1 = relationship('User1', back_populates='comics')

class Comment(Base):
    __tablename__ = 'comments'
    id = Column(Integer, primary_key=True, index=True)
    content = Column(String, nullable=False)
    sender_id = Column(Integer, ForeignKey('users.id'))
    sender = relationship('User1', back_populates='comments')

class Rating(Base):
    __tablename__ = 'ratings'
    id = Column(Integer, primary_key=True, index=True)
    score = Column(Integer, nullable=False)
    user_id = Column(Integer, ForeignKey('users.id'))
    user = relationship('User1', back_populates='ratings')

class Group(Base):
    __tablename__ = 'groups'
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, nullable=False)
    users = relationship('User1', secondary=user_group_table, back_populates='groups')
    ```

### SCHEMAS.PY
Содержит Pydantic схемы для валидации и сериализации данных.
```python 
from pydantic import BaseModel, EmailStr
from typing import List, Optional

class UserBase(BaseModel):
    username: str
    email: EmailStr

class UserCreate(UserBase):
    password: str

class User(UserBase):
    id: int
    user_id: str
    comics: List[str] = []
    comments: List[str] = []
    ratings: List[int] = []

    class Config:
        orm_mode = True
```

### DATABASE.PY
Устанавливает соединение с базой данных и создает сессию для взаимодействия с БД.
``` python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./test.db"  # Используйте свою базу данных

engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### CRUD.PY
Содержит функции для создания, чтения, обновления и удаления записей в БД.
``` python
from sqlalchemy.orm import Session
from models import User1
from schemas import UserCreate

def create_user(db: Session, user: UserCreate):
    db_user = User1(username=user.username, email=user.email, password=user.password)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

def get_user(db: Session, user_id: int):
    return db.query(User1).filter(User1.id == user_id).first()
```
## Реализация и проверка
Соответственно, запускаем код уже известной командой  uvicorn app:app --reload  и переходим по 127.0.0.1:8000 , попадаем в ошибку, но нам, конечно же интереснее 127.0.0.1/docs
![Pasted image 20241007232429.png](/img/user/Pasted%20image%2020241007232429.png)

Подключаем PostMan и реализуем создание пользователя

![Снимок экрана 2024-10-07 в 23.25.34.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-10-07%20%D0%B2%2023.25.34.png)
Пользователь создался, смотрим через запрос GET 
![Снимок экрана 2024-10-07 в 23.25.45.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-10-07%20%D0%B2%2023.25.45.png)
Видим, что все прошло успешно