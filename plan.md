## Rvalue-references
1. Копирование объектов при возврате из функции
2. Return value optimization (RVO)
3. Named return value optimization (NRVO)
4. Копирование объектов при получении параметра функции по значению
5. Понятие lvalue и rvalue
6. Value category у аргумента и у результата у встроенных операторов (=, +=, +, ++, &, *)
7. Проблема хранения в контейнерах non-copyable объектов и объектов с дорогим копированием
8. Rvalue-references
9. Соглашения по инспользованию rvalue-references, move-семантика
10. `std::move`
    * новое поведение `std::swap`
    * тонкость с реализацией move-присваивание через swap, поскольку swap реализован через move
    * требования на move-присваивание от `*this`
    ! куда впихнуть && for *this?
    ! noexcept?
    ! xvalue/prvalue, продление времени жизни при бинде к rvalue-ссылке?
11. сравнение с destructive move

## Perfect-forwarding
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
    ! почитать http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3276.pdf
9. `std::declval` (он тут нужен, или рассказать о нем в [секции "Metaprogramming"](#metaprogramming)?)
10. Правила поиска имен, trailing return type
11. `nullptr`

## Вывод типов и анонимные функции
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
    ! нужно ли до этого рассказать SFINAE? (_а причем тут SFINAE?_)

## Патерн type-erasure, runtime и compile-time полиморфизм
1. `std::function`
2. `any_iterator`
4. `any_range`
3. `std::any` (C++17)
5. Boost.TypeErasure
5. `std::mem_fn`
    ! почитать http://cplusplus.github.io/LWG/lwg-defects.html#2048
6.` std::bind`
    * полезность при необходимости скопировать захватываемые данные
7. Runtime и compile-time полиморфизм

## Small-object optimization and misc vocabulary types
1. Small-object optimization
    * для `std::string`
    * для `std::function`
2. `std::optional` (С++17)
    ! включить сюда рассуждения про важность сохранения инварианта класса
      и как хорошо, когда часть инварианта можно закодировать в типе
3. `std::aligned_storage`
4. Управление выравниванием (alignof, alignas)
5. `std::variant` (C++17)
6. `std::tuple`
    * сравнение tuple со структурами
    ! structured binding (C++17)?

## Всякие полезности при написании классов
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

## Умные указатели
1. `unique_ptr`
    * unique_ptr pimpl?
2. `shared_ptr`
    * custom deleter
    * shared_ptr на члены-класса
    * внутреннее устройство `shared_ptr`
    * `std::make_shared`
    * `weak_ptr`
    * замечание о том, что `weak_ptr::expired` racy и надо использовать `weak_ptr::lock`
    * коварный `std::enable_shared_for_this` -- отнаследуешься private и схлопочешь null pointer в runtime
    * *_pointer_cast
    
## Metaprogramming
1. Template type aliases
2. Template variables
3. Static assertions
4. Expression SFINAE
5. Std type traits
6. Constexpr functions
   * Разница с точки зрения компилятора между вычисление constexpr functions и variables -- значения последних кэшируются
7. Пример использования -- вычисление аргумента `noexcept`
8. Бонусы
   * `std::void_t`
   * `if constexpr`
   * fold expressions

## Uniform initialization
### Какие задачи решали?
* инициализация контейнеров так же кратно как встроенных массивов -- initializer lists
* инициализация POD-структур как в С -- aggregate initialization
  * designated initializers (C++2a)
* неоднозначность грамматики: function delcaration / direct initialization
### Что получилось?
* инициализация контейнеров -- но ценой сложного overload resolution `vector(1, 2)` и `vector{1, 2}` дают разный результат.
* инициализация POD-структур -- но не работает с forwarding-функциями (emplace, make, ...)
* использовать {} при инициализации всегда -- работает, но очень сложно, куча puzzlers (мой любимый из свежего -- `vector<string> ss {{"aba", "caba"}}`). Нетривиальное взаимодействие с `auto`.

## Всякая жуть и муть:
* Range-based for loop
    * `auto &&`
    * проблема итерации по контейнеру без &
* Внешнии функции в интерфейсе контейнера
    * `std::begin`
    * `std::end`
    * `std::size`
* Undefined behavior
* Упомянуть про emplace, emplace_{front,back}
* Initialization of class objects by rvalues  
* Right angle brackets
* Default template arguments for function templates
* Extern templates
* Generalized attributes
* Generalized constant expressions
* New character types
* Unicode string literals
* Raw string literals
* Universal character name literals
* User-defined literals
* Standard Layout Types
* Extending sizeof
* Inline namespaces
* Unrestricted unions
