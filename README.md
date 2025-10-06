import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.*;

public class OrderFood {
    static class Dish {
        String name;
        int price;
        int prepMin;
        Dish(String name, int price, int prepMin) {
            this.name = name;
            this.price = price;
            this.prepMin = prepMin;
        }
    }

    public static void main(String[] args) {
        Map<String, Dish> menu = new LinkedHashMap<>();
        menu.put("бургер", new Dish("Бургер", 1200, 10));
        menu.put("пицца", new Dish("Пицца", 2500, 20));
        menu.put("салат", new Dish("Салат", 800, 5));
        menu.put("суп", new Dish("Суп", 900, 12));

        Scanner sc = new Scanner(System.in);
        for (Dish d : menu.values()) {
            System.out.printf("%s — %d тг, %d мин%n", d.name, d.price, d.prepMin);
        }

        Map<String, Integer> order = new LinkedHashMap<>();
        while (true) {
            System.out.print("Введите блюдо (или 'готово'): ");
            String input = sc.nextLine().trim().toLowerCase();
            if (input.equals("готово")) break;
            if (!menu.containsKey(input)) continue;
            System.out.print("Количество: ");
            int qty = sc.nextInt();
            sc.nextLine();
            order.put(input, order.getOrDefault(input, 0) + qty);
        }

        if (order.isEmpty()) return;

        int total = 0;
        List<Integer> prepTimes = new ArrayList<>();
        for (Map.Entry<String, Integer> e : order.entrySet()) {
            Dish d = menu.get(e.getKey());
            int qty = e.getValue();
            int linePrice = d.price * qty;
            total += linePrice;
            prepTimes.add(d.prepMin * qty);
            System.out.printf("%s x%d — %d тг%n", d.name, qty, linePrice);
        }

        int readyInMin = Collections.max(prepTimes);
        LocalTime readyTime = LocalTime.now().plusMinutes(readyInMin);
        System.out.println("Итого: " + total + " тг");
        System.out.println("Готово через " + readyInMin + " минут (" +
                readyTime.format(DateTimeFormatter.ofPattern("HH:mm")) + ")");
    }
}
