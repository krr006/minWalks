## Описание Алгоритма
Используем жадный алгоритм для решения данной задачи. Начинаем с полянок, где морковь имеет максимальный вес, и постепенно переходим к полянкам с меньшим весом, если заяц еще не достиг максимального количества ходок.

1. Заяц начинает с полянки, где морковь имеет максимальный вес (5 кг). Он собирает всю морковь с этой полянки, пока она не закончится или пока количество ходок не достигнет максимума (10 ходок).

2. Если после этого у зайца остаются еще ходки, он начинает собирать морковь с комбинаций полянок, сумма весов которых равна 5 кг. Например, это могут быть комбинации 4 и 1 кг, или 3 и 2 кг. Заяц продолжает собирать морковь, пока комбинации не исчерпаются или пока не достигнуто максимальное количество ходок.

3. Если после этого у зайца все еще остаются ходки, он переходит к полянкам, где морковь имеет вес на единицу меньше, чем предыдущий максимальный (4 кг), и повторяет процесс.

4. Заяц продолжает этот процесс, постепенно уменьшая максимальный вес моркови, с полянок и их комбинаций, пока не достигнет максимального количества ходок или пока не соберет всю морковь.

Таким образом, реализован жадный алгоритм для сбора моркови с 5 полянок — на каждом этапе заяц ищет полянки или
их комбинации, на которых получится взять максимально возможное для данного этапа количество моркови.
## Код
```java
import java.util.HashMap;
import java.util.Map;
import java.util.ArrayList;
import java.util.List;

public class RabbitCarrot {
    public static void main(String[] args) {
        Map<Integer, Integer> map = new HashMap<>();
        // В качестве ключа выступает вес моркови на соответствующей полянке, а значением является количество моркови, растущей там.
        map.put(1, 6);
        map.put(2, 7);
        map.put(3, 8);
        map.put(4, 0);
        map.put(5, 6);

        takeMaxCarrots(map, 10);
    }

    private static void takeMaxCarrots(Map<Integer, Integer> map, int maxWalks) {
        int[] totalWeight = {0}; // Используется массив для изменения значения переменной внутри массива.
        int[] walksLeft = {maxWalks};
        int[] currentMaxWeight = {5};

        while (currentMaxWeight[0] > 0 && walksLeft[0] > 0) {
            totalWeight[0] += collectMaxWeight(map, currentMaxWeight, walksLeft);
            if (walksLeft[0] > 0) {
                currentMaxWeight[0]--;
            }
        }

        System.out.println("Количество ходок: " + (maxWalks - walksLeft[0]) + "\nУнесено моркови: " + totalWeight[0] + " кг");
    }

    private static int collectMaxWeight(Map<Integer, Integer> map, int[] currentMaxWeight, int[] walksLeft) {
        int weightCollected = 0;


        while (map.get(currentMaxWeight[0]) > 0 && walksLeft[0] > 0) {
            weightCollected += currentMaxWeight[0];
            walksLeft[0]--;
            map.computeIfPresent(currentMaxWeight[0], (key, value) -> value - 1);
        }

        List<int[]> pairs = getFieldPairs(map.keySet().toArray(new Integer[0]), currentMaxWeight[0]);

        for (int[] pair : pairs) {
            int pairWeight = pair[0] + pair[1];
            while (map.get(pair[0]) > 0 && map.get(pair[1]) > 0 && walksLeft[0] > 0) {
                weightCollected += pairWeight;
                walksLeft[0]--;
                map.computeIfPresent(pair[0], (key, value) -> value - 1);
                map.computeIfPresent(pair[1], (key, value) -> value - 1);
            }
        }


        return weightCollected;
    }

    private static List<int[]> getFieldPairs(Integer[] fields, int maxWeight) {
        List<int[]> pairs = new ArrayList<>();

        for (int i = 0; i < fields.length; i++) {
            for (int j = i + 1; j < fields.length; j++) {
                if (fields[i] + fields[j] == maxWeight) {
                    pairs.add(new int[]{fields[i], fields[j]});
                }
            }
        }

        return pairs;
    }
}
