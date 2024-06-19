# Golang Base

## Build Project

```bash 
 go build -ldflags="-s -w" -o appName;
```

<hr/>

## Golang Linter

- установка https://golangci-lint.run/welcome/install/
- проверка версии `golangci-lint --version`

```bash 
golangci-lint run
```

<hr/>

## Встроеные Функции

### MAKE

Ключевое слово <span style="color:red">__MAKE__</span>  используется для создания и инициализации срезов (slices),
карт (maps) и каналов (channels). Оно возвращает инициализированное значение этих типов, готовое к использованию.

```
a := make([]int, 5)
m := make(map[string]int) 
c := make(chan int)
```

### NEW

Ключевое слово <span style="color:red">__NEW__</span>  используется для выделения памяти и создания указателя на тип, но
не инициализирует память, кроме как для установки нулевых значений. Оно возвращает указатель на вновь выделенную память.

```
p := new(int) // p это указатель на int, значение которого равно 0
--------------------------------------------------------------------------------

type Person struct {
    Name string
    Age  int
}
p := new(Person) // p это указатель на Person, с нулевыми значениями полей
```

### FOR

```
for i := 0; i < len(arr); i++ { }
```

```
for i := 0; i < 10; i++ { }
```

```
for key, value := range map { }
```

```
for index, value := range arr {	}
```

<hr/>

## Short IF

``` 
if x := rand.Intn(18); x < 9 {
  fmt.Println(x)
}
```

<hr/>

## Switch fallthrough без брейк

``` 
switch number := rand.Intn(3); number {
case 0:
	fmt.Println("Zero")
	fallthrough
case 1:
	fmt.Println("One")
	fallthrough
case 2:
	fmt.Println("Two")
}
```

<hr/>

## Замыкание в функции

``` 
fClosure := closure()

fmt.Println(fClosure(10)) //10
fmt.Println(fClosure(10)) //20

func closure() func(int) int {
    sum := 0
    return func(v int) int {
        sum += v
        return sum
    }
}
```

<hr/>

## Передача Неизвестного количества аргументов функции

``` 
add(1, 2, 3, 4, 5, 6)

func add(values ...int) int {
    sum := 0
    for _, value := range values {
	    sum += value
    }
    return sum
}
```

<hr/>

## Массив

Длинна массива обязательный параметр.<br/>
Значение по умолчанию у массива это значение типа по умолчанию<br/>
Для автоматического подсчёта длинны массивы используеюся  <span style="color:red">__[...]__</span>.<br/>
`var arr = [...]int{1, 2, 3, 4}`

Объявление массива

- `var arr[4]int`
- `var arr = [4]int{1,2,3,4}`
- `arr := [4]int{1,2,3,4}`

Объявление многомерный массив массива

- `    arr := [...][4]int{
  {1, 2, 3, 4},
  {5, 6, 7, 8},
  }`

<hr/>

## Слайс

Слайc имеет capacity и длину<br/>
Значение по умолчанию у сайлса nil

Объявление слайса

- `var slice []int`
- `slice2 := make([]int, 5)`
- `slice1 := []int{1, 2, 3}`

Копирование слайсов

```
slice1 := []int{1, 2, 3}
slice2 := make([]int, len(slice1))
copy(slice2, slice1)
```

<hr/>

## MAP

Значение по умолчанию nil <br/>
Что бы создать пустую мапу нужно использовать MAKE <br/>
При обращении к несуществующему ключу получим дефолтное значение <br/>
При пробежке циклом, порядок ключей не соблюдается

Объявление map

- `var m = make(map[string]int)`
- `m := make(map[string]int)`
- `m := map[string]int{"hi": 1112}`
- `m := map[string][]int{"0": {1, 2, 3}}`
- `m := map[string][]map[string]int{"0": {{"2": 1}}}`

Проверка наличия ключа в мап

```
m := map[string]int{"a": 1, "b": 2, "c": 3}
x, ok := m["a"]
fmt.Println("x:", x, "ok:", ok)
```

Удаление ключа из мап

```
delete(m, "key")
```

<hr/>

## Строки

Неизменяемый тип <br/>
Для изменения строки, нужно представить её в виде слайса рун и уже потом менять<br/>

Изменение строки

```
str := "тест"
newStr := []rune(str)
newStr[0] = '1'
```

Поиск подстроки в строке

```
s := "hello world"
if strings.Contains(s, "world") {
  fmt.Println("Found substring 'world'")
}
```

Разделение строки на масив

```
s := "a,b,c,d,e"
parts := strings.Split(s, ",")
```

Замена подстроки

```
s := "hello world"
newS := strings.Replace(s, "world", "Go", 1)
```

Правильный цикл строки

```
s := "Привет Го"
runes := []rune(s)
for i, r := range runes {
  fmt.Println(i, string(r))
}
```

<hr/>

## Каналы

Канал бывает буферизированый и небуферизированный <br/>

- Небуферизированные каналы: передача данных блокирует отправителя до тех пор, пока получатель не примет данные.
- Буферизированные каналы: передача данных не блокируется, если есть свободное место в буфере.

Значение по умолчанию nil <br/>
Читать из закрытого канала можно <br/>
Писать в закрытый канал нельзя <br/>
В канал всегда должен кто то писать и кто то читать из его! <br/>

Объявление канала

```
chanInit := make(chan string) // ссылка на канал
chanInit := make(chan string, 3) // ссылка на , буферизированый канал
```

Каналы могут быть сделаны однонаправленными, что ограничивает их использование только для отправки или получения данных.

```
func sendOnly(ch chan<- int) {
  ch <- 42 // только отправка
}

func receiveOnly(ch <-chan int) {
  value := <-ch // только получение
}
```

Для получения данных из канала __<- ch__

```
data:= <- ch
```

Для отправки данных в канал __ch <-__

```
ch <- value
```

Закрытие канала

```
close(ch)
```

Проверка на закрытие канала

```
_, ok := <-ch
```

Итерация по каналу

```
for value := range ch {}
```

## Select

Конструкция select позволяет работать с несколькими операциями над каналами одновременно, блокируясь до тех пор, пока не
станет доступной одна из них.

```
ch1 := make(chan int)
ch2 := make(chan int)

go func() {
  time.Sleep(time.Second)
  ch1 <- 1
}()

go func() {
  time.Sleep(2 * time.Second)
  ch2 <- 2
}()

select {
case value := <-ch1:
  fmt.Println("Received from ch1:", value)
case value := <-ch2:
  fmt.Println("Received from ch2:", value)
case <-time.After(3 * time.Second):
  fmt.Println("Timeout")
}

```

<hr/>

## sync.WaitGroup

Структура, используемая для ожидания завершения группы горутин. Она предоставляет простой и безопасный способ
синхронизации нескольких горутин, обеспечивая, что основная горутина (или другая горутина) может дождаться завершения
набора горутин, прежде чем продолжить выполнение.<br/>

<span style="color:red"> Не может быть использован повторно после вызова Wait.</span> Для новой группы горутин нужно использовать новый экземпляр WaitGroup.<br/>

Инициализация

```
var wg sync.WaitGroup
```

Добавление горутин в счетчик WaitGroup: Используйте метод Add для увеличения счетчика на количество горутин, которые вы
собираетесь запустить.

```
wg.Add(2)
```

Пометка завершения горутин: Каждая горутина вызывает метод Done при завершении своей работы, что уменьшает счетчик на
единицу.

```
wg.Done()
```

Ожидание завершения всех горутин: Основная горутина (или другая горутина) вызывает метод Wait, который блокируется до
тех пор, пока счетчик не станет равен нулю.

```
wg.Wait()
```

Нельзя копировать и внутрь функции всегда передаётся по указателю

```
var wg sync.WaitGroup
wg.Add(1)
myFunc(&wg)

func myFunc(wg *sync.WaitGroup)  {
  wg.Done()
}
wg.Wait()
```

<hr/>

## sync.Mutex/sync.RWMutex

Mutex используется для обеспечения эксклюзивного доступа к разделяемым ресурсам и предотвращения гонок данных. Мьютексы
позволяют безопасно изменять общие данные из нескольких горутин, блокируя доступ к этим данным для других горутин на
время выполнения критической секции кода.<br/>

Основные методы Mutex

- __Lock:__ Захватывает мьютекс, блокируя доступ другим горутинам. Если мьютекс уже захвачен, горутина будет ждать, пока
  мьютекс не освободится. <br/>
- __Unlock:__ Освобождает мьютекс, позволяя другим горутинам захватить его. <br/>

```
var (
  counter int
  mutex   sync.Mutex
)

func increment(wg *sync.WaitGroup) {
  defer wg.Done()
  mutex.Lock()
  defer mutex.Unlock()
  counter++
}

func main() {
  var wg sync.WaitGroup

  for i := 0; i < 1000; i++ {
    wg.Add(1)
    go increment(&wg)
  }

  wg.Wait()
  fmt.Println("Final counter:", counter)
}
```

Для повышения производительности при частом чтении и редком изменении данных можно использовать sync.RWMutex, который
позволяет одновременно выполнять несколько операций чтения, но блокирует запись.

```
var (
  counter int
  rwMutex sync.RWMutex
)

func readCounter(wg *sync.WaitGroup) {
  defer wg.Done()
  rwMutex.RLock()
  defer rwMutex.RUnlock()
  fmt.Println("Counter value:", counter)
}

func writeCounter(wg *sync.WaitGroup, value int) {
  defer wg.Done()
  rwMutex.Lock()
  defer rwMutex.Unlock()
  counter = value
}

func main() {
  var wg sync.WaitGroup

  // Запуск горутин для чтения и записи
  for i := 0; i < 5; i++ {
    wg.Add(1)
    go readCounter(&wg)
  }

  wg.Add(1)
  go writeCounter(&wg, 42)

  wg.Wait()
}
```

## sync.Map

Значение по умолчанию не nil

Нельзя копировать

Тип данных, предоставляющий конкурентный (безопасный для использования в многопоточной среде) ассоциативный массив (или
словарь).<br/>

- __Безопасность для использования в многопоточной среде:__ sync.Map безопасен для использования в конкурентных средах,
  что означает, что несколько горутин могут одновременно читать, писать или изменять карту без дополнительной
  синхронизации.
- __Оптимизация для частых чтений и редких записей:__ Этот тип данных особенно эффективен, когда частые операции чтения
  сочетаются с редкими операциями записи, благодаря использованию структур, оптимизированных для таких случаев.

Инициализация

```
var m sync.Map
```

Основные методы sync.Map:

Сохранить значение по заданному ключу. Если значение с таким ключом уже существует, оно будет перезаписано.

```
Store(key, value interface{})
```

Вернуть значение, связанное с ключом, и булево значение, указывающее, найдено ли оно. Если ключ не найден, ok будет
false.

```
Load(key interface{}) (value interface{}, ok bool)
```

Вернуть значение если оно уже существует, оно возвращается, и loaded будет true. Если значения нет, оно сохраняется и
возвращается, и loaded будет false.

```
LoadOrStore(key, value interface{}) (actual interface{}, loaded bool)
```

Удалить ключ и его значение из карты

```
Delete(key interface{})
```

Выполняет функцию для каждого элемента в карте. Если функция возвращает false, перебор прерывается.

```
Range(f func(key, value interface{}) bool)
```

Примеры

```
func main() {
    var m sync.Map

    // Сохранение значения
    m.Store("foo", "bar")

    // Загрузка значения
    value, ok := m.Load("foo")
    if ok {
        fmt.Println("Found value:", value)
    } else {
        fmt.Println("Value not found")
    }

    // Удаление значения
    m.Delete("foo")

    // Попытка загрузки удаленного значения
    value, ok = m.Load("foo")
    if ok {
        fmt.Println("Found value:", value)
    } else {
        fmt.Println("Value not found")
    }

    // Сохранение значения с LoadOrStore
    actual, loaded := m.LoadOrStore("baz", "qux")
    if loaded {
        fmt.Println("Existing value:", actual)
    } else {
        fmt.Println("Stored value:", actual)
    }

    // Перебор всех элементов
    m.Store("key1", "value1")
    m.Store("key2", "value2")
    m.Range(func(key, value interface{}) bool {
        fmt.Println(key, value)
        return true // продолжить перебор
    })
}
```

<hr/>

## Type switch

Type switch (тайп свитч) используется для выполнения различных действий в зависимости от конкретного типа значения
интерфейса. Это расширение обычного switch и полезный инструмент для работы с интерфейсами, когда тип значения
становится известен только во время выполнения.

```
var i interface{} = "hello"

switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
case bool:
    fmt.Printf("Boolean: %t\n", v)
default:
    fmt.Printf("Unknown type\n")
}
```

<hr/>

## Тесты

Файлы тестов должны оканчиваться на _test.go

Запуск тестов

```
go test // Запуск тестов в текущем пакете
```

```
go test ./...   // Запуск тестов во всех подкаталогах проекта
```

```
go test ./... -count 1   // Запуск количества тестов во всех подкаталогах проекта, убирает кеш
```

```
go test -v ./...  // Запуск тестов с подробным выводом
```

Стандартная сигнатура теста:

```
func TestMySum(t *testing.T) {
	values := my_sum.MySum(1, 2)
	expected := 3
	if values != expected {
		t.Fail()
	}
}
```

Табличный тест

```
func TestTableMySum(t *testing.T) {
	testCases := map[string]struct {
		a      int
		b      int
		result int
	}{
		"sum equal": {
			a:      10,
			b:      10,
			result: 20,
		},
		"sum with zero": {
			a:      0,
			b:      15,
			result: 15,
		},
	}
	for _, v := range testCases {
		got := my_sum.MySum(v.a, v.b)
		if got != v.result {
			t.Errorf("Expected %d but got %d", v.result, got)
		}
	}
}
```

### FUZZ test

Подставляет сам рандомные значения <br/>

Запуск FUZZ теста

```
go test -fuzz=FuzzTestMySum internal/my_sum/my_sum_test.go
```

Пример

```
func MySum(a, b int) int {
  if b == 10 {
    return 0
  }
  return a + b
}

func FuzzTestMySum(f *testing.F) {
  f.Fuzz(func(t *testing.T, a, b int) {
    res := my_sum.MySum(a, b)
    if b == 10 {
      if res != 0 {
        t.Errorf("a=%d, b=%d, res=%d, expected=%d", a, b, res, 0)
      }
    } else {
      if res != a+b {
        t.Errorf("a=%d, b=%d, res=%d, expected=%d", a, b, res, a+b)
      }
    }
  })
}
```

<hr/>

## Context

Инструмент для управления сроками выполнения, отмены и передачи метаданных между goroutine. Он часто используется в
асинхронных операциях и при работе с сетевыми запросами, базами данных и другими ресурсами. Рассмотрим основные
особенности и концепции, связанные с context

Это интерфейс определенный в пакете context и включает следующие методы:

```
type Context interface {
    Deadline() (deadline time.Time, ok bool)  // Возвращает дедлайн (если есть)
    Done() <-chan struct{}                   // Канал, закрываемый при завершении контекста
    Err() error                              // Возвращает ошибку, если контекст завершен
    Value(key interface{}) interface{}       // Возвращает значение по ключу
}
```

###### Получить значение из контекста
```
ctxBackground := context.Background()
value := ctxBackground.Value("qwer")
```

### Типы контекстов

1. `context.Background()` - Используется в качестве корневого контекста. Часто используется в основном goroutine или в тестах.
```
ctx := context.Background()
```
<br/>


2. __`context.TODO()`__  -  Используется, когда контекст пока неизвестен или еще не определен.
```
ctx := context.TODO()
```
<br/>

3. __`context.WithCancel`__  -  Создает контекст, который можно отменить вручную.

```
ctx, cancel := context.WithCancel(context.Background())
defer cancel() // Важно вызвать cancel для предотвращения утечки ресурсов

// Пример использования
go func() {
    // Выполняем некоторую работу
    time.Sleep(1 * time.Second)
    cancel() // Отменяем контекст
}()

<-ctx.Done() // Ждем завершения контекста
fmt.Println("Контекст отменен:", ctx.Err())
 ```
<br/>

4. __`context.WithTimeout`__  - Создает контекст, который автоматически отменяется через заданный промежуток времени.
```
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()

select {
case <-time.After(1 * time.Second):
    fmt.Println("Работа завершена")
case <-ctx.Done():
    fmt.Println("Контекст отменен:", ctx.Err())
}
```
<br/>

5. __`context.WithDeadline`__  - Создает контекст, который автоматически отменяется в определенное время.
```
deadline := time.Now().Add(2 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
defer cancel()

select {
case <-time.After(1 * time.Second):
    fmt.Println("Работа завершена")
case <-ctx.Done():
    fmt.Println("Контекст отменен:", ctx.Err())
}
```

6. __`context.WithValue`__  - Используется для передачи данных по ключу между goroutine. Это может быть полезно для передачи идентификаторов запроса или аутентификационной информации.

__Создаёт новый контекст на основе существующего__

```
type contextKey string

const key contextKey = "key"

ctx := context.WithValue(context.Background(), key, "value")

value := ctx.Value(key)
fmt.Println("Значение из контекста:", value)
```
Часто при помощи его прокидывается логер
```
ctx = context.WithValue(ctxBackground, "logger", logger)
```


##### Использование context в goroutine и функциях

```
func doWork(ctx context.Context) {
    select {
    case <-time.After(1 * time.Second):
        fmt.Println("Работа завершена")
    case <-ctx.Done():
        fmt.Println("Контекст завершен:", ctx.Err())
    }
}

func main() {
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    go doWork(ctx)

    time.Sleep(500 * time.Millisecond)
    cancel() // Отменяем контекст раньше времени
}
```

Практические советы
1. __Всегда вызывайте cancel()__: Убедитесь, что cancel() вызывается для предотвращения утечек ресурсов.
2. __Используйте контексты для отмены и тайм-аутов__: Это помогает сделать код более управляемым и предсказуемым.
3. __Не используйте контексты для передачи данных__: Контексты предназначены для управления сроками выполнения и отмены, а не для передачи данных между goroutine.

<hr/>


## Флаги командной строки (Параметры при запуске скрипта)
```
strFlag := flag.String("name", "default", "a string flag")
intFlag := flag.Int("age", 0, "an int flag")
boolFlag := flag.Bool("married", false, "a bool flag")
flag.Parse()
fmt.Println("Name:", *strFlag)
fmt.Println("Age:", *intFlag)
fmt.Println("Married:", *boolFlag)
```

##### Позиционные аргументы: после вызова flag.Parse() можно получить доступ к позиционным аргументам через flag.Args() и flag.NArg().
```
 flag.Parse()
    fmt.Println("Remaining args:", flag.Args())
    fmt.Println("Number of remaining args:", flag.NArg())
}
```

<hr/>

## SLOG (логирование)
Важно создавать логер в приложении подобной конструкцией

TEXT вывод
```
opts := &slog.HandlerOptions{
    Level: slog.LevelInfo,
}
logger := slog.New(slog.NewTextHandler(os.Stdout, opts))
```
или
JSON вывод
```
opts := &slog.HandlerOptions{
    Level:     slog.LevelInfo,
    AddSource: false,
}
logger := slog.New(slog.NewJSONHandler(os.Stdout, opts))
logger.Info("222", slog.String("version", "2222"))

```