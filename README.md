# Radix sort O(kn)
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