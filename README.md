# Inventory Management System

## 📦 Overview  
This is a **multi-threaded Java application** that simulates an **Inventory Management System** with concurrent operations. The system supports:  

✅ **🛒 Purchase** – Reducing stock when an item is bought  
✅ **📥 Restock** – Increasing stock for a product  
✅ **🔍 Check Stock** – Viewing the current stock level  

The program runs **100 concurrent threads** that randomly perform these operations on **10 different products** while handling potential conflicts.

## 🛠 Installation  
1. Clone this repository:  
2. Compile the Java program:
3. Run the application:  

## 🚀 How It Works  
- Each thread selects a **random product** and a **random operation**.  
- If two or more threads access the same product **simultaneously**, a **conflict is detected**.  
- The system **logs conflicts** and tracks which thread **wins or loses** the operation.  

## 🔄 Sample Output  
```
📦 Initial Inventory: {Product 1 - Apple=5, Product 2 - Banana=3, ...}

⚠ CONFLICT on: pool-1-thread-80 - Product 2 - Banana - 🛒 Purchase 
   at 1739526893085 with Previous: pool-1-thread-75 - Product 2 - Banana - 🛒 Purchase at 1739526893085

pool-1-thread-80 🏆 WON the conflict on Product 2 - Banana  
pool-1-thread-75 ❌ Purchase Failed: Product 2 - Banana Out of Stock  

pool-1-thread-22 ✅ Restock Success: Product 4 - Dates (+10)  
pool-1-thread-57 🔍 Checked Stock: Product 6 - Fig => 8  

📦 Final Inventory: {Product 1 - Apple=2, Product 2 - Banana=0, ...}
```

## 🤝 Contributions  
Feel free to fork and submit pull requests if you improve the conflict resolution logic or add new features! 🚀  
