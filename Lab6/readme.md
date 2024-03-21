# Лабораторная работа № 6 - Шаблоны проектирования #

## Порождающие шаблоны ##
### Фабричный метод / Factory Method ###
Зачем нужен метод: используется для создания объектов определенного типа, но позволяет подклассам выбирать класс создаваемого объекта. \
Пример: предположим, у нас есть интерфейс SupportFactory (Фабрика помощи), который содержит метод createSupport() для создания объектов помощи. У нас также есть классы-фабрики, такие как FoodSupportFactory (Фабрика помощи в питании) и ShelterSupportFactory (Фабрика помощи в предоставлении убежища), которые реализуют этот метод. Каждая фабрика может создавать свои собственные объекты помощи, такие как еда или убежище, в зависимости от конкретных потребностей бездомных. Таким образом, каждая фабрика решает, какой конкретный тип помощи предоставить, но интерфейс для создания помощи остается одинаковым для всех фабрик.
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

# Пример использования

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
Пример: создим абстрактные классы Support и Inventory, которые определяют интерфейсы для предоставления поддержки и инвентаря соответственно. Затем мы создаем абстрактный класс Factory, который определяет методы для создания объектов поддержки и инвентаря. У нас есть две конкретные фабрики: FoodFactory и ShelterFactory. Каждая из них реализует методы create_support и create_inventory, чтобы создавать конкретные объекты поддержки и инвентаря. 

```
from abc import ABC, abstractmethod

# Абстрактный класс поддержки
class Support(ABC):
    @abstractmethod
    def provide_support(self):
        pass

# Абстрактный класс инвентаря
class Inventory(ABC):
    @abstractmethod
    def provide_inventory(self):
        pass

# Общий абстрактный класс фабрики
class Factory(ABC):
    @abstractmethod
    def create_support(self):
        pass
    
    @abstractmethod
    def create_inventory(self):
        pass

# Фабрика помощи в питании
class FoodFactory(Factory):
    def create_support(self):
        return FoodSupport()

    def create_inventory(self):
        return FoodInventory()

# Фабрика помощи в предоставлении убежища
class ShelterFactory(Factory):
    def create_support(self):
        return ShelterSupport()

    def create_inventory(self):
        return ShelterInventory()

# Конкретный класс помощи в питании
class FoodSupport(Support):
    def provide_support(self):
        print("Предоставляется помощь в питании")

# Конкретный класс инвентаря для питания
class FoodInventory(Inventory):
    def provide_inventory(self):
        print("Предоставляется инвентарь для питания")

# Конкретный класс помощи в предоставлении убежища
class ShelterSupport(Support):
    def provide_support(self):
        print("Предоставляется помощь в предоставлении убежища")

# Конкретный класс инвентаря для убежища
class ShelterInventory(Inventory):
    def provide_inventory(self):
        print("Предоставляется инвентарь для убежища")

# Функция для использования фабрики
def client_code(factory):
    support = factory.create_support()
    inventory = factory.create_inventory()
    support.provide_support()
    inventory.provide_inventory()

# Пример использования
food_factory = FoodFactory()
shelter_factory = ShelterFactory()

client_code(food_factory)
client_code(shelter_factory)

```

### Одиночка / Singleton ### 
Зачем нужен метод: используется для обеспечения того, что у класса есть только один экземпляр и предоставление глобальной точки доступа к этому экземпляру.\
Пример: предположим, у нас есть класс Shelter (Приют), который предоставляет доступ к ресурсам и услугам для бездомных. Мы можем использовать паттерн "Одиночка" (Singleton) для обеспечения того, чтобы у нас был только один экземпляр класса Shelter, чтобы избежать создания нескольких приютов и сохранить единый доступ к ресурсам и услугам.

```
class Shelter:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            # Здесь могут быть инициализации ресурсов приюта
        return cls._instance
    
    def admit_pers(self, pers):
        print(f"{pers} принят в приют.")
    def provide_food(self):
        print("Пища предоставлена.")

# Пример использования:
if __name__ == "__main__":
    # Создаем экземпляры приюта
    shelter1 = Shelter()
    shelter2 = Shelter()
    
    # Оба экземпляра ссылаются на один и тот же объект
    print(shelter1 is shelter2)  # Выведет: True
    
    # Добавляем людей в приют
    shelter1.admit_pers("Коля")
    shelter2.admit_pers("Саша")

 shelter1.provide_food()

```
