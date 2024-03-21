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
Пример:

```
class Support(ABC):
    @abstractmethod
    def provide_support(self):
        pass

class Inventory(ABC):
    @abstractmethod
    def provide_inventory(self):
        pass

# Фабрика помощи в питании
class FoodFactory(Support, Inventory):
    def provide_support(self):
        print("Предоставляется помощь в питании")

    def provide_inventory(self):
        print("Предоставляется инвентарь для питания")

# Фабрика помощи в предоставлении убежища
class ShelterFactory(Support, Inventory):
    def provide_support(self):
        print("Предоставляется помощь в предоставлении убежища")

    def provide_inventory(self):
        print("Предоставляется инвентарь для убежища")


# Функция для использования фабрики
def client_code(factory):
    print("\nFactory:")
    factory.provide_support()
    factory.provide_inventory()

# Пример использования
food_factory = FoodFactory()
shelter_factory = ShelterFactory()

client_code(food_factory)
client_code(shelter_factory)
```

### Одиночка / Singleton ### 
Зачем нужен метод: используется для обеспечения того, что у класса есть только один экземпляр и предоставление глобальной точки доступа к этому экземпляру.\
Пример: предположим, у нас есть класс ShelterManager (Менеджер притонов), который отвечает за управление доступом к притонам для бездомных. Для обеспечения того, чтобы в системе существовал только один экземпляр ShelterManager, мы используем паттерн Одиночка. Таким образом, любой, кто нуждается в доступе к менеджеру притонов, может обратиться к глобальной точке доступа, чтобы получить этот единственный экземпляр. Это гарантирует согласованное управление притонами и предотвращает создание нескольких менеджеров, что могло бы привести к проблемам с синхронизацией и координацией.
## Структурные шаблоны ##
### Адаптер / Adapter
### Мост / Bridge
### Декоратор / Decorator
### Компоновщик / Composite
## Поведенческие шаблоны ##
### Стратегия / Strategy
### Наблюдатель / Observer
### Состояние / State
### Команда / Command
### Шаблонный метод / Template Method
