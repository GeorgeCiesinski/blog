---
layout: single
title: "Designing the Textblock model"
date: 2022-01-16 23:00:00 -0500
categories: Programming SimiDex Flask
comments: true
typora-root-url: ..
---

# Life update

Hello world. I have a new update about life stuffs. So all of my anxiety paid off with my house hunt. I was able to land an apartment, and the funny part is that I am moving into the building next to my current building. This said, I still can't shake the anxiety for some reason. I think I'm dreading the next time I have to find a place.

Besides that, i discovered the N64 game Paper Mario for the first time. This game came out in 2001 but I am completely hooked on it. For such an old mario game, I'm actually impressed how big the world is, and how many secrets there are. It also has the feel of an open world game without actually being open world.

Finally, I'm also not feeling sick any more. I had a fever for 7 straight days so I had me a worry! Luckily it ended up being the common flu and other than a persistent cough I'm feeling okay.

# What is a model

The model, or `db.model`, is a Flask-SQLAlchemy class that functions as a declarative base used to declare models. What is it a model of? It is a model of a SQL table.

To create a model, we create a new class that inherits from `db.model`. The columns of the table are also defined as `db.Column` with a few arguments. One of these is the data type, and there are also some optional ones such as whether this column is a primary key, unique, or nullable.

```python
class UserModel(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  username = db.Column(db.String(80), unique=True, nullable=False)
  password = db.Column(db.String(80), nullable=False)

  def __repr__(self):
    return `<User %r> % self.username
```

This model represents a template of an object representation of a row of the databse. Thanks to this, we can design utility methods that either save the objects to the database as a new row, or find objects in the database by filtering one of the columns and a certain value.

```python
def save_to_db(self):
  db.session.add(self)
  db.session.commit()

  @classmethod
	def find_by_shortcut(cls, shortcut):
		return cls.query.filter_by(username=shortcut).first()

	@classmethod
	def find_by_id(cls, _id):
		return cls.query.filter_by(id=_id).first()
```

This allows you to import the model in another module known as a resource. A resource is a class that inherits from the flask_restful `Resource` class. Once the model is imported, you can use the class methods defined above to search the database for a row, or post a new one to the database.

```python
from flask_restful import Resource
from webargs.flaskparser import use_kwargs
from api.models.user import UserModel

class UserRegister(Resource):

	@use_kwargs({
		"username": fields.String(required=True),
		"password": fields.String(required=True)
	},
		location='json')
	def post(self, **kwargs):

		if UserModel.find_by_username(kwargs['username']):
			return {"message": "A user with that username already exists"}, 400

		user = UserModel(**kwargs)
		user.save_to_db()

		return {"message": "User created successfully."}, 201
```

In the above example, we are using the `UserModel.find_by_username()` class method to look for a User. Later in the example, we create a new user object by simply doing `user = UserModel(**kwargs)`. Once it is created, you can simply call `user.save_to_db()` and that created object is then committed to the database.

This is the power of Flask-SQLAlchemy. 
