# Radix sort O(kn)

У меня на ноуте быстро работает при n = 10^5 но не уверен что зачтут за линейное время 

``` C++
#include <iostream>


// Функция для нахождения максимального элемента в массиве
int findMax(int arr[], int n) {
    int max_val = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > max_val) {
            max_val = arr[i];
        }
    }
    return max_val;
}

// Counting Sort для конкретного разряда
void countingSortForRadix(int arr[], int n, int exp) {
    int* output = new int[n];
    int count[10] = {0};

    // Подсчитываем количество вхождений каждой цифры
    for (int i = 0; i < n; i++) {
        int digit = (arr[i] / exp) % 10;
        count[digit]++;
    }

    // Преобразуем count в позиции
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    // Строим выходной массив (идем с конца для стабильности)
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    // Копируем отсортированный массив обратно
    for (int i = 0; i < n; i++) {
        arr[i] = output[i];
    }

    delete[] output;
}

// Основная функция Radix Sort LSD
void radixSortLSD(int arr[], int n) {
    if (n <= 1) return;

    int max_val = findMax(arr, n);

    // Сортируем для каждого разряда
    for (int exp = 1; max_val / exp > 0; exp *= 10) {
        countingSortForRadix(arr, n, exp);
    }
}

// Решение исходной задачи
int main() {
    int n;
    std::cin >> n;

    if (n < 2) {
        std::cout << 0 << std::endl;
        return 0;
    }

    int days[n];

    for (int i = 0; i < n; i++) {
        std::cin >> days[i];
    }

    radixSortLSD(days, n);

    int max_gap = 0;
    for (int i = 1; i < n; i++) {
        int gap = days[i] - days[i - 1];
        if (gap > max_gap) {
            max_gap = gap;
        }
    }

    std::cout << max_gap << std::endl;


    return 0;
}
```

# Bucket sort O(n?)

Другая нейронка говорит что это лучше и ровное O(n), хотя bucketsort не оч понятный мне пока что

``` C++
#include <iostream>

using namespace std;

struct Bucket {
    int min_val;
    int max_val;
    bool used;
};

int main() {
    int n;
    cin >> n;

    if (n < 2) {
        cout << 0 << endl;
        return 0;
    }

    int* arr = new int[n];
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    int min_val = 2147483647;
    int max_val = -2147483648;
    for (int i = 0; i < n; i++) {
        if (arr[i] < min_val) min_val = arr[i];
        if (arr[i] > max_val) max_val = arr[i];
    }
    if (min_val == max_val) {
        cout << 0 << endl;
        delete[] arr;
        return 0;
    }

    int bucket_size = (max_val - min_val) / (n - 1);
    if (bucket_size == 0) bucket_size = 1;
    int bucket_count = (max_val - min_val) / bucket_size + 1;

    Bucket* buckets = new Bucket[bucket_count];
    for (int i = 0; i < bucket_count; i++) {
        buckets[i].used = false;
        buckets[i].min_val = 2147483647;
        buckets[i].max_val = -2147483648;
    }

    for (int i = 0; i < n; i++) {
        int idx = (arr[i] - min_val) / bucket_size;
        if (!buckets[idx].used) {
            buckets[idx].used = true;
            buckets[idx].min_val = arr[i];
            buckets[idx].max_val = arr[i];
        } else {
            if (arr[i] < buckets[idx].min_val) buckets[idx].min_val = arr[i];
            if (arr[i] > buckets[idx].max_val) buckets[idx].max_val = arr[i];
        }
    }

    int max_gap = 0, prev_max = min_val;
    for (int i = 0; i < bucket_count; i++) {
        if (!buckets[i].used) continue;
        int gap = buckets[i].min_val - prev_max;
        if (gap > max_gap) max_gap = gap;
        prev_max = buckets[i].max_val;
    }

    cout << max_gap << endl;
    delete[] arr;
    delete[] buckets;
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
