
Declarative Serializer
====
Serialize SQLAlchemy models into json-api compliant objects.

Example
-------

Given the following SQLAlchemy model:
```
class Person(declarative_base()):
    """Simple person model."""
    __tablename__ = 'people'
    id = Column(Integer, primary_key=True)
    first_name = Column(String(50))
    username = Column(String(50), unique=True, nullable=False)
    password = Column(String(50), nullable=False)
```
You can define a serializer describing how to serialize the SQLAlchemy model:
```
class PersonSerializer(serializer.Serializer):
    fields = ['id', 'first_name', 'username']  # Attributes we want serialized.
    model = Person  # The model mapping to the serializer.
    dasherize = True  # Specify model names such as first_name to be serialized as 'first-name'
```
How you can use your newly defined serializer:
```
# Some GET view in a web framework such as Flask
def get_resource(db_id):
    # Fetch some resource from database.
    some_sqlalchemy_model_resource = db.query(Person).get(db_id)
    person_serializer = PersonSerializer()
    data = person_serializer.serialize(some_sqlalchemy_model_resource)
    return flask.jsonify(data), status_code
```