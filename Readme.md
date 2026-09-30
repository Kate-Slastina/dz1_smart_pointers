Проведенные тесты:
## UnqPtr

| № | Функция | Что проверяет |
|---|---|---|
| 1 | `test_unq_basic` | Базовое создание, `operator*`, `operator->`, `get()` |
| 2 | `test_unq_move` | Конструктор перемещения и `operator=` перемещения |
| 3 | `test_unq_self_move` | Самоприсваивание через `std::move` |
| 4 | `test_unq_release_reset` | Методы `release()` и `reset()` |
| 5 | `test_unq_edge_cases` | Граничные случаи: `reset()`/`release()` на nullptr |
| 6 | `test_unq_swap` | Метод `swap()`  |
| 7 | `test_unq_array` | Специализация для массивов `UnqPtr<T[]>`, `operator[]` |
| 8 | `test_unq_array_reset` | `reset()` для массивов |
| 9 | `test_unq_make` | Фабрики `make_unq` и `make_unq_arr` |
| 10 | `test_unq_subtype` | Подтипизация `UnqPtr<Dog>` -> `UnqPtr<Animal>` через move |
| 11 | `test_unq_null` | Работа с `nullptr`, операторы `==`/`!=` с nullptr |
| 12 | `test_unq_custom_deleter` | Кастомный делитель |
| 13 | `test_unq_get_deleter` | Метод `get_deleter()`  |
| 14 | `test_unq_const_correctness` | Const-корректность указателя и объекта |
| 15 | `test_unq_move_only_type` | Работа с move-only типами |
| 16 | `test_unq_explicit_bool` | `explicit operator bool()` |

## ShrdPtr

| № | Функция | Что проверяет |
|---|---|---|
| 17 | `test_shrd_basic` | Базовое создание, копирование, `use_count()` |
| 18 | `test_shrd_move` | Конструктор перемещения |
| 19 | `test_shrd_assignment` | Копирующее и move-присваивание, присваивание `nullptr` |
| 20 | `test_shrd_self_assignment` | Самоприсваивание `p = p` |
| 21 | `test_shrd_swap` | Метод `swap()` |
| 22 | `test_shrd_complex` | Сложные сценарии: множественное копирование, цепочка присваиваний |
| 23 | `test_shrd_subtype` | Подтипизация `ShrdPtr<Dog>` → `ShrdPtr<Animal>` |
| 24 | `test_shrd_array` | Специализация для массивов `ShrdPtr<T[]>` |
| 25 | `test_shrd_make` | Фабрики `make_shrd` и `make_shrd_arr` |
| 26 | `test_shrd_null` | Работа с `nullptr`, операторы `==`/`!=` с nullptr |
| 27 | `test_shrd_custom_deleter` | Кастомный делитель |   
| 28 | `test_shrd_const_correctness` | Const-корректность указателя и объекта |
| 29 | `test_shrd_explicit_bool` | `explicit operator bool()` |


---

### Итого
- **UnqPtr:** 16 тестов
- **ShrdPtr:** 13 тестов
- **Всего:** 29 тестов

Результаты бенчмарков
========================================
  SMART POINTERS BENCHMARK
========================================

>>> N = 10^3 (1000)

Type                  N       Create+Del (ms)   Copy (ms)       Move (ms)       Access (ms)     
------------------------------------------------------------------------------------------------
raw new/delete        10^3    0.07              N/A             N/A             0.00            
my UnqPtr             10^3    0.09              N/A             0.04            0.00            
std::unique_ptr       10^3    0.09              N/A             0.05            0.00            
my ShrdPtr            10^3    0.15              0.01            N/A             0.00            
std::shared_ptr       10^3    0.08              0.01            N/A             0.00            

>>> N = 10^6 (1000000)

Type                  N       Create+Del (ms)   Copy (ms)       Move (ms)       Access (ms)     
------------------------------------------------------------------------------------------------
raw new/delete        10^6    80.68             N/A             N/A             3.52            
my UnqPtr             10^6    67.68             N/A             39.27           1.06            
std::unique_ptr       10^6    66.00             N/A             38.19           3.54            
my ShrdPtr            10^6    144.52            16.66           N/A             2.34            
std::shared_ptr       10^6    81.81             16.07           N/A             3.35            

>>> Запустить тест на 10^9? (y/n): y

>>> N = 10^9 (итеративный, батчи по 10^6)
    Это займёт несколько минут. Подождите...

Type                  Total (ms)        Per 10^6 (ms)     Per op (ns)       
----------------------------------------------------------------------------
  [1/5] raw new/delete...
raw new/delete        84025.9           84.03             84.0              
  [2/5] my UnqPtr...
my UnqPtr             66012.5           66.01             66.0              
  [3/5] std::unique_ptr...
std::unique_ptr       65180.5           65.18             65.2              
  [4/5] my ShrdPtr...
my ShrdPtr            143888.7          143.89            143.9             
  [5/5] std::shared_ptr...
std::shared_ptr       80644.4           80.64             80.6              

========================================
  BENCHMARK COMPLETE
========================================

my_ShrdPtr работает медленнее stl тк по моему варинту ТЗ(3) я релизовала контрол блок и указатель, хранящиеся отдельно в памяти в отличии от STL, где они оба хранятся в одном блоке, из-за чего они экономят одно обращение к памяти при каждом обращении к структуре.

Сырые же медленне смарт указателей из-за ручного удаления в цикле.