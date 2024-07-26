## Описание Алгоритма
Используем жадный алгоритм для решения данной задачи.
Сначала заяц собирает всю морковь с 5 полянки. Если морковь на 5 полянке закончилась,
а количество ходок еще не достигло 10, заяц приступает к комбинациям полянок таких, что сумма весов
их морковей равна 5. В данном случаи это 4 и 1 полянки, а также 3 и 2 полянки.
Если количество ходок после этого еще не достигло 10, то заяц начинает собирать морковь с
1 и 3 полянок, и, закончив с этим, идет собирать полянки морковь с 4 полянки отдельно. Далее заяц идет
собирать морковь с 1 и 2 полянок (там общий вес составляет уже 3 кг). И последняя часть алгоритма —
заяц собирает морковь отдельно с 3, 2 и 1 полянок (если суммарное количество ходок все еще не достигло 10).
Таким образом, реализован жадный алгоритм для сбора моркови с 5 полянок — на каждом этапе заяц ищет полянки или
их комбинации, на которых получится взять максимально возможное для данного этапа количество моркови.

## Код
```java
import java.util.HashMap;
import java.util.Map;

public class RabbitCarrot {
    public static void main(String[] args) {
        Map<Integer, Integer> map = new HashMap<>();
        // В качестве ключа выступает вес моркови на соответствующей полянке, а значением является количество моркови, растущей там.
        map.put(1, 0);
        map.put(2, 0);
        map.put(3, 0);
        map.put(4, 0);
        map.put(5, 0);
        takeCarrots(map);
    }

    private static void takeCarrots(Map<Integer, Integer> map) {

        int maxWalks = 10;

        int[] currentWalks = {0}; // Используется массив для изменения значения переменной внутри массива.
        int[] totalWeight = {0};

        singleIter(map, 5, currentWalks, maxWalks, totalWeight);

        int[][] fieldPairs = {
                {4, 1},
                {3, 2},
                {3, 1}
        };

        iterOfPairs(map, fieldPairs, currentWalks, maxWalks, totalWeight);

        singleIter(map, 4, currentWalks, maxWalks, totalWeight);

        iterOfPairs(map, new int[][] {{2, 1}}, currentWalks, maxWalks, totalWeight);

        for (int i = 3; i >= 1; i--) {
            singleIter(map, i, currentWalks, maxWalks, totalWeight);
        }

        System.out.println("Количество ходок: " + currentWalks[0] + "\nУнесено моркови: " + totalWeight[0] + " кг");
    }

    private static void singleIter(Map<Integer, Integer> map, int n, int[] currentWalks, int maxWalks, int[] totalWeight){
        while (map.get(n) > 0 && currentWalks[0] < maxWalks) {
            totalWeight[0] += n;
            currentWalks[0]++;
            map.computeIfPresent(n, (key, value) -> value - 1);
        }
    }

    private static void iterOfPairs(Map<Integer, Integer> map, int[][] fieldPairs, int[] currentWalks, int maxWalks, int[] totalWeight){
        for (int[] pair : fieldPairs) {
            int pairWeight = pair[0] + pair[1];
            while (map.get(pair[0]) > 0 && map.get(pair[1]) > 0 && currentWalks[0] < maxWalks) {
                totalWeight[0] += pairWeight;
                currentWalks[0]++;
                map.computeIfPresent(pair[0], (key, value) -> value - 1);
                map.computeIfPresent(pair[1], (key, value) -> value - 1);
            }
        }
    }
}
