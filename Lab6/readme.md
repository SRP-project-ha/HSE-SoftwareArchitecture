# Лабораторная работа № 6 - Шаблоны проектирования #


## Структурные шаблоны ##
### Адаптер / Adapter ### 
Зачем нужен метод: используется для преобразования интерфейса одного класса в интерфейс, ожидаемый клиентом, чтобы классы с несовместимыми интерфейсами могли работать вместе.\
Пример: адаптируем метод sleep_outside() класса Homeless к методу provide_shelter() класса Shelter через адаптер ShelterAdapter, преобразуя входные данные метода sleep_outside() в параметры метода provide_shelter().
![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/e127278c-39dd-4401-844b-e1984e8b2970)


```
def convertToServiceMethod(children):
    return children / 2

class Homeless:
    def sleep_outside(self, children):
        print("Sleeping outside...")

class Shelter:
    def provide_shelter(self, num_people):
        print(f"Providing shelter for {num_people} people...")

class ShelterAdapter:
    def __init__(self, homeless):
        self.homeless = homeless

    def provide_shelter(self, children):
        num_people = convertToServiceMethod(children)
        self.homeless.sleep_outside(num_people)

```
### Заместитель / Proxy ### 
Зачем нужен метод: позволяет подставлять вместо реальных объектов специальные объекты-заменители. Эти объекты перехватывают вызовы к оригинальному объекту, позволяя сделать что-то до или после передачи вызова оригиналу.\
Пример: есть абстрактный класс Homeless, который определяет метод find_food(). Затем есть класс RealHomeless, который реализует этот метод, и класс Proxy, который является заместителем для RealHomeless. Клиентский код создает экземпляр RealHomeless, а затем передает его в конструктор Proxy, чтобы использовать его в заместителе.

![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/4e2b0b95-09cb-4f7d-b70d-61d3cfa3d8ca)

```
class Homeless:
    def find_food(self):
        pass

class RealHomeless(Homeless):
    def find_food(self):
        print("Ищем еду...")

class Proxy(Homeless):
    def __init__(self, real_homeless):
        self.real_homeless = real_homeless
    
    def find_food(self):
        print("Проверяем доступность еды...")
        self.real_homeless.find_food()

real_homeless = RealHomeless()
proxy_homeless = Proxy(real_homeless)
proxy_homeless.find_food()

```

### Компоновщик / Composite ### 
Зачем нужен метод: позволяет сгруппировать множество объектов в древовидную структуру, а затем работать с ней так, как будто это единичный объект.\
Пример: есть компоненты Homeless и Shelter, где Homeless представляет бездомного человека, а Shelter представляет приют. Компонент Shelter может содержать другие компоненты, в данном случае объекты Homeless. Когда вызывается метод survive() для приюта, он также вызывает метод survive() для каждого компонента внутри себя.

![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/6d183241-09c3-42bd-88e4-6674c3c3f025)

```
class Component:
    def survive(self):
        pass

class Homeless(Component):
    def __init__(self, name):
        self.name = name

    def survive(self):
        print(f"{self.name}: Ищу еду и убежище...")

class Shelter(Component):
    def __init__(self):
        self.components = []

    def add_component(self, component):
        self.components.append(component)

    def remove_component(self, component):
        self.components.remove(component)

    def survive(self):
        print("В приюте:")
        for component in self.components:
            component.survive()

if __name__ == "__main__":
    # Создаем бездомных людей
    homeless1 = Homeless("Иван")
    homeless2 = Homeless("Мария")

    shelter = Shelter()

    shelter.add_component(homeless1)
    shelter.add_component(homeless2)

    shelter.survive()

```
### Декоратор / Decorator ### 
Зачем нужен метод:  позволяет динамически добавлять объектам новую функциональность, оборачивая их в полезные «обёртки».\
![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/547eb3d4-7f14-4fcb-8666-92324cfba2b0)


```
class Homeless:
    def survive(self):
        pass

class HomelessDecorator(Homeless):
    def __init__(self, homeless):
        self._homeless = homeless

    def survive(self):
        return self._homeless.survive()

class BaseHomeless(Homeless):
    def survive(self):
        return "Выживаю на улице"

class FoodDecorator(HomelessDecorator):
    def survive(self):
        return f"{self._homeless.survive()}, нахожу еду"

class ShelterDecorator(HomelessDecorator):
    def survive(self):
        return f"{self._homeless.survive()}, ищу убежище"

if __name__ == "__main__":
    base_homeless = BaseHomeless()
    decorated_homeless = ShelterDecorator(FoodDecorator(base_homeless))
    result = decorated_homeless.survive()

```
## Поведенческие шаблоны ##

### Шаблонный метод / Template Method ### 
Зачем нужен метод: позволяет подклассам переопределять шаги алгоритма, не меняя его общей структуры.\
Пример: метод survive() включает в себя последовательность шагов (ищем убежище, ищем еду), которые каждый подкласс бездомного (Homeless1 и Homeless2) может реализовать по-своему.

![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/5c38cfd5-8b62-4666-a520-dd9060a7c13a)

```
class Homeless(ABC):
    def survive(self):
        self.find_shelter()
        self.find_food()
    
    @abstractmethod
    def find_shelter(self):
        pass
    
    @abstractmethod
    def find_food(self):
        pass

class Homeless1(Homeless):
    def find_shelter(self):
        print("Ищем убежище в городе...")
    
    def find_food(self):
        print("Ищем еду в мусорных баках...")

class Homeless2(Homeless):
    def find_shelter(self):


        print("Ищем убежище в пригороде...")
    
    def find_food(self):
        print("Ищем съедобные растения в лесу...")

```
## Порождающие шаблоны ##
### Одиночка / Singleton ### 
Зачем нужен метод: используется для обеспечения того, что у класса есть только один экземпляр и предоставление глобальной точки доступа к этому экземпляру.\
Пример: есть класс Shelter (Приют), который предоставляет доступ к ресурсам и услугам для бездомных. Можно использовать паттерн "Одиночка" (Singleton) для обеспечения того, чтобы был только один экземпляр класса Shelter, чтобы избежать создания нескольких приютов и сохранить единый доступ к ресурсам и услугам.

![image](https://github.com/alenatetenova/HSE-SoftwareArchitecture/assets/71338455/37f63af4-f6c5-4822-b16c-8021a66badc0)


```
class Shelter:
    _instance = None
    
    def __new__(cls): #данный метод вызывается еще перед инит для выделения памяти для объекта, он работает именно с классом. 
        if cls._instance is None: #если ноне то значит объекта нет и его надо создать, для этого надо вызвать метод у предка
            cls._instance = super().__new__(cls)
        return cls._instance #создаем объект если его не было, если был - возвращается ссылочка на объекта
    
    def admit_pers(self, pers):
        print(f"{pers} принят в приют.")
    def provide_food(self):
        print("Пища предоставлена.")

if __name__ == "__main__":
    shelter1 = Shelter()
    shelter2 = Shelter()
    
    # Оба экземпляра ссылаются на один и тот же объект, и выведет: True
    print(shelter1 is shelter2) 

```
### Фабричный метод / Factory Method ###
Зачем нужен метод: используется для создания объектов определенного типа, но позволяет подклассам выбирать класс создаваемого объекта. \
Пример: есть интерфейс SupportFactory (Фабрика помощи), который содержит метод createSupport() для создания объектов помощи. Также есть классы-фабрики, такие как FoodSupportFactory (Фабрика помощи в питании) и ShelterSupportFactory (Фабрика помощи в предоставлении убежища), которые реализуют этот метод. Каждая фабрика может создавать свои собственные объекты помощи, такие как еда или убежище, в зависимости от конкретных потребностей бездомных. Таким образом, каждая фабрика решает, какой конкретный тип помощи предоставить, но интерфейс для создания помощи остается одинаковым для всех фабрик.
```
class SupportFactory(ABC):
    @abstractmethod
    def create_support(self):
        pass

class FoodSupportFactory(SupportFactory):
    def create_support(self):
        return FoodSupport()

class ShelterSupportFactory(SupportFactory):
    def create_support(self):
        return ShelterSupport()


class FoodSupport:
    def provide_support(self):
        print("Предоставляется еда")

class ShelterSupport:
    def provide_support(self):
        print("Предоставляется убежище")

def client_code(factory):
    support = factory.create_support()
    support.provide_support()  

food_factory = FoodSupportFactory()
client_code(food_factory)

shelter_factory = ShelterSupportFactory()
client_code(shelter_factory)


```

### Абстрактная фабрика / Abstract Factory ###
Зачем нужен метод: используется для создания семейств взаимосвязанных объектов без привязки к конкретным классам. .\
Пример: есть абстрактные классы Support и Inventory, которые определяют интерфейсы для предоставления поддержки и инвентаря соответственно. Затем создаем абстрактный класс Factory, который определяет методы для создания объектов поддержки и инвентаря. Также есть две конкретные фабрики: FoodFactory и ShelterFactory. Каждая из них реализует методы create_support и create_inventory, чтобы создавать конкретные объекты поддержки и инвентаря. 

```
from abc import ABC, abstractmethod

class Support(ABC):
    @abstractmethod
    def provide_support(self):
        pass

class Inventory(ABC):
    @abstractmethod
    def provide_inventory(self):
        pass

class Factory(ABC):
    @abstractmethod
    def create_support(self):
        pass
    
    @abstractmethod
    def create_inventory(self):
        pass

class FoodFactory(Factory):
    def create_support(self):
        return FoodSupport()

    def create_inventory(self):
        return FoodInventory()

class ShelterFactory(Factory):
    def create_support(self):
        return ShelterSupport()

    def create_inventory(self):
        return ShelterInventory()

class FoodSupport(Support):
    def provide_support(self):
        print("Предоставляется помощь в питании")

class FoodInventory(Inventory):
    def provide_inventory(self):
        print("Предоставляется инвентарь для питания")

class ShelterSupport(Support):
    def provide_support(self):
        print("Предоставляется помощь в предоставлении убежища")

class ShelterInventory(Inventory):
    def provide_inventory(self):
        print("Предоставляется инвентарь для убежища")

def client_code(factory):
    support = factory.create_support()
    inventory = factory.create_inventory()
    support.provide_support()
    inventory.provide_inventory()

food_factory = FoodFactory()
shelter_factory = ShelterFactory()

client_code(food_factory)
client_code(shelter_factory)

```
