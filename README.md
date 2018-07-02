- [Общие принципы](#Общие-принципы)
- [Именование](#Именование)
- [Отступы](#Отступы)
- [Комментарии](#Комментарии)
- [Классы и структуры](#Классы-и-структуры)
- [Вложенные типы](#Вложенные-типы)
- [Объявления функций](#Объявления-функций)
- [Замыкания](#Замыкания)
- [Типы](#Типы)
- [Управление памятью](#Управление-памятью)
- [Управление доступом](#Управление-доступом)
- [Циклы](#Циклы)
- [Golden path](#Golden-path)
- [Точки с запятыми](#Точки-с-запятыми)
- [Скобки в if else](#Скобки-в-if-else)
- [Копирайт](#Копирайт)
- [Необязательные параметры функций](#Необязательные-параметры-функций)

Основано на [https://github.com/raywenderlich/swift-style-guide](https://github.com/raywenderlich/swift-style-guide)

Хорошо написанный код облегчает чтение и понимание его вашими коллегами и вами же в будущем. Относитесь с уважением к тому, кто будет читать ваш код.

## Общие принципы

*   Глобальные константы недопустимы, они всегда должны находиться внутри неймспейса.
*   Warnings компилятора считаются ошибками и не допускаются. Исключение: TODO:, FIXME:

## Именование

Следует придерживаться правил [Swift.org — Api Design Guidelines](https://swift.org/documentation/api-design-guidelines/)

*   Имена переменных должны описывать переменную семантически (смысл её передавать), переменные вида **a** или **x** не допускаются. 
*   Следует избегать сокращений (например, **res**), если они не общеприняты (например, **url**).
*   При объявлении функции/переменной именовать её следует, спрашивая себя: а будет ли это название понятно при её вызове/использовании (где может не быть того контекста для понимания, который есть при их объявлении)?
*   Ясность названия всегда имеет приоритет над краткостью.
*   Используется **camelCase**, а не **snake_case**.
*   Имена типов и протоколов начинаются с заглавной, всё остальное со строчной.
*   Название должно описывать роль, назначение переменной/функции, а не её тип.
*   Фабрики должны начинаться со слова **make**.
*   Функции должны именоваться соответсвенно их побочному действию:
    *   Если метод мутирующий, то используется глагол: sequence.sort(), то есть метод меняет саму sequence в этом примере.
    *   Если метод не мутируюший, то используется причастие: sequence.sorted(), метод возвращает отсортированную послевадельность, но сама исходная sequence при этом не изменяется.
*   Протоколы именуются в зависимости от того, что они описывают:
    *   Если это _сущность_, то используется существительное: **Person**.
    *   Если это _спобность_, то название должно заканчиваться на _-able_, _-ible_: **Drinkable**, **Convertible**.
*   Параметры в функциях должны именоваться так, чтобы вместе с именем метода они читались как документация к методу.
*   Следует именовать параметры кортежей (tuples), потому что потом при ис

Как надо
```swift
struct WidgetContainer {
    private let maximumWidgetCount = 100
    var widgetButton: UIButton
    var widgetHeightPercentage = 0.85
}
```

Как не надо
```swift
let MAX_WIDGET_COUNT = 100
struct widget_container {
    var widgetBtn: UIButton
    var widgetHeightPerc = 0.85
}
```

## Отступы

*   Для идентации используется 4 пробела (не табы).
*   У функций, циклов и других выражений (if, else, switch, while, etc.) открывающая фигурная скобка должна находиться на той же строке, что и выражение. Закрывающая же всегда на новой строке.
*   Между конструкцией и открывающей фигурной скобкой должен быть ровно один пробел.
*   Скобки вызова функции не отделяются от него пробелом.
*   Между функциями должна быть ровно одна пустая строка, для удобства чтения и организации кода. Внутри функции допускаются пробелы для разделения блоков функциональности, но слишком много блоков указывает на то, что метод должен быть разделён на несколько (SRP).

Как надо
```swift
func doSomething() {
    // Do something
}
  
if user.isHappy {
    doSomething()
} else {
    // Do something else
}
```

Как не надо
```swift
func doSomething (){
// Do something
}
if user.isHappy
{
    doSomething ()
}
else{
  // Do something else }
```

*   Двоеточие всегда ставится без пробела слева и с одним пробелом справа. Исключения — тернарный оператор ? :, пустой словарь [:] и #selector синтаксис для параметров без имени (_:).

## Комментарии

Комментарии должны описывать не то, _что_ делает какой-то кусок кода, а то, _почему _он это делает.

Комментарии надо либо всё время поддерживать в актуальном состоянии, либо удалять.

Не надо писать посреди кода большие блочные комментарии на несколько строк, если это не документация.

## Классы и структуры

**Когда использовать классы, а когда — структуры?**

Классы надо использовать для объектов, у которых есть какая-то внутренняя уникальность, которая не сводится к значениям их полей.

Допустим, у нас есть класс Person:

```swift
class Person {
  var firstName: String
  var lastName: String
  var birthdate: Date
}
```

Может оказаться, что у нас есть два разных человека, alexander1 и alexander2, у которых полностью совпадают имена и которые родились в один день. Но, несмотря на полное совпадение полей, alexander1 и alexander2 — разные сущности.

Обычно такая внутренняя уникальность связана с тем, что у объекта есть изменяемое состояние. Например, человек может пойти в паспортный стол и поменять имя. Но он всё равно останется тем же самым человеком.

Так как состояние объекта может поменяться, то даже если сейчас у каких-то двух объектов все поля совпадают, в будущем это может нарушиться.

Есть другие объекты. У них нет изменяемого состояния и какой-то внутренней уникальности. Они полностью сводятся к значениям своих полей.

Это, например, даты.

Допустим, у нас есть структура Date:

```swift
struct Date {
  let day: Int
  let month: Int
  let year: Int
}
```
Если у даты поменять день или месяц, то мы получим не ту же самую дату, а другую. Если у двух дат совпадают день, месяц и год, то это — одна и та же дата.

Состояние у таких объектов всегда неизменяемое, потому что, если изменить состояние, мы получим другой объект.

Для таких объектов надо использовать структуры.

_Замечание. Иногда для сущностей, которые должны быть структурами, всё-таки приходится использовать классы. Например, потому, что от них требуют соблюдать протокол AnyObject, или потому, что они уже реализованы как классы (скажем, NSDate и NSSet)._

**Пример хорошего объявления класса:**

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }
 
  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }
 
  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }
 
  override func area() -> Double {
    return Double.pi * radius * radius
  }
}
 
extension Circle: CustomStringConvertible {
  var description: String {
    return "center = \(centerString) area = \(area())"
  }
  private var centerString: String {
    return "(\(x),\(y))"
  }
}
```
Этот пример демонстрирует следующие правила:

— перед двоеточием пробел не ставится, а после — ставится (например, x: Int и circle: Shape)

— близкие по назначению переменные объявляются на одной строке

— блоки get, set, willSet и didSet должны быть сдвинуты вправо относительно родительского свойства

— модификаторы не указываются, если по умолчанию они и так включены (например, по умолчанию все свойства и методы — internal)

— модификаторы не указываются, когда мы переопределяем метод или свойство (например, override func area() → Double)

— дополнительные функции выносятся в расширения к классу

— в расширении к классу внутренние детали реализации (например, centerString) скрываются от внешнего мира с помощью модификатора private

**Использование self**

В отличие от Objective-C, в Свифте не обязательно использовать self при вызове методов или свойств объекта.

self обязателен только в некоторых случаях. Например, внутри замыканий, помеченных как @escaping, и в конструкторе — для того, чтобы отличить свойства объекта от аргументов конструктора.

Везде, где программа компилируется без self, self надо убирать.

**Вычисляемые свойства**

Если у вычисляемого свойства есть только геттер, то конструкцию get { ... } писать не надо.

Правильно:
```swift
var diameter: Double {
  return radius * 2
}
```
Неправильно:
```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

**Final**

В экспериментальном коде помечать классы или свойства как final не обязательно — это отвлекает от сути. Но иногда final полезен — он помогает прояснить ваши намерения. В нижеприведённом коде класс Box имеет вполне определённое назначение, и его логично закрыть от наследования:

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
  let value: T
  init(_ value: T) {
    self.value = value
  }
}
```

## Вложенные типы

При необходимости использования вложенных типов (`nested types`) подтипы необходимо складывать в `extension` внури файла с основной моделью после ее описания. Колличество `extension` в которые будут складываться вложенные типы выбираются по усмотрению разработчика. Если появилась необоходимость сделать вложенный тип, для вложенного типа, то его нахождение делается так-же на усмотрение разработчика. Каждый вложенный тип необходимо выделять `MARK:`.

В случае, если колличество строк внутри файла превышает установленные ограничения, то допустимо разбивать типы по файлам по установленным выше правилам.

**Правильно:**
```swift
struct Person {
    let id: String
    let document: Document
    let gender: Gender
}

// MARK: - Document
extension Person {
    struct Document {

        let id: String
        let kind: Kind

        // MARK: - Kind
        enum Kind {
            case plain
            case notPlain
        }
    }
}

// MARK: - Gender
extension Person {
    enum Gender {
        case male
        case female
        case unspecified
    }
}
```

**Неправильно:**
```swift
struct Person {

    // MARK: - Document
    struct Document {
        
        enum Kind {
            case plain
            case notPlain
        }
        
        let id: String
        let kind: Kind
    }
    
    // MARK: - Gender
    enum Gender {
        case male
        case female
        case unspecified
    }

    let id: String
    let document: Document
    let gender: Gender
}
```

**Вложенные типы, как namespace**
// TODO:

## Объявления функций

Короткие объявления функций надо писать в одну строчку. Открывающая фигурная скобка не должна переносится на новую строку:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```
Если у функции очень длинная сигнатура, то объявление функции надо разбивать на несколько строк. При этом все строки, кроме первой, должны быть с отступом:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  // reticulate code goes here
}
```
## Замыкания

Специальный синтаксис для замыканий надо использовать в единственном случае: среди параметров есть только одно замыкание и оно идёт последним.

Параметрам замыканий надо давать понятные имена.

Правильно:

```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}
 
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```
Неправильно:

```swift
UIView.animate(withDuration: 1.0, animations: { // не использован специальный синтаксис для последнего замыкания
  self.myView.alpha = 0
})
 
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in // здесь, наоборот, специальный синтаксис для замыкания использовать не надо; кроме того, f — непонятное название
  self.myView.removeFromSuperview()
}
```
Если замыкание состоит из одного выражения, можно использовать неявный return:

```swift
attendeeList.sort { a, b in
  a > b
}
```
Цепочки из методов высшего порядка ( strings.map { ...} filter {...} ) должны быть понятными и читаться однозначно.

Как расставлять пробелы и переносы строк в таких цепочках и следует ли использовать анонимные параметры ($0, $1 и так далее) — всё это остаётся на усмотрение автора.

Примеры того, как можно делать:

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)
 
let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```
##Типы

Если есть возможность, надо использовать родные свифтовые типы (Double, String, Date), а не старые типы из Objective-C (NSNumber, NSString, NSDate).

В Свифте есть трансформации, которые превращают свифтовые типы в старые типы из Objective-C. Поэтому, когда понадобится какой-нибудь старый тип, его всегда можно получить из свифтового.

Правильно:

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```
Неправильно:

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```
Но при работе со SpriteKit можно использовать CGFloat, чтобы не было слишком много конверсий.

**Константы**

Константы определяются с помощью ключевого слова let, а переменные — с помощью ключевого слова var.

Если значение переменной не будет меняться, надо использовать let, а не var.

_Совет: можно всегда использовать let и заменять его на var только тогда, когда компилятор начинает ругаться._

Константы можно объявить не только у объекта, но и у типа (static let ...).

Статические константы — лучше, чем глобальные переменные, потому что их легче отличить от полей класса.

Правильно:

```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}
 
let hypotenuse = side * Math.root2 // сразу ясно, что root2 — статическая переменная в енуме Math
```

Неправильно:

```swift
e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872
 
let hypotenuse = side * root2 // не ясно, что такое root2: то ли локальная переменная, то ли глобальная
```
Статические методы и свойства работают примерно так же, как глобальные функции и переменные. Их не надо использовать слишком часто. Они полезны, когда нужно выделить функционал, относящийся к конкретному типу, или когда нужна совместимость с Objective-C.

**Опциональные поля**

Если переменная может стать равной nil, надо помечать такую переменную как опциональную с помощью знака ?

Если функция может вернуть nil, надо помечать возвращаемое значение функции как опциональное (тоже с помощью знака ?).

Implicitly-unwrapped переменные (var x: String!) надо использовать только тогда, когда точно известно, что они будут инициализированы перед первым использованием. Например, это могут быть дочерние вью, которые инициализируются во viewDidLoad().

Если мы обращаемся к опциональной переменной только один раз (или если надо обратиться к цепочке из вложенных опциональных полей), лучше воспользоваться знаком вопроса:

```swift
self.textContainer?.textField?.setNeedsDisplay()
```
Если мы обращаемся к опциональной переменной несколько раз подряд, то удобнее использовать if let ... :

```swift
if let textField = self.textContainer.textField {
  textField.hidden = false
  textField.backgroundColor = UIColor.green
  textField.text = "This is green text field."
}
```
В названиях опциональных полей не надо подчёркивать их опциональность. Опциональность уже указана в типе.

Неправильно:

```swift
var optionalTitleString: String?
var maybeMainSubview: UIView?
```
Правильно:

```swift
var titleString: String?
var mainSubview: UIView?
```
**Ленивая инициализация**

Если нужно точно контролировать время создания объектов, надо использовать ленивую инициализацию. Это особенно актуально для вью-контроллеров, которые лениво загружают вью.

Для ленивой инициализации можно использовать либо замыкание, которое вызывается немедленно ( makeView { ... } () ), либо приватный фабричный метод.

Замыкание, которое вызывается немедленно:

```swift
lazy var locationManager: CLLocationManager = {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
} ()
```
Приватный фабричный метод:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()
 
private func makeLocationManager() -> CLLocationManager {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
}
```
_Примечания:_

_— здесь точно не будет ретэйн-цикла, поэтому не обязательно использовать [unowned self]_

_— у этого метода есть побочный эффект: когда мы вызываем manager.requestAlwaysAuthorization(), на экране телефона может появиться алерт, спрашивающий у пользователя разрешения; именно поэтому имеет смысл использовать ленивую инициализацию_

**Автоматический вывод типов**

Обычно при объявлении переменных не надо указывать их тип — компилятор догадается и сам, а код станет проще.

Случаи, когда тип переменных всё-таки надо указывать:

— сложные массивы или другие коллекции

— переменные типа CGFloat или Int16

Правильно:

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```
Неправильно:

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```
Пустые массивы и словари лучше объявлять так:

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```
А так лучше не делать:

```swift
var names = [String]()
var lookup = [String: Int]()
```
_Примечание: если мы пользуемся описанным стилем объявления переменных, то хорошие и понятные названия переменных играют ещё бо́льшую роль._

**Сокращённый синтаксис для контейнеров**

Надо использовать сокращённые варианты для объявления словарей, массивов и опциональных полей.

Правильно:

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```
Неправильно:

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```
## Управление памятью

В коде не должно быть ретэйн-циклов. Даже если это не боевой код, а какие-то эксперименты.

Чтобы избавиться от ретэйн-циклов, надо:

— использовать поля типа weak и unowned

— использовать структуры и енумы вместо классов

**Увеличение времени жизни объекта**

Иногда нужно, чтобы объект был жив внутри замыкания, даже если все остальные ссылки на него удалены.

Чтобы продлить время жизни объекта, можно использовать связку: сначала говорим `[weak self]`, а после этого — ```guard let _self = self else { return }```:

```swift
resource.request().onComplete { [weak self] response in
  guard let _self = self else {
    return
  }
  let model = _self.updateModel(response)
  self.updateUI(model)
}
```
Если сразу известно, что объект будет жить дольше, чем замыкание, то в замыкании надо использовать `[unowned self]`.

Если не очевидно, кто будет жить дольше, то лучше использовать `[weak self]`.

Кроме связки `[weak self]` + ```guard let _self = self else { return }```, есть два других способа обращаться с объектом внутри замыкания, но они плохие.

**Вот так делать не надо:**

```swift
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```
Объект self может быть удалён из памяти перед вызовом замыкания; в этом случае метод self.updateModel() упадёт.

**Вот так делать тоже не надо:**

```swift
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```
В промежутке между .updateModel() и .updateUI() объект self может быть удалён из памяти. Получится, что self?.updateModel() сработает, а self?.updateUI() — нет.

## Управление доступом

Модификатор управления доступом (private, fileprivate или public) должен стоять первым среди всех остальных модификаторов. Единственное, что может стоять перед ним — модификатор static и атрибуты (например, @IBAction или @IBOutlet).

Правильно:

```swift
class TimeMachine {
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```
Неправильно:

```swift
class TimeMachine {
  lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```
## Циклы

Лучше использовать конструкцию for-in, а не while.

Правильно:

```swift
for _ in 0..<3 {
  print("Hello three times")
}
 
for (index, person) in attendeeList.enumerated() {
  print("\(person) is at position #\(index)")
}
 
for index in stride(from: 0, to: items.count, by: 2) {
  print(index)
}
 
for index in (0...3).reversed() {
  print(index)
}
```
Неправильно:

```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}
 
 
var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```
## Golden path

Не надо плодить вложенные if. Там, где возможно, надо заменять их на конструкцию guard let x = ... else ...

Неправильно:

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
 
  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies
 
      return frequencies
    } else {
      throw FFTError.noInputData
    }
  } else {
    throw FFTError.noContext
  }
}
```
Правильно:

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
 
  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }
 
  // use context and input to compute the frequencies
 
  return frequencies
}
```
Если мы разворачиваем сразу несколько опшионалов, то лучше делать это одной командой:

```swift
guard let number1 = number1,
      let number2 = number2,
      let number3 = number3 else {
  fatalError("impossible")
}
// do something with numbers
```
Вот так — плохо:

```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```
**Failing guards**

В конструкции guard let x = ... else ... блок else ... должен быть устроен просто.

В идеале он должен состоять из единственной команды (return, throw, break, continue или fatalError()).

Если в нескольких гардах требуется выполнить один и тот же код, который что-то чистит, то этот код лучше обернуть в конструкцию defer ... , чтобы избежать дублирования.

## Точки с запятыми

В Свифте не обязательно ставить точку с запятой в конце строки. Точка с запятой нужна только тогда, когда вы объединяете на одной строке несколько выражений.

Но так делать не надо, лучше, чтобы каждое выражение было на своей строке.

Единственное место, где можно использовать точку с запятой — циклы for-conditional-increment.

Но вместо них лучше использовать циклы for-in.

## Скобки в if else

Вокруг условий в if ... else ... скобки не нужны.

Правильно:

```swift
if string.isEmpty {
  print("String is empty.")
}
```
Неправильно:

```swift
if (string.isEmpty) {
  print("String is empty.")
}
```
## Копирайт

В начале каждого нового файла должна стоять шапка с копирайтом:

```swift
//
// Copyright © 2018 Tutu.ru. All rights reserved.
//
```
## Необязательные параметры функций

Для функций и методов класса/структуры и т.п.

*   необязательные параметры должны быть в конце списка аргументов
*   необязательные параметры должны иметь значение по умолчанию (если требуется)  

```swift
func foo(bar: Int, baz: Int? = nil) {
}
```
Для методов протокола

*   необязательные параметры должны быть в конце списка аргументов
*   в экстеншене к протоколу реализованы методы без опциональных параметров (если требуется)  

```swift
protocol Faz {
    func foo(bar: Int, baz: Int?)
}
 
extension Faz {
    func foo(bar: Int) {
        foo(bar: bar, baz: nil)
    }
}
```
