# Неполный Bucket sort O(n)

![Наглядная сортировка ведрами](https://deen3evddmddt.cloudfront.net/uploads/content-images/bucket-sort.webp)

**На картинке видна суть сортировки, наш алгоритм доходит до 2 этапа, после чего мы не сортируем, а храним инфу о мин и макс каждого ведра (при создании размера ведра мы гаранитруем, что между элементами ведра разрыв СРЕДНИЙ, т.е. <= макс). После этого мы просто сравниваем макс элемент ведра с мин элементов следующего и таким образом находим искомый максимальный разрыв.**

``` C++
#include <iostream>

using namespace std;

struct Bucket { // наши ведра будут выглядеть так
    int min_val; // мин в ведре
    int max_val; // макс в ведре
    bool used; // пустое ведро или нет
};

int main() {
    int n;
    cin >> n; // ввод кол-ва элементов

    if (n < 2) {
        cout << 0 << endl; //Если меньше двух элементов - разрыв 0
        return 0;
    }

    int* arr = new int[n];
    for (int i = 0; i < n; i++) {
        cin >> arr[i]; // Заполняем массив
    }

    int min_val = 1000000000; // мин сначала больше макс. возможного
    int max_val = -1; // макс сначала меньше мин. возможного
    for (int i = 0; i < n; i++) {
        if (arr[i] < min_val) min_val = arr[i];
        if (arr[i] > max_val) max_val = arr[i]; // Ищем макс и мин в массиве
    }
    if (min_val == max_val) {
        cout << 0 << endl;
        delete[] arr; // Если макс = мин то все элементы равны -> разрыв 0
        return 0;
    }

    int bucket_size = (max_val - min_val) / (n - 1); // Делаем ведра размером (макс - мин) / мощность массива - 1
    if (bucket_size == 0) bucket_size = 1; // Исключаем пустые ведра
    int bucket_count = (max_val - min_val) / bucket_size + 1; // Вычисляем кол-во ведер как (макс - мин) / размер + 1 (+ 1 для округления вверх на случай неполного последнего ведра)

    Bucket* buckets = new Bucket[bucket_count];
    for (int i = 0; i < bucket_count; i++) { // Создаем массив ведер, каждое из которых имеет 3 хар-ки
        buckets[i].used = false; // не пустое ли
        buckets[i].min_val = 1000000000;  // мин элемент больше макс. возможного
        buckets[i].max_val = -1; // макс элемент меньше мин. возможного
    }

    for (int i = 0; i < n; i++) { // Начинаем проход по массиву
        int bucket_index = (arr[i] - min_val) / bucket_size; // определяем в какое ведро попадет элемент как (элемент - мин) / размер
        if (!buckets[bucket_index].used) {
            buckets[bucket_index].used = true; // если ведро было пустым то теперь нет
            buckets[bucket_index].min_val = arr[i]; // записываем первый элемент ведра как макс и мин
            buckets[bucket_index].max_val = arr[i];
        } else { // если ведро не пустое то проверяем является ли элемент макс/мин в ведре
            if (arr[i] < buckets[bucket_index].min_val) buckets[bucket_index].min_val = arr[i];
            if (arr[i] > buckets[bucket_index].max_val) buckets[bucket_index].max_val = arr[i];
        }
    }

    int max_gap = 0; // наш будущий ответ
    int prev_max = min_val;  // то с чем сравниваем первое ведро
    for (int i = 0; i < bucket_count; i++) { // начинаем проход по массиву ведер
        if (!buckets[i].used) continue; //если пустое скип
        int gap = buckets[i].min_val - prev_max; // если не пустое то смотрим на дырку между ведрами
        if (gap > max_gap) { // если дырка (мин этого - макс прошлого) больше ответа
            max_gap = gap; //то теперь это наш ответ
        }
        prev_max = buckets[i].max_val; // записыавем макс элемент для сравнения с некст ведром
    }

    cout << max_gap << endl; // выводим ответ
    delete[] arr; // подчищаем дин память (в целом не особо важно)
    delete[] buckets; // подчищаем дин память (в целом не особо важно)
    return 0;
}

```

# Код которым тестил на Python

``` python
from random import*
n = 10**5
print(n)
a=[]
maxx=0
for i in range(0, n):
    b = randint(0,n)
    print(b,end=' ')
    a.append(b)
a = sorted(a)
for i in range(0, len(a)-1):
    maxx = max(maxx,abs(a[i+1] - a[i]))
print()
print(maxx)
```
