## Описание Алгоритма
Используем жадный алгоритм для решения данной задачи.
За каждую ходку заяц забирает максимально возможные 5 кг моркови, пока суммарно на полянах находится не менее этих 5 кг моркови.
При этом, максимальное количество ходок равно 10.
Когда оставшееся суммарное количество моркови на полянах становится меньше 5 кг, заяц забирает то, что осталось.

## Код
```java
import java.util.Arrays;

public class Carrot {
    public static void main(String[] args) {
        int[] carrots = {1, 2, 3, 4, 5};
        minWalks(carrots);
    }

    public static void minWalks(int[] arr){

        int sum = Arrays.stream(arr).sum();
        int maxWeight = 5;
        int totalCarrots = 0;
        int maxWalks = 10;
        int currentWalks = 0;

        while (currentWalks < maxWalks && sum > 0){
            if (sum >= maxWeight){
                totalCarrots += maxWeight;
                sum -= maxWeight;
            } else {
                totalCarrots += sum;
                sum -= sum;
            }

            currentWalks++;
        }

        System.out.println("Количество ходок: " + currentWalks + "\nУнесено моркови: " + totalCarrots + " кг");
    }

}
