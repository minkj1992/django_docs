# FBV vs CBV
> [link](http://jangwon.io/django/study/2018/03/01/(Django)-FBV%EC%99%80-CBV%EC%9D%98-%EC%A0%95%EC%9D%98/)

- Function Base View (FBV - 함수 기반 뷰)
  - 함수로 구현한 뷰
- Class Base View (CBV - 클래스 기반 뷰)
  - 클래스로 함수를 구현한 뷰


## FBV
```python
from django.shortcuts import get_object_or_404, render
def person_detail(request, id):
    person = get_object_or_404(Person, id=id)
    return render(request, 'person/person_detail.html',{
        'person': person,
    })

person_detail(4)
```

## CBV
> 재사용을 위해서 사용하는 일종의 템플릿 (DRY principle)

```python
class DetailView:
    @classmethod
    def as_view(cls, model):
        def view_fn(request, id):
            instance = get_object_or_404(model, id=id)
            instance_name = model._meta.model_name
            app_name = model._meta.app_label
            template_name = '{}/{}_detail.html'.format(app_name, instance_name)
            return render(request, template_name, {
                instance_name: instance_name,
            })
        return view_fn

DetailView.as_view(Person, 4)
```

### CBV's Method type (3)

- instance method : instance를 통한 호출. 첫번째 인자로 instance가 자동 지정 (self에 대입)

- class method : class를 통한 호출. 첫번째 인자로 Class가 자동 지정 (cls에 대입)

- static method : class를 통한 호출. 자동으로 지정되는 인자가 없음. 활용은 class method와 동일.

```python
import random


class Person:
    def __init__(self, name, gender, country):
        self.name = name
        self.gender = gender
        self.country = country
    
    # instance method
    def say_hello(self):
        return f'Hi this is {self.name}[{self.gender}] from {self.country}'
    
    # class method
    @classmethod
    def new_boy(cls, name, country):
        return Person(name, "boy", country)
    
    @classmethod
    def new_girl(cls, name, country):
        return Person(name, "girl", country)
    
    # static method
    @staticmethod
    def random_name(gender, country, level = 6):
        random_name = ''.join(random.choice(gender+country) for i in range(level))
        return Person(random_name, gender, country)
```

