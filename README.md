# Проект №9

### т.к 2 семестр еще не закончился, я не имею права загрузить реализацию проекта. Будет выгружен 01.09.2024

Библиотека адаптеров для упрощенной работы с алгоритмами и контейнерами.

Зачастую стоит задача применения нескольких алгоритмов одновременно, например:

```cpp
std::vector<int> v = {1,2,3,4,5,6};
std::vector<int> result;

std::copy_if(v.begin(), v.end(), std::back_inserter(result), [](int i){return i % 2;});
std::transform(result.begin(), result.end(), result.begin(), [](int i){return i * i;});

for(int i : result)
    std::cout << i << " ";
```

Эту задачу можно было бы решить более "элегантно"

```cpp
std::vector<int> v = {1,2,3,4,5,6};

auto removed = v | filter([](int i){return i % 2;});
auto result = removed | transform([](int i){return i * i;});

for(int i : result)
    std::cout << i << " ";
```

или еще более коротко, использовав [конвейер, наподобие того как это принято в unix-системах](https://en.wikipedia.org/wiki/Pipeline_(Unix))

```cpp
std::vector<int> v = {1,2,3,4,5,6};

for(int i : v | filter([](int i){return i % 2;}) | transform([](int i){return i * i;}))
     std::cout << i << " ";
```

Еще одним значимым отличием такого подхода от изначального является то, что вычисления являются ленивыми, а создаваемые объекты не владеют массивом данных для решения данной задача. Подобный подход в частонсти применяется в классах [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view) и [std::span](https://en.cppreference.com/w/cpp/container/span)

### адаптеры

* transform - изменяют значения элементов наподобие того как это делает алгоритм transform
* filter    - фильтрация по определенному признаку, признак передается в качестве аргумента
* take      - берет только N первых элементов
* drop      - пропускаем N первых элементов
* reverse   - реверсия
* keys      - ключи для ассоциативных контейнеров
* values    - значения для ассоциативных контейнеров
