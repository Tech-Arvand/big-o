# Виды и примеры Big-O (Golang)
### [@Big-O на Python](./README_python.md)
### [@Big-O на JavaScript](./README_javascript.md)

## 1. $O(1)$ — Константная сложность
Алгоритм выполняет одно и то же количество операций независимо от размера входных данных.

Пример: доступ к элементу массива по индексу.
```go
func getFirstElement(arr []int) int {
    return arr[0]  // Одна операция, независимо от размера массива
}
```

## 2. $O(log\ n)$ — Логарифмическая сложность
Алгоритм работает быстрее за счет уменьшения количества данных на каждом шаге, как, например, в бинарном поиске.

Пример: бинарный поиск в отсортированном массиве.
```go
func binarySearch(arr []int, target int) bool {
    left, right := 0, len(arr)-1
    for left <= right {
        mid := (left + right) / 2
        if arr[mid] == target {
            return true
        } else if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return false
}
```

## 3. $O(n)$ — Линейная сложность
Алгоритм выполняет количество операций, прямо пропорциональное размеру входных данных.

Пример: поиск элемента в несортированном массиве.
```go
func findElement(arr []int, target int) bool {
    for _, num := range arr {
        if num == target {
            return true
        }
    }
    return false
}
```

## 4. $O(n\log\ n)$ — Линейно-логарифмическая сложность
Эта сложность характерна для многих алгоритмов сортировки, таких как быстрая сортировка или сортировка слиянием.

Пример: сортировка слиянием.
```go
func mergeSort(arr []int) []int {
    if len(arr) <= 1 {
        return arr
    }
    mid := len(arr) / 2
    left := mergeSort(arr[:mid])
    right := mergeSort(arr[mid:])
    return merge(left, right)
}

func merge(left, right []int) []int {
    result := []int{}
    i, j := 0, 0
    for i < len(left) && j < len(right) {
        if left[i] < right[j] {
            result = append(result, left[i])
            i++
        } else {
            result = append(result, right[j])
            j++
        }
    }
    result = append(result, left[i:]...)
    result = append(result, right[j:]...)
    return result
}
```

## 5. $O(n^2)$ — Квадратичная сложность
Количество операций увеличивается как квадрат от размера входных данных. Это происходит в алгоритмах с двумя вложенными циклами.

Пример: сортировка пузырьком.
```go
func bubbleSort(arr []int) {
    for i := 0; i < len(arr); i++ {
        for j := 0; j < len(arr)-i-1; j++ {
            if arr[j] > arr[j+1] {
                arr[j], arr[j+1] = arr[j+1], arr[j]
            }
        }
    }
}
```

## 6. $O(2^n)$ — Экспоненциальная сложность
Количество операций удваивается с каждым увеличением размера входных данных. Часто встречается в задачах, связанных с рекурсией.

Пример: решение задачи о нахождении всех подмножеств (рекурсия).
```go
func subsets(arr []int) [][]int {
    res := [][]int{}
    backtrack(&res, []int{}, arr, 0)
    return res
}

func backtrack(res *[][]int, temp, arr []int, start int) {
    *res = append(*res, append([]int{}, temp...))
    for i := start; i < len(arr); i++ {
        temp = append(temp, arr[i])
        backtrack(res, temp, arr, i+1)
        temp = temp[:len(temp)-1]
    }
}
```

## 7. $O(n!)$ — Факториальная сложность
Очень редкая сложность, возникает в задачах, где необходимо перебирать все возможные перестановки или комбинации.

Пример: нахождение всех перестановок множества.
```go
func permute(arr []int) [][]int {
    res := [][]int{}
    backtrackPermute(&res, arr, 0)
    return res
}

func backtrackPermute(res *[][]int, arr []int, start int) {
    if start == len(arr) {
        *res = append(*res, append([]int{}, arr...))
        return
    }
    for i := start; i < len(arr); i++ {
        arr[start], arr[i] = arr[i], arr[start]
        backtrackPermute(res, arr, start+1)
        arr[start], arr[i] = arr[i], arr[start]
    }
}
```

## 8. $O(√n)$ — Корневая сложность
Алгоритм выполняет операции пропорционально квадратному корню от размера входных данных. Такая сложность встречается, например, при поиске делителей числа до его квадратного корня.

Пример: проверка числа на простоту через делители до $√n$.
```go
func isPrime(n int) bool {
    if n <= 1 {
        return false
    }
    for i := 2; i*i <= n; i++ {
        if n%i == 0 {
            return false
        }
    }
    return true
}
```

## 9. $O(log^2\ n)$ — Логарифм в квадрате
Эта сложность возникает в некоторых сбалансированных структурах данных или в оптимизированных вариантах поиска и сортировки.

Пример: Двойной бинарный поиск
Предположим, что у нас есть двумерный массив, отсортированный по строкам и столбцам, и мы хотим найти элемент. Здесь мы используем бинарный поиск сначала по строкам, затем по столбцам, что приводит к сложности $O(log\ n * log\ n)$ = $O(log^2\ n)$.

```go
// Двойной бинарный поиск
func binarySearch2D(matrix [][]int, target int) bool {
	rows := len(matrix)
	if rows == 0 {
		return false
	}
	cols := len(matrix[0])

	// Выполняем бинарный поиск по строкам
	start, end := 0, rows-1
	for start <= end {
		mid := (start + end) / 2
		if matrix[mid][0] <= target && target <= matrix[mid][cols-1] {
			// Если target в пределах строки, выполняем бинарный поиск по этой строке
			return binarySearch(matrix[mid], target)
		} else if matrix[mid][0] > target {
			end = mid - 1
		} else {
			start = mid + 1
		}
	}

	return false
}

// Обычный бинарный поиск в одномерном массиве
func binarySearch(arr []int, target int) bool {
	start, end := 0, len(arr)-1
	for start <= end {
		mid := (start + end) / 2
		if arr[mid] == target {
			return true
		} else if arr[mid] < target {
			start = mid + 1
		} else {
			end = mid - 1
		}
	}
	return false
}
```
### Объяснение:
Мы сначала делим двумерный массив на строки, выполняя бинарный поиск по строкам.
Затем выполняем бинарный поиск по столбцам внутри выбранной строки.
Это приводит к двум логарифмическим операциям: первая по строкам, вторая по столбцам, что даёт сложность $O(log\ n)$ для поиска строки и $O(log\ n)$ для поиска в строке, а в итоге $O(log^2\ n)$.

## 10. $O(n^3)$ — Кубическая сложность
Этот тип сложности возникает в задачах, связанных с матрицами или графами, когда требуется перебрать все пары и для каждой пары выполнить дополнительную работу.

Пример: алгоритм Флойда-Уоршелла для нахождения всех кратчайших путей в графе.
```go
func floydWarshall(graph [][]int) [][]int {
    dist := make([][]int, len(graph))
    for i := range graph {
        dist[i] = make([]int, len(graph[i]))
        copy(dist[i], graph[i])
    }

    for k := 0; k < len(graph); k++ {
        for i := 0; i < len(graph); i++ {
            for j := 0; j < len(graph); j++ {
                if dist[i][k] + dist[k][j] < dist[i][j] {
                    dist[i][j] = dist[i][k] + dist[k][j]
                }
            }
        }
    }

    return dist
}
```

## 11. $O(3^n)$ — Экспоненциальная сложность с базой 3
Такой тип сложности появляется в задачах, где каждое решение имеет три варианта развития, например, при работе с деревьями решений.

Пример: рекурсия с тремя ветвями.
```go
// Рекурсивное разветвление на три варианта, пример решения с трёхвариантной рекурсией
func threeWayRecursion(n int) int {
    if n == 0 {
        return 1
    }
    return threeWayRecursion(n-1) + threeWayRecursion(n-2) + threeWayRecursion(n-3)
}
```

## 12. $O(\frac{2^n}{log\ n})$ — Экспоненциальная сложность с логарифмическим делителем
Такую сложность можно встретить в сложных задачах оптимизации или сжатия данных, где экспоненциальное разрастание замедляется за счет деления на логарифм.  Примеры для сложности $O(\frac{2^n}{log\ n})$ встречается крайне редко, так как это очень специфическая сложность. Она возникает в задачах, где мы имеем экспоненциальное разрастание вариантов, но на каждом шаге количество операций уменьшается за счёт логарифмического фактора. Один из вариантов, где можно увидеть такую сложность, — это задачи, связанные с комбинаторикой или оптимизацией, где мы можем применять технику разветвления решений, а затем на каждом шаге сокращать варианты.

Пример: задачи, где разрастание комбинируется с уменьшением на каждом шаге.
```go
// Пример задачи, где увеличение вариантов замедляется за счет логарифма
// Подобная структура может быть при решении задач с оптимизацией
// Функция для генерации всех подмножеств с оптимизацией
func generateSubsetsOptimized(arr []int, target int) [][]int {
	res := [][]int{}
	limit := int(math.Pow(2, float64(len(arr)))) // количество возможных подмножеств = 2^n

	for i := 0; i < limit; i++ {
		subset := []int{}
		sum := 0
		for j := 0; j < len(arr); j++ {
			// Проверяем, включен ли j-й элемент в подмножество
			if (i>>j)&1 == 1 {
				subset = append(subset, arr[j])
				sum += arr[j]
			}
		}
		// Применяем оптимизацию: если сумма подмножества уже превышает target, игнорируем его
		if sum > target {
			continue
		}
		res = append(res, subset)
	}
	return res
}
```
### Объяснение:
Мы генерируем все подмножества массива, используя битовые операции. Это стандартная экспоненциальная операция с $2^n$ вариантами.
Однако, для оптимизации, мы проверяем на каждом шаге сумму элементов в текущем подмножестве. Если сумма превышает заданное целевое значение (target), мы прерываем дальнейшую обработку этого подмножества и продолжаем с другими вариантами. Это даёт сокращение вариантов поиска, основанное на некотором логарифмическом факторе, уменьшающем количество операций.


# Заключение
Эти примеры показывают, как различная сложность алгоритмов отражается на времени выполнения. Чем выше сложность (например, $O(n!)$ или $O(2^n))$, тем быстрее растёт время выполнения при увеличении входных данных. Чтобы научиться определять сложность алгоритмов, попробуй анализировать их, изучая вложенность циклов, рекурсию и другие структуры.

####
# Шпоргалки
![BIG O - оценка пространственной сложности и временной скорости](./BIG%20O%20-%20оценка%20пространственной%20сложности%20и%20временной%20скорости.png)
![BIG O - оценка пространственной сложности и временной скорости](./BIG%20O%20-%20оценка%20сложности%20.png)

