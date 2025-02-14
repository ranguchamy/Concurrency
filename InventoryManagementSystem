import java.util.concurrent.*;
import java.util.*;

public class InventoryManagementSystem {
    private static final int THREAD_COUNT = 100;
    private static final ConcurrentHashMap<String, Integer> inventory = new ConcurrentHashMap<>();
    private static final Random random = new Random();

    private static final String[] products = {
            "Product 1 - Apple", "Product 2 - Banana", "Product 3 - Cherry",
            "Product 4 - Dates", "Product 5 - Eggplant", "Product 6 - Fig",
            "Product 7 - Grapes", "Product 8 - Honeydew", "Product 9 - Iceberg Lettuce",
            "Product 10 - Jackfruit"
    };

    private static final ConcurrentHashMap<String, Long> productLocks = new ConcurrentHashMap<>();
    private static final ConcurrentHashMap<String, String> productOperations = new ConcurrentHashMap<>();
    private static final ConcurrentHashMap<String, String> productThreads = new ConcurrentHashMap<>();

    public static void main(String[] args) throws InterruptedException {
        for (String product : products) {
            inventory.put(product, random.nextInt(10));
        }

        System.out.println("📦 Initial Inventory: " + inventory);

        ExecutorService executor = Executors.newFixedThreadPool(THREAD_COUNT);

        for (int i = 0; i < THREAD_COUNT; i++) {
            executor.submit(() -> performRandomInventoryOperation());
        }

        executor.shutdown();
        executor.awaitTermination(10, TimeUnit.SECONDS);

        System.out.println("📦 Final Inventory: " + inventory);
    }

    private static void performRandomInventoryOperation() {
        String product = products[random.nextInt(products.length)];
        int operation = random.nextInt(3);
        String operationType = switch (operation) {
            case 0 -> "🛒 Purchase";
            case 1 -> "📥 Restock";
            default -> "🔍 Check Stock";
        };

        long currentTime = System.currentTimeMillis();
        Long previousTime = productLocks.put(product, currentTime);
        String previousOp = productOperations.put(product, operationType);
        String previousThread = productThreads.put(product, Thread.currentThread().getName());

        boolean conflict = (previousTime != null);
        if (conflict) {
            System.out.println("⚠ CONFLICT on: " + Thread.currentThread().getName() + " - " + product + " - " + operationType +
                    " at " + currentTime + " with Previous: " + previousThread + " - " + product + " - " + previousOp + " at " + previousTime);
        }

        boolean operationSuccess = false;
        switch (operation) {
            case 0: // PURCHASE
                operationSuccess = inventory.compute(product, (key, stock) -> {
                    if (stock == null || stock <= 0) {
                        System.out.println(previousThread + " ❌ Purchase Failed: " + product + " Out of Stock");
                        return stock;
                    }
                    System.out.println(Thread.currentThread().getName() + " ✅ Purchase Success: " + product + " (Remaining: " + (stock - 1) + ")");
                    return stock - 1;
                }) != null;
                break;

            case 1: // RESTOCK
                int restockAmount = 5 + random.nextInt(11);
                inventory.compute(product, (key, stock) -> (stock == null) ? restockAmount : stock + restockAmount);
                System.out.println(Thread.currentThread().getName() + " ✅ Restock Success: " + product + " (+ " + restockAmount + ")");
                operationSuccess = true;
                break;

            case 2: // CHECK STOCK
                Integer stockLevel = inventory.get(product);
                System.out.println(Thread.currentThread().getName() + " 🔍 Checked Stock: " + product + " => " + stockLevel);
                operationSuccess = true;
                break;
        }

        if (conflict) {
            if (operationSuccess) {
                System.out.println(Thread.currentThread().getName() + " 🏆 WON the conflict on " + product);
            } else {
                System.out.println(Thread.currentThread().getName() + " ❌ LOST the conflict on " + product);
            }
        }

        productLocks.remove(product);
        productOperations.remove(product);
        productThreads.remove(product);

        try {
            Thread.sleep(random.nextInt(50));
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
