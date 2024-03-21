# Лабораторная работа № 6 - Шаблоны проектирования
# Шаблоны проектирования Gang of Four (GoF)
##  Порождающие шаблоны
###  1.Builder
Шаблон Builder применяется в тех случаях, когда создание сложного объекта требует сборки его составных частей пошагово или с различными конфигурациями. Он позволяет создавать различные варианты объекта, используя один и тот же процесс сборки.
**UML-диаграмма:**
Фото диаграммы
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Builder.png)
Эта диаграмма показывает, что PleuralEffusionBuilder определяет интерфейс для создания объекта PleuralEffusionAnalyzer. PleuralEffusionBuilderConcrete реализует этот интерфейс и строит объект PleuralEffusionAnalyzer. PleuralEffusionDirector использует строитель для создания объекта PleuralEffusionAnalyzer с заданными параметрами.

**Код:**
```Python
// Интерфейс объекта Route
protocol Route {
    func planRoute()
}

// Класс маршрута с полной информацией
class FullRoute: Route {
    func planRoute() {
        print("Planning a full route")
    }
}

// Класс маршрута для демонстрации
class DemoRoute: Route {
    func planRoute() {
        print("Planning a demo route")
    }
}

// Протокол фабрики маршрутов
protocol RouteFactory {
    func createRoute() -> Route
}

// Реализация фабрики маршрутов с полной информацией
class FullRouteFactory: RouteFactory {
    func createRoute() -> Route {
        return FullRoute()
    }
}

// Реализация фабрики маршрутов для демонстрации
class DemoRouteFactory: RouteFactory {
    func createRoute() -> Route {
        return DemoRoute()
    }
}

// Пример использования
let fullRouteFactory: RouteFactory = FullRouteFactory()
let fullRoute: Route = fullRouteFactory.createRoute()
fullRoute.planRoute()

let demoRouteFactory: RouteFactory = DemoRouteFactory()
let demoRoute: Route = demoRouteFactory.createRoute()
demoRoute.planRoute()
```
Этот код демонстрирует пример использования паттерна Builder для создания объекта PleuralEffusionAnalyzer, который анализирует плевральные выпоты. В данном примере конкретный строитель PleuralEffusionBuilderConcrete строит объект PleuralEffusionAnalyzer с указанными характеристиками, такими как тип изображения, алгоритм обработки и метод диагностики.

###  2.Фабричный метод / Factory Method
Шаблон Factory (Фабрика) применяется для создания объектов без необходимости предоставления клиентскому коду информации о конкретном классе создаваемого объекта. Его цель - обеспечить создание объекта определенного типа в зависимости от переданных параметров или условий.

**UML - диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Factory1.png)
Эта диаграмма показывает, что ImageProcessor является абстрактным классом, определяющим интерфейс для всех конкретных классов обработчиков изображений. XRayImageProcessor и MRIImageProcessor представляют конкретные реализации обработчиков изображений для типов X-ray и MRI соответственно. ImageProcessorFactory - это фабричный класс, который имеет статический метод get_image_processor(), возвращающий объект типа ImageProcessor в зависимости от переданного типа изображения.
**Код:**
```Python
class ImageProcessor:
    def process_image(self, image):
        pass

class XRayImageProcessor(ImageProcessor):
    def process_image(self, image):
        print("Processing X-ray image...")

class MRIImageProcessor(ImageProcessor):
    def process_image(self, image):
        print("Processing MRI image...")

class ImageProcessorFactory:
    @staticmethod
    def get_image_processor(image_type):
        if image_type == "X-ray":
            return XRayImageProcessor()
        elif image_type == "MRI":
            return MRIImageProcessor()
        else:
            raise ValueError("Unsupported image type")

# Пример использования:

image_type = "X-ray"
processor = ImageProcessorFactory.get_image_processor(image_type)
processor.process_image(image_type)

image_type = "MRI"
processor = ImageProcessorFactory.get_image_processor(image_type)
processor.process_image(image_type)
```
Этот код демонстрирует пример использования порождающего шаблона Factory для создания объектов ImageProcessor в зависимости от типа изображения. Класс ImageProcessorFactory определяет метод get_image_processor, который возвращает объект соответствующего типа в зависимости от переданного аргумента image_type.

###  3. Одиночка / Singleton
Назначение шаблона Singleton состоит в том, чтобы гарантировать, что у класса есть только один экземпляр, и предоставить глобальную точку доступа к этому экземпляру.

**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Factory1.png)
Эта диаграмма показывает, что PleuralEffusionAnalyzer имеет приватное статическое поле _instance, которое хранит экземпляр класса. Метод __new__ класса используется для создания и возвращения этого экземпляра, если он еще не был создан. Метод analyze представляет функционал объекта, который здесь представлен просто как метод анализа изображения.
**Код:**
```Python
class PleuralEffusionAnalyzer:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(PleuralEffusionAnalyzer, cls).__new__(cls, *args, **kwargs)
        return cls._instance

    def analyze(self, image):
        print("Analyzing pleural effusion...")
        # Здесь будет код анализа изображения и определения наличия плеврального выпота
        # Для примера просто выводим сообщение
        print("Pleural effusion analysis completed.")


# Пример использования:

analyzer1 = PleuralEffusionAnalyzer()
analyzer2 = PleuralEffusionAnalyzer()

print(analyzer1 is analyzer2)  # Вывод: True, оба объекта являются одним и тем же экземпляром класса
analyzer1.analyze("xray_image.jpg")
analyzer2.analyze("mri_image.jpg")
```
Этот код демонстрирует реализацию порождающего шаблона Singleton для класса PleuralEffusionAnalyzer. При попытке создания нового объекта класса PleuralEffusionAnalyzer, если экземпляр еще не создан, будет создан только один экземпляр, который будет использоваться для всех последующих вызовов.

##  Структурные шаблоны
###  1. Адаптер / Adapter
Назначение шаблона Adapter (Адаптер) заключается в том, чтобы позволить объектам с несовместимыми интерфейсами работать вместе. Он преобразует интерфейс одного класса в интерфейс, ожидаемый клиентом, что позволяет объектам совместно взаимодействовать, не зная о внутренней реализации друг друга.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Adapter.png)
Эта диаграмма показывает, что PleuralEffusionDetector - это абстрактный класс или интерфейс, определяющий метод detect, который используется для обнаружения плевральных выпотов. ThirdPartyDetector - это класс, представляющий сторонний компонент, который имеет свой собственный метод find_pleural_effusion для обнаружения плевральных выпотов. Adapter - это класс-адаптер, который адаптирует интерфейс ThirdPartyDetector к интерфейсу PleuralEffusionDetector, предоставляя метод detect, который вызывает метод find_pleural_effusion стороннего компонента.
**Код:**
```Python
class PleuralEffusionDetector:
    def detect(self, image):
        pass

class ThirdPartyDetector:
    def find_pleural_effusion(self, img):
        print("Using third-party detector to find pleural effusion in the image...")

class Adapter(PleuralEffusionDetector):
    def __init__(self, third_party_detector):
        self.third_party_detector = third_party_detector

    def detect(self, image):
        self.third_party_detector.find_pleural_effusion(image)


# Пример использования:

third_party_detector = ThirdPartyDetector()
adapter = Adapter(third_party_detector)

adapter.detect("xray_image.jpg")
```
Этот код демонстрирует реализацию структурного шаблона Adapter. В данном примере класс ThirdPartyDetector представляет сторонний компонент, который имеет метод find_pleural_effusion, используемый для обнаружения плевральных выпотов. Класс Adapter адаптирует этот сторонний компонент к интерфейсу PleuralEffusionDetector, предоставляя метод detect, который вызывает метод find_pleural_effusion стороннего компонента.

### 2. Декоратор / Decorator
Шаблон Decorator применяется для динамического добавления нового функционала или обязанностей объекту без изменения его основного класса. Он позволяет оборачивать объекты в дополнительные оболочки или декораторы, каждый из которых добавляет свою функциональность.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Decorator.png)
Эта диаграмма показывает, что у нас есть базовый класс PleuralEffusionAnalyzer, который выполняет анализ плевральных выпотов. Затем у нас есть абстрактный класс PleuralEffusionAnalyzerDecorator, который также имеет метод analyze, но также содержит ссылку на объект PleuralEffusionAnalyzer. PreProcessingDecorator и PostProcessingDecorator являются конкретными декораторами, расширяющими базовый класс декоратора PleuralEffusionAnalyzerDecorator. Оба декоратора также имеют метод analyze, который вызывает соответствующий метод базового декоратора и выполняет дополнительные действия перед или после вызова базового метода.
**Код:**
```Python
from abc import ABC, abstractmethod

# Базовый класс декоратора
class PleuralEffusionAnalyzerDecorator(ABC):
    def __init__(self, analyzer):
        self._analyzer = analyzer

    @abstractmethod
    def analyze(self):
        pass

# Конкретные декораторы
class PreProcessingDecorator(PleuralEffusionAnalyzerDecorator):
    def analyze(self):
        print("Performing preprocessing before analysis")
        self._analyzer.analyze()

class PostProcessingDecorator(PleuralEffusionAnalyzerDecorator):
    def analyze(self):
        self._analyzer.analyze()
        print("Performing postprocessing after analysis")

# Класс анализатора
class PleuralEffusionAnalyzer:
    def analyze(self):
        print("Analyzing pleural effusion...")

# Пример использования:

analyzer = PleuralEffusionAnalyzer()
analyzer_with_preprocessing = PreProcessingDecorator(analyzer)
analyzer_with_pre_and_post_processing = PostProcessingDecorator(analyzer_with_preprocessing)

analyzer_with_pre_and_post_processing.analyze()
```
В этом примере PleuralEffusionAnalyzer представляет класс анализатора плевральных выпотов, а PleuralEffusionAnalyzerDecorator является абстрактным классом декоратора. PreProcessingDecorator и PostProcessingDecorator - это конкретные декораторы, которые добавляют дополнительную функциональность перед и после основной операции анализа. При выполнении кода создается экземпляр анализатора, который затем оборачивается в декораторы, добавляющие предварительную и последующую обработку. В результате вызов метода analyze() декорированного объекта сначала выполняет предварительную обработку, затем основную операцию анализа и, наконец, последующую обработку.

###  3. Мост / Bridge
Шаблон Bridge (Мост) предназначен для разделения абстракции от ее реализации таким образом, чтобы они могли изменяться независимо друг от друга. Он используется в ситуациях, когда необходимо иметь возможность изменять и расширять как абстракцию, так и реализацию независимо друг от друга.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Bridge.png)
Эта диаграмма показывает, что у нас есть абстрактный класс PleuralEffusionAnalyzer, который определяет метод analyze() для анализа плевральных выпотов. У нас есть две конкретные реализации этого класса: XRayAnalyzer и MRIAnalyzer, которые представляют анализаторы, использующие различные технологии для анализа. Затем у нас есть абстрактный класс AnalyzerHandler, который определяет метод handle_analyze() для обработки результатов анализа. У нас также есть две конкретные реализации этого класса: GeneralAnalyzerHandler и SpecializedAnalyzerHandler, которые представляют различные способы обработки результатов анализа для обоих типов анализаторов.

**Код:**
```Python
from abc import ABC, abstractmethod

# Абстрактный класс для анализатора плевральных выпотов
class PleuralEffusionAnalyzer(ABC):
    @abstractmethod
    def analyze(self):
        pass

# Конкретные реализации анализатора плевральных выпотов
class XRayAnalyzer(PleuralEffusionAnalyzer):
    def analyze(self):
        print("Analyzing pleural effusion using X-ray...")

class MRIAnalyzer(PleuralEffusionAnalyzer):
    def analyze(self):
        print("Analyzing pleural effusion using MRI...")

# Абстрактный класс для интерфейса обработчика анализатора
class AnalyzerHandler(ABC):
    def __init__(self, analyzer):
        self._analyzer = analyzer

    @abstractmethod
    def handle_analyze(self):
        pass

# Конкретные реализации обработчика анализатора
class GeneralAnalyzerHandler(AnalyzerHandler):
    def handle_analyze(self):
        print("General handling of analysis:")
        self._analyzer.analyze()

class SpecializedAnalyzerHandler(AnalyzerHandler):
    def handle_analyze(self):
        print("Specialized handling of analysis:")
        self._analyzer.analyze()

# Пример использования:

xray_analyzer = XRayAnalyzer()
mri_analyzer = MRIAnalyzer()

general_handler = GeneralAnalyzerHandler(xray_analyzer)
specialized_handler = SpecializedAnalyzerHandler(mri_analyzer)

general_handler.handle_analyze()
specialized_handler.handle_analyze()
```
В этом примере мы имеем абстрактный класс PleuralEffusionAnalyzer, который определяет интерфейс для анализатора плевральных выпотов. У нас есть две конкретные реализации этого интерфейса: XRayAnalyzer и MRIAnalyzer, которые представляют анализаторы, использующие различные технологии для анализа. Затем мы имеем абстрактный класс AnalyzerHandler, который представляет интерфейс для обработчика анализатора. У нас есть две конкретные реализации этого интерфейса: GeneralAnalyzerHandler и SpecializedAnalyzerHandler, которые представляют различные способы обработки результатов анализа.

###  4. Компоновщик / Composite
Шаблон Composite используется для создания структуры объектов в виде древовидной иерархии, где как отдельные объекты (листья) так и группы объектов (ветви) могут быть обработаны одинаково.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Composite.png)
Эта диаграмма показывает, что у нас есть абстрактный класс Component, который определяет общий интерфейс для всех компонентов в структуре. XRayAnalyzer представляет конкретный компонент (листовой узел), который реализует метод analyze(). CompositeAnalyzer представляет композит, который может содержать другие компоненты, включая другие композиты. CompositeAnalyzer также реализует метод analyze(), который вызывает метод analyze() для каждого компонента в своем списке компонентов.

**Код:**
```Python
from abc import ABC, abstractmethod

# Абстрактный класс компонента
class Component(ABC):
    @abstractmethod
    def analyze(self):
        pass

# Листовой узел (Конкретный компонент)
class XRayAnalyzer(Component):
    def analyze(self):
        print("Analyzing pleural effusion using X-ray...")

# Компоновщик (Композит)
class CompositeAnalyzer(Component):
    def __init__(self):
        self._components = []

    def add_component(self, component):
        self._components.append(component)

    def remove_component(self, component):
        self._components.remove(component)

    def analyze(self):
        print("Composite analysis:")
        for component in self._components:
            component.analyze()

# Пример использования:

xray1 = XRayAnalyzer()
xray2 = XRayAnalyzer()
composite_analyzer = CompositeAnalyzer()
composite_analyzer.add_component(xray1)
composite_analyzer.add_component(xray2)

composite_analyzer.analyze()
```
Этот код демонстрирует использование шаблона Composite. У нас есть абстрактный класс Component, представляющий общий интерфейс для всех компонентов в структуре. Затем у нас есть листовой узел XRayAnalyzer, который представляет конкретный компонент. CompositeAnalyzer - это компоновщик, который может содержать другие компоненты (листовые или другие композиты). Когда вызывается метод analyze() для CompositeAnalyzer, он также вызывает метод analyze() для всех компонентов, которые он содержит, в том числе и другие композиты, если они есть.

## Поведенческий шаблон

### 1. Шаблонный метод / Template Method
Шаблон Template Method определяет основу алгоритма в родительском классе, позволяя подклассам переопределить определенные шаги этого алгоритма без изменения его структуры.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Template%20Method.png)
Эта диаграмма показывает, что у нас есть абстрактный класс PleuralEffusionAnalyzerTemplate, который содержит метод analyze() как шаблонный метод для выполнения алгоритма анализа. MRIAnalyzer и XRayAnalyzer представляют конкретные реализации этого шаблонного метода, где каждый из них реализует свои специфические шаги алгоритма, такие как load_data(), preprocess_data(), analyze_data() и postprocess_data().

**Код:**
```Python
from abc import ABC, abstractmethod

# Абстрактный класс для анализатора плевральных выпотов
class PleuralEffusionAnalyzerTemplate(ABC):
    def analyze(self):
        self.load_data()
        self.preprocess_data()
        self.analyze_data()
        self.postprocess_data()

    @abstractmethod
    def load_data(self):
        pass

    @abstractmethod
    def preprocess_data(self):
        pass

    @abstractmethod
    def analyze_data(self):
        pass

    @abstractmethod
    def postprocess_data(self):
        pass

# Конкретный анализатор плевральных выпотов с использованием метода МРТ
class MRIAnalyzer(PleuralEffusionAnalyzerTemplate):
    def load_data(self):
        print("Loading MRI data...")

    def preprocess_data(self):
        print("Preprocessing MRI data...")

    def analyze_data(self):
        print("Analyzing pleural effusion using MRI...")

    def postprocess_data(self):
        print("Postprocessing MRI analysis results...")

# Конкретный анализатор плевральных выпотов с использованием метода рентгенографии
class XRayAnalyzer(PleuralEffusionAnalyzerTemplate):
    def load_data(self):
        print("Loading X-ray data...")

    def preprocess_data(self):
        print("Preprocessing X-ray data...")

    def analyze_data(self):
        print("Analyzing pleural effusion using X-ray...")

    def postprocess_data(self):
        print("Postprocessing X-ray analysis results...")

# Пример использования:

mri_analyzer = MRIAnalyzer()
mri_analyzer.analyze()

print("\n")

xray_analyzer = XRayAnalyzer()
xray_analyzer.analyze()
```
Этот код демонстрирует использование шаблона Template Method для определения общего процесса анализа в абстрактном классе PleuralEffusionAnalyzerTemplate, а также конкретных реализаций этого процесса в классах MRIAnalyzer и XRayAnalyzer. Каждый конкретный анализатор переопределяет необходимые шаги алгоритма анализа, оставляя остальные шаги без изменений.

## 2. Стратегия / Strategy
Шаблон Strategy (стратегия) предназначен для определения семейства алгоритмов, инкапсуляции каждого из них и обеспечения их взаимозаменяемости. Он позволяет выделить набор алгоритмов из класса и сделать их независимыми от клиентов, которые их используют. 
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Strategy.png)
Эта диаграмма показывает, что у нас есть абстрактный класс DetectionStrategy, который определяет интерфейс для всех стратегий обнаружения плевральных выпотов. XRayDetectionStrategy и MRIDetectionStrategy представляют конкретные стратегии, реализующие метод detect_effusion() в соответствии с использованным методом обнаружения. EffusionAnalyzer представляет класс контекста, который содержит ссылку на объект стратегии. Он использует эту стратегию для выполнения обнаружения плевральных выпотов. Внутри EffusionAnalyzer есть метод set_detection_strategy(), который позволяет изменять стратегию, и метод detect_effusion(), который вызывает метод detect_effusion() текущей стратегии.

**Код:**
```Python
from abc import ABC, abstractmethod

# Абстрактный класс стратегии
class DetectionStrategy(ABC):
    @abstractmethod
    def detect_effusion(self):
        pass

# Конкретная стратегия: обнаружение с использованием рентгенографии
class XRayDetectionStrategy(DetectionStrategy):
    def detect_effusion(self):
        print("Detecting effusion using X-ray...")

# Конкретная стратегия: обнаружение с использованием МРТ
class MRIDetectionStrategy(DetectionStrategy):
    def detect_effusion(self):
        print("Detecting effusion using MRI...")

# Класс контекста
class EffusionAnalyzer:
    def __init__(self, detection_strategy):
        self._detection_strategy = detection_strategy

    def set_detection_strategy(self, detection_strategy):
        self._detection_strategy = detection_strategy

    def detect_effusion(self):
        self._detection_strategy.detect_effusion()

# Пример использования:

# Создание объекта контекста с передачей ему конкретной стратегии (например, рентгенография)
xray_strategy = XRayDetectionStrategy()
analyzer = EffusionAnalyzer(xray_strategy)
analyzer.detect_effusion()

print("\n")

# Переключение на другую стратегию (например, МРТ)
mri_strategy = MRIDetectionStrategy()
analyzer.set_detection_strategy(mri_strategy)
analyzer.detect_effusion()
```
В этом примере определен абстрактный класс DetectionStrategy, который представляет собой интерфейс для всех стратегий обнаружения плевральных выпотов. Затем определены две конкретные стратегии: XRayDetectionStrategy для обнаружения с использованием рентгенографии и MRIDetectionStrategy для обнаружения с использованием МРТ.

## 3. Команда / Command
Шаблон Command используется для инкапсуляции запроса в виде объекта, позволяя параметризовать клиентов с различными запросами, организовывать отмену операций и выполнение их асинхронно или в параллельных потоках
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Command.png)
Эта диаграмма показывает, что у нас есть абстрактный класс Command, который определяет метод execute(). MriAnalysisCommand и XRayAnalysisCommand представляют конкретные команды, которые реализуют этот метод.Invoker содержит список команд и имеет методы для добавления новых команд и выполнения всех команд. Клиент создает экземпляры конкретных команд и передает их в Invoker, который затем может выполнить их по запросу.
**Код:**
```Python
from abc import ABC, abstractmethod

# Команды
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

# Конкретная команда: команда для анализа плевральных выпотов по данным МРТ
class MriAnalysisCommand(Command):
    def __init__(self, data):
        self.data = data

    def execute(self):
        print("Executing MRI analysis with data:", self.data)

# Конкретная команда: команда для анализа плевральных выпотов по данным рентгенографии
class XRayAnalysisCommand(Command):
    def __init__(self, data):
        self.data = data

    def execute(self):
        print("Executing X-ray analysis with data:", self.data)

# Инвокер
class Invoker:
    def __init__(self):
        self._commands = []

    def add_command(self, command):
        self._commands.append(command)

    def execute_commands(self):
        for command in self._commands:
            command.execute()

# Пример использования:
if __name__ == "__main__":
    # Создание команд для анализа плевральных выпотов
    mri_command = MriAnalysisCommand("MRI data")
    xray_command = XRayAnalysisCommand("X-ray data")

    # Создание инвокера и добавление команд
    invoker = Invoker()
    invoker.add_command(mri_command)
    invoker.add_command(xray_command)

    # Выполнение команд
    invoker.execute_commands()
```
В этом примере мы определяем абстрактный класс Command, который имеет метод execute(). Затем мы создаем две конкретные команды: MriAnalysisCommand и XRayAnalysisCommand, которые реализуют метод execute(). Каждая команда принимает данные для анализа. Класс Invoker отвечает за выполнение команд. Он хранит список команд и имеет метод execute_commands(), который последовательно вызывает метод execute() для каждой команды.

## 4. Наблюдатель / Observer
Шаблон Observer используется для определения зависимости "один-ко-многим" между объектами таким образом, что при изменении состояния одного объекта все зависимые от него объекты автоматически оповещаются и обновляются.
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/Observer.png)

**Код:**
```Python
class Subject:
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self):
        for observer in self._observers:
            observer.update(self)

class Observer:
    def update(self, subject):
        pass

# Конкретный субъект
class PleuralEffusionDetector(Subject):
    def __init__(self):
        super().__init__()
        self._effusion_detected = False

    @property
    def effusion_detected(self):
        return self._effusion_detected

    @effusion_detected.setter
    def effusion_detected(self, value):
        self._effusion_detected = value
        self.notify()

# Конкретный наблюдатель
class Doctor(Observer):
    def update(self, subject):
        if isinstance(subject, PleuralEffusionDetector) and subject.effusion_detected:
            print("Doctor: Pleural effusion detected! Immediate action required.")

# Пример использования:
if __name__ == "__main__":
    effusion_detector = PleuralEffusionDetector()
    doctor1 = Doctor()
    doctor2 = Doctor()

    effusion_detector.attach(doctor1)
    effusion_detector.attach(doctor2)

    # Предположим, плевральный выпот был обнаружен
    effusion_detector.effusion_detected = True
```
В этом примере у нас есть абстрактный класс Subject, который определяет интерфейс для добавления, удаления и оповещения наблюдателей. Observer - это абстрактный класс, который определяет метод update(), который вызывается субъектом при изменении состояния. PleuralEffusionDetector - это конкретный субъект, который отслеживает наличие плеврального выпота. Когда состояние этого субъекта изменяется, он оповещает своих наблюдателей. Doctor - это конкретный наблюдатель, который реагирует на оповещения о наличии плеврального выпота и выводит сообщение о необходимости принятия мер.

## 5. Состояние / State
Шаблон State используется для моделирования объекта, который может изменять свое поведение в зависимости от его внутреннего состояния
**UML-диаграмма:**
![paps2 drawio](https://github.com/Vodyiara/pap_hse/blob/main/Lab%20Work%20%E2%84%966/Lab6_photos/State.png)
Эта диаграмма показывает, что у нас есть класс PleuralEffusionDetector, который содержит внутреннее состояние _state, являющееся объектом класса State. Класс State определяет интерфейс для всех конкретных состояний. У нас есть три конкретных состояния: NormalState, SuspiciousState и EffusionDetectedState, каждое из которых реализует метод analyze() в соответствии с текущим состоянием объекта. Класс PleuralEffusionDetector содержит метод set_state(), который позволяет изменять состояние, и метод analyze(), который делегирует выполнение анализа текущему состоянию _state.

**Код:**
```Python
from abc import ABC, abstractmethod

# Класс контекста
class PleuralEffusionDetector:
    def __init__(self):
        self._state = NormalState(self)

    def set_state(self, state):
        self._state = state

    def analyze(self):
        self._state.analyze()

# Абстрактный класс состояния
class State(ABC):
    def __init__(self, detector):
        self._detector = detector

    @abstractmethod
    def analyze(self):
        pass

# Конкретное состояние: нормальное состояние
class NormalState(State):
    def analyze(self):
        print("No pleural effusion detected.")

# Конкретное состояние: состояние с подозрением на плевральный выпот
class SuspiciousState(State):
    def analyze(self):
        print("Suspicious for pleural effusion. Further investigation needed.")

# Конкретное состояние: обнаружен плевральный выпот
class EffusionDetectedState(State):
    def analyze(self):
        print("Pleural effusion detected. Immediate action required.")

# Пример использования:
if __name__ == "__main__":
    detector = PleuralEffusionDetector()

    # Анализ в нормальном состоянии
    detector.analyze()

    # Переключение на состояние с подозрением на плевральный выпот
    detector.set_state(SuspiciousState(detector))
    detector.analyze()

    # Переключение на состояние обнаруженного плеврального выпота
    detector.set_state(EffusionDetectedState(detector))
    detector.analyze()
```
В этом примере мы имеем класс PleuralEffusionDetector, который является контекстом и имеет внутреннее состояние _state. Этот класс имеет метод analyze(), который делегирует анализ текущему состоянию. У нас также есть абстрактный класс State, определяющий интерфейс для всех конкретных состояний. Затем у нас есть три конкретных состояния: NormalState, SuspiciousState и EffusionDetectedState, каждое из которых реализует метод analyze() в соответствии с текущим состоянием объекта.

#GRASP
## Роли классов
### 1. PleuralEffusionDetecto
    - Информационный эксперт для текущего состояния системы
    - Создатель для объектов состояний
### 2.State
    - Информационный эксперт для процесса анализа
    - Выполняет функции контекста для шаблона State
### 3.NormalState
    - Конкретное состояние для нормального состояния системы
### 4.SuspiciousState
    - Конкретное состояние для состояния с подозрением на плевральный выпот
### 5.EffusionDetectedState
    - Конкретное состояние для обнаруженного плеврального выпота
## Принципы разработки
   1.Высокоуровневые модули не должны зависеть от низкоуровневых модулей. Оба должны зависеть от абстракций (Dependency Inversion Principle): В коде шаблона State, класс PleuralEffusionDetector зависит от абстракции State, а не от конкретных состояний.

 2.Принцип единственной ответственности (Single Responsibility Principle): Каждый класс в коде отвечает за выполнение только одной задачи. Например, классы State и его подклассы отвечают только за управление состоянием объекта, а класс PleuralEffusionDetector отвечает за управление процессом анализа.

   3.Принцип подстановки Барбары Лисков (Liskov Substitution Principle): Подклассы могут быть заменены своими базовыми классами без изменения ожидаемого поведения. В коде шаблона State, все подклассы State могут быть использованы вместо базового класса без изменения поведения контекста PleuralEffusionDetector.

## Свойства программы
Свойство: Гибкость и расширяемость.

Обоснование: При использовании шаблона State система становится гибкой и расширяемой, поскольку можно легко добавлять новые состояния и изменять поведение системы, не изменяя ее основной логики. Это особенно полезно в системе распознавания плевральных выпотов, где может потребоваться добавление новых состояний или изменение алгоритмов анализа в будущем.
    
