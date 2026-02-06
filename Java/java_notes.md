# POCKET NOTES:

## 1. DATA TYPES
### ✅ WHY we need Data Types:
        - Computer memory is only 0s and 1s.
		- Data types tell JVM:
			👉 What kind of data it is
			👉 How much memory to allocate
			👉 How to operate on it

		- So JVM can handle data properly.

### 🎒 Analogy (Real life):
		Containers:
		🧴 Bottle → for water
		🍱 Lunch box → for food
		🎒 Bag → for random things

		If we mix everything:
		• Water in lunch box → leaks, hard to use
		• Food in bag → damaged
		• Pens in bottle → hard to remove

		👉 Messy & inefficient

		Same in memory without data types.

### ❌ Problems WITHOUT data types:
		• JVM won’t know how much memory to use
		• Confusion between number, character, true/false
		• Wrong operations
		• More errors
		• Slower performance

### ✅ Solution with Data Types
		✔ Proper memory allocation
		✔ Clear data meaning
		✔ Safe operations
		✔ Early error detection
		✔ Better performance
		
		ex: int a = 10;
		    int b = 20;   --------- Here JVM knows for (a + b) I need to perform addition
		    
		    String a = "A";
		    String b = "B"; ------- Here JVM knows for (a + b) I need to perform concatenation

### 🎯 One-line summary: Data types organize data in memory so JVM can store, understand, and process it correctly.


## 2. SELECTION STATEMENTS - if, switch

### 🧠 Core idea:
		👉 They solve different types of decision problems
		Not one is better — each is made for a purpose.

### ✅ IF–ELSE → for LOGIC & CONDITIONS

		Use when decision depends on:

		• ranges
		• comparisons
		• complex expressions

		Examples it handles best:
		age >= 18
		marks > 60 && marks < 80
		salary > 50000

		WHY if-else exists:

		Because real life is mostly conditions, not fixed values.

### ✅ SWITCH → for FIXED OPTIONS
	      - Use when one variable is checked against many exact values.

		Examples:
		choice = 1,2,3,4
		day = "Monday","Tuesday"
		operation = '+','-','*'

		WHY switch exists:

		To avoid long ugly if-else ladders like:

		if(choice==1) ...
		else if(choice==2) ...
		else if(choice==3) ...

		Switch makes it:

		✔ cleaner
		✔ readable
		✔ easier to maintain

### 🎯 Real life analogy
	   1. if-else = decision by condition
		  👉 "If temperature > 30 wear cotton"

	   2. switch = decision by choice
	      👉 "Press 1 for English, 2 for Hindi"

### 📌 Pocket summary:
	   1. if-else:
		  👉 if-else is for checking conditions.

	   2. switch:
		  👉 switch is for matching values
		

	👉 You CANNOT replace all if-else with switch
	👉 You CAN replace some long if-else chains with switch

	That’s why both exist.

## 3. LOOPING STATEMENTS - while & do-while 
    - We can solve below example using both do-while and while. But do-while is Cleaner.
	👉 do-while - Real Menu Example using

	   ex:
		do {
		    // show menu
		    System.out.println("\n--- MENU ---");
		    System.out.println("1. Add numbers");
		    System.out.println("2. Subtract numbers");
		    System.out.println("3. Multiply numbers");
		    System.out.println("0. Exit");

		    System.out.print("Enter your choice: ");
		    choice = sc.nextInt();

		    switch(choice) {
			 case 1:
			     System.out.println("Addition selected");
			     break;
			 case 2:
			     System.out.println("Subtraction selected");
			     break;
			 case 3:
			     System.out.println("Multiplication selected");
			     break;
			 case 0:
			     System.out.println("Exiting program...");
			     break;
			 default:
			     System.out.println("Invalid choice");
		    }
		} while (choice != 0);
	
	👉 while - Same example in while
	   ex:
	        int choice = 1;
		while(choice != 0) {
		    System.out.println("\n--- MENU ---");
		    System.out.println("1. Add numbers");
		    System.out.println("2. Subtract numbers");
		    System.out.println("3. Multiply numbers");
		    System.out.println("0. Exit");

		    System.out.print("Enter your choice: ");
		    choice = sc.nextInt();
		   
		    switch(choice) {
			 case 1:
			     System.out.println("Addition selected");
			     break;
			 case 2:
			     System.out.println("Subtraction selected");
			     break;
			 case 3:
			     System.out.println("Multiplication selected");
			     break;
			 case 0:
			     System.out.println("Exiting program...");
			     break;
			 default:
			     System.out.println("Invalid choice");
		    }
		 }


## 4. GENERICS

### 1. What is Type Erasure?
   *  Java compiler removes generic type information at compile time i.e **Type Erasure**.

    1️⃣ At compile time:
        ✔ Generics exist
        ✔ Type checking happens
        ✔ Primitives are autoboxed to wrapper classes
        ✔ Then compiler erases generic type info (type erasure)

        ex: ArrayList<Integer>  →  ArrayList

    2️⃣ At run-time:
        ✔ JVM sees only raw ArrayList
        ✔ It stores Object references
        ✔ Actual objects are wrapper objects (Integer, Double, etc.)

        ex: Object obj  = new Integer(10);

    🎯 MAIN reason for type erasure:
        👉 **Backward compatibility** with old Java code and JVM