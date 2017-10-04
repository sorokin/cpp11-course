## Rvalue-references (2 пары)
1. Копирование объектов при возврате из функции
2. Return value optimization (RVO)
3. Named return value optimization (NRVO)
4. Копирование объектов при получении параметра функции по значению
5. Понятие lvalue и rvalue
6. Value category у аргумента и у результата у встроенных операторов (=, +=, +, ++, &, *)
7. Проблема хранения в контейнерах non-copyable объектов и объектов с дорогим копированием
8. Rvalue-references
9. Соглашения по использованию rvalue-references, move-семантика
10. `std::move`
    * новое поведение `std::swap`
    * тонкость с реализацией move-присваивание через swap, поскольку swap реализован через move
    * требования на move-присваивание от `*this`
    * rvalue references for *this
    * noexcept
    * xvalue/prvalue, продление времени жизни при бинде к rvalue-ссылке
11. сравнение с destructive move

## Perfect-forwarding (2 пары)
1. Проблема perfect-fowarding
2. Вывод параметров шаблона при использовании rvalue-ссылок
3. Reference-collapsing rule
4. `std::forward`
5. Variadic templates
    * expand сразу двух pack'ов
    * для template template параметров http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2555.pdf
6. Примеры использования perfect forwarding: `emplace`, `emplace_{front,back}`, `make_{unique,shared,tuple}`
7. Возврат результата в при perfect-fowarding
8. decltype
    * http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3276.pdf
9. `std::declval`
10. Правила поиска имен, trailing return type
11. `nullptr`

## Вывод типов и анонимные функции (1 пара)
1. auto
    * вывод типа в случае наличия нескольких деклараторов
    * вывод при наличии ptr/ref деклараторов
    * вывод с rvalue-ссылками
    * различия вывода auto и параметра шаблона
    * `decltype(auto)` (C++14)
2. Анонимные функции
    * Local and unnamed types as template arguments
3. Capture список в анонимных функциях
    * Generalized lambda capture C++14
    * проблема захвата this, в конструкторе, если параметр и
      член-данных называются одинаково
4. Свойства анонимных функций
    * копируемость
    * присваиваемость
    * move-амость
    * конверсия в указатель на функцию
5. Вывод типа возвращаемого значения в лямбдах и в обычных функциях C++14
    * в темплейтных функциях
    * упомянуть про SFINAE

## Патерн type-erasure, runtime и compile-time полиморфизм (1 пара)
1. `std::function`
2. `any_iterator`
4. `any_range`
3. `std::any` (C++17)
5. Boost.TypeErasure
5. `std::mem_fn`
    * http://cplusplus.github.io/LWG/lwg-defects.html#2048
6. `std::bind`
    * полезность при необходимости скопировать захватываемые данные
7. Runtime и compile-time полиморфизм

## Small-object optimization and misc vocabulary types (1 пара)
1. Small-object optimization
    * для `std::string`
    * для `std::function`
2. Copy-on-write optimization
    * для строк в GCC до C++11
2. `std::optional` (С++17)
    ! включить сюда рассуждения про важность сохранения инварианта класса
      и как хорошо, когда часть инварианта можно закодировать в типе
3. Unrestricted unions
3. `std::aligned_storage`
4. Управление выравниванием (alignof, alignas)
5. `std::variant` (C++17)
6. `std::tuple`
    * сравнение tuple со структурами
    * structured binding (C++17)
    * изменение value-category при return переменных при structured binding'е

## Всякие полезности при написании классов (1 пара)
1. Explicit override control, final classes
    * указать, что это context-sensitive keyword
2. Defaulted and deleted functions
    * для конструкторов
    * пример с `std::ref`/`std::cref`
3. Delegating constructors
    * уточнение когда вызывается деструктор
4. Inheriting constructors
    * разные правила для C++11 и C++17
5. Explicit conversion operators
6. Extended friend declarations
7. Non-static data member initializers
8. Defining move special member functions
9. Strongly-typed enums
10. Forward declarations for enums
11. Inline namespaces
12. Nested namespace definition

## Умные указатели (1 пара)
1. `unique_ptr`
2. `shared_ptr`
    * custom deleter
    * shared_ptr на члены-класса
    * внутреннее устройство `shared_ptr`
    * `std::make_shared`
    * `weak_ptr`
    * замечание о том, что `weak_ptr::expired` racy и надо использовать `weak_ptr::lock`
    * pitfall: private inheritance from `std::enable_shared_for_this`
    * *_pointer_cast
3. pimpl используя `unique_ptr` и `shared_ptr`

## Uniform initialization (1 пара)
* Решаемые задачи
    * инициализация контейнеров так же кратно как встроенных массивов -- initializer lists
    * инициализация POD-структур как в С -- aggregate initialization
    * designated initializers (C++2a)
    * неоднозначность грамматики: function delcaration / direct initialization
* Инициализация контейнеров
    * разный результат для `vector(1, 2)` и `vector{1, 2}`
* Инициализация POD-структур
    * не работает с forwarding-функциями (emplace, make, ...)
* Использование {} для инициализации
    * `vector<string> ss {{"aba", "caba"}}`
    * Нетривиальное взаимодействие с `auto`

## Metaprogramming (2 пары)
1. Template type aliases
2. Template variables
3. Самый эффективный в смысле compilation time способ отсечься по SFINAE -- default template arguments, теперь и для function templates
4. Static assertions
5. Expression SFINAE
6. Std type traits
7. Constexpr functions
   * Разница с точки зрения компилятора между вычислением constexpr functions и variables -- значения последних кэшируются
8. Пример использования -- вычисление аргумента `noexcept`
9. Бонусы
   * `std::void_t`
   * `if constexpr`
   * fold expressions
10. Явное инстанцирование темплейтов и подавление инстанцирования

## Расширения стандартной библитеки (2 пары)
* emplace и move semantics везде, даже в std::pair
* transparent comparator и heterogeneous lookup
* std::filesystem -- вот есть такой, очень кратко
* std::chrono -- вот есть такой, очень кратко + UDL (стандартные и как писать свои)
* `string_view`
   * конструирование от temporary string
   * view на смуванную small-строку
   * забавный курьез: из перегрузок `foo(bool)` и `foo(string_view)` при вызове `foo("aba")` выберется первая, решение -- `foo("aba"sv)`
* unordered containers и специализация `std::hash` для своей структуры
   * согласованность с `operator ==`
   * пессимизация из-за забытого `noexcept`
* Внешнии функции в интерфейсе контейнера
    * `std::begin`
    * `std::end`
    * `std::size`
* Range-based for loop
    * `auto &&`
    * проблема итерации по контейнеру без &
* Ranges
* не владеющие типы `reference_wrapper`, `string_view`
* когда передавать параметры функции по ссылке, а когда по значению
* реализуем vector::emplace_back
* реализуем vector::insert(range)
* реализуем range adaptor правильно (как в range-v3, а не как в boost.range)

## Undefined behavior (1 пара)
1. Пример с sum
2. Пример с memcpy
3. Ключевое слово restrict
4. Понятие undefined behavior
5. Виды undefined behavior
    * для целых чисел
    * для указателей
    * strict aliasing rule
6. Sanitizers
