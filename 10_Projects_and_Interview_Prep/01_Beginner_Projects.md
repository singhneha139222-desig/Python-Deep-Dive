# 📘 Beginner Projects

## 📌 Introduction

These beginner projects help you apply fundamental Python concepts. Each project uses only basics: variables, loops, functions, and simple file operations.

---

## 🎯 Project 1: Todo List Application

### Problem Statement

Create a command-line todo list application where users can:
- Add tasks
- View all tasks
- Mark tasks as complete
- Delete tasks
- Save tasks to file

### Approach

1. Use a list to store tasks
2. Implement menu-driven interface
3. Save/load tasks from JSON file
4. Handle user input validation

### Complete Code

```python
import json
import os
from datetime import datetime

class TodoList:
    def __init__(self, filename="todos.json"):
        self.filename = filename
        self.tasks = []
        self.load_tasks()
    
    def load_tasks(self):
        """Load tasks from file"""
        if os.path.exists(self.filename):
            try:
                with open(self.filename, 'r') as f:
                    self.tasks = json.load(f)
            except json.JSONDecodeError:
                self.tasks = []
        else:
            self.tasks = []
    
    def save_tasks(self):
        """Save tasks to file"""
        with open(self.filename, 'w') as f:
            json.dump(self.tasks, f, indent=2)
    
    def add_task(self, title, priority="medium"):
        """Add a new task"""
        task = {
            "id": len(self.tasks) + 1,
            "title": title,
            "completed": False,
            "priority": priority,
            "created_at": datetime.now().strftime("%Y-%m-%d %H:%M")
        }
        self.tasks.append(task)
        self.save_tasks()
        print(f"✅ Task '{title}' added successfully!")
    
    def view_tasks(self, show_completed=True):
        """Display all tasks"""
        if not self.tasks:
            print("\n📋 No tasks found. Add some tasks to get started!")
            return
        
        print("\n" + "=" * 50)
        print("📋 YOUR TODO LIST")
        print("=" * 50)
        
        for task in self.tasks:
            if not show_completed and task['completed']:
                continue
            
            status = "✅" if task['completed'] else "⬜"
            priority_icons = {"high": "🔴", "medium": "🟡", "low": "🟢"}
            priority = priority_icons.get(task['priority'], "⚪")
            
            print(f"{task['id']}. {status} {priority} {task['title']}")
            print(f"   Created: {task['created_at']}")
        
        print("=" * 50)
        
        # Statistics
        total = len(self.tasks)
        completed = sum(1 for t in self.tasks if t['completed'])
        print(f"Total: {total} | Completed: {completed} | Pending: {total - completed}")
    
    def complete_task(self, task_id):
        """Mark a task as complete"""
        for task in self.tasks:
            if task['id'] == task_id:
                task['completed'] = True
                self.save_tasks()
                print(f"✅ Task '{task['title']}' marked as complete!")
                return
        print(f"❌ Task with ID {task_id} not found.")
    
    def delete_task(self, task_id):
        """Delete a task"""
        for i, task in enumerate(self.tasks):
            if task['id'] == task_id:
                deleted = self.tasks.pop(i)
                self.save_tasks()
                print(f"🗑️ Task '{deleted['title']}' deleted!")
                return
        print(f"❌ Task with ID {task_id} not found.")
    
    def edit_task(self, task_id, new_title):
        """Edit task title"""
        for task in self.tasks:
            if task['id'] == task_id:
                task['title'] = new_title
                self.save_tasks()
                print(f"✏️ Task updated to '{new_title}'!")
                return
        print(f"❌ Task with ID {task_id} not found.")

def main():
    todo = TodoList()
    
    while True:
        print("\n📝 TODO LIST MENU")
        print("1. View all tasks")
        print("2. Add new task")
        print("3. Mark task as complete")
        print("4. Delete task")
        print("5. Edit task")
        print("6. View pending tasks only")
        print("0. Exit")
        
        choice = input("\nEnter your choice: ").strip()
        
        if choice == "1":
            todo.view_tasks()
        
        elif choice == "2":
            title = input("Enter task title: ").strip()
            if title:
                print("Priority (high/medium/low): ", end="")
                priority = input().strip().lower() or "medium"
                todo.add_task(title, priority)
            else:
                print("❌ Task title cannot be empty!")
        
        elif choice == "3":
            todo.view_tasks()
            try:
                task_id = int(input("Enter task ID to complete: "))
                todo.complete_task(task_id)
            except ValueError:
                print("❌ Please enter a valid number!")
        
        elif choice == "4":
            todo.view_tasks()
            try:
                task_id = int(input("Enter task ID to delete: "))
                todo.delete_task(task_id)
            except ValueError:
                print("❌ Please enter a valid number!")
        
        elif choice == "5":
            todo.view_tasks()
            try:
                task_id = int(input("Enter task ID to edit: "))
                new_title = input("Enter new title: ").strip()
                if new_title:
                    todo.edit_task(task_id, new_title)
            except ValueError:
                print("❌ Please enter a valid number!")
        
        elif choice == "6":
            todo.view_tasks(show_completed=False)
        
        elif choice == "0":
            print("👋 Goodbye!")
            break
        
        else:
            print("❌ Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

### Skills Used
- File I/O (JSON)
- Classes and OOP
- User input handling
- Error handling
- Menu-driven interface

### Improvements to Try
- Add due dates
- Add categories/tags
- Add search functionality
- Add export to text/CSV

---

## 🎯 Project 2: Number Guessing Game

### Problem Statement

Create a number guessing game where:
- Computer picks a random number
- User guesses with hints (higher/lower)
- Track attempts and high scores
- Different difficulty levels

### Complete Code

```python
import random
import json
import os

class NumberGuessingGame:
    def __init__(self):
        self.high_scores_file = "high_scores.json"
        self.high_scores = self.load_high_scores()
        self.difficulties = {
            "easy": {"range": (1, 50), "max_attempts": 10},
            "medium": {"range": (1, 100), "max_attempts": 7},
            "hard": {"range": (1, 200), "max_attempts": 5}
        }
    
    def load_high_scores(self):
        if os.path.exists(self.high_scores_file):
            with open(self.high_scores_file, 'r') as f:
                return json.load(f)
        return {}
    
    def save_high_scores(self):
        with open(self.high_scores_file, 'w') as f:
            json.dump(self.high_scores, f, indent=2)
    
    def display_menu(self):
        print("\n" + "=" * 40)
        print("🎮 NUMBER GUESSING GAME")
        print("=" * 40)
        print("1. Play Game")
        print("2. View High Scores")
        print("3. How to Play")
        print("0. Exit")
        print("=" * 40)
    
    def select_difficulty(self):
        print("\n🎯 SELECT DIFFICULTY")
        print("1. Easy (1-50, 10 attempts)")
        print("2. Medium (1-100, 7 attempts)")
        print("3. Hard (1-200, 5 attempts)")
        
        choice = input("\nEnter choice (1-3): ").strip()
        
        if choice == "1":
            return "easy"
        elif choice == "2":
            return "medium"
        elif choice == "3":
            return "hard"
        else:
            print("Invalid choice, defaulting to Medium")
            return "medium"
    
    def play(self):
        difficulty = self.select_difficulty()
        settings = self.difficulties[difficulty]
        
        low, high = settings["range"]
        max_attempts = settings["max_attempts"]
        secret_number = random.randint(low, high)
        attempts = 0
        
        print(f"\n🎲 I'm thinking of a number between {low} and {high}")
        print(f"You have {max_attempts} attempts. Good luck!\n")
        
        while attempts < max_attempts:
            attempts += 1
            remaining = max_attempts - attempts
            
            try:
                guess = int(input(f"Attempt {attempts}/{max_attempts} - Your guess: "))
            except ValueError:
                print("❌ Please enter a valid number!")
                attempts -= 1
                continue
            
            if guess < low or guess > high:
                print(f"❌ Please guess between {low} and {high}")
                attempts -= 1
                continue
            
            if guess == secret_number:
                self.handle_win(difficulty, attempts)
                return
            elif guess < secret_number:
                hint = "📈 Too low!"
                if secret_number - guess > 20:
                    hint += " (Much higher!)"
            else:
                hint = "📉 Too high!"
                if guess - secret_number > 20:
                    hint += " (Much lower!)"
            
            print(f"{hint} {remaining} attempts remaining.")
        
        print(f"\n😢 Game Over! The number was {secret_number}")
        print("Better luck next time!")
    
    def handle_win(self, difficulty, attempts):
        print(f"\n🎉 CONGRATULATIONS! You got it in {attempts} attempts!")
        
        # Check if it's a high score
        if difficulty not in self.high_scores or attempts < self.high_scores[difficulty]["attempts"]:
            name = input("New high score! Enter your name: ").strip() or "Anonymous"
            self.high_scores[difficulty] = {
                "name": name,
                "attempts": attempts
            }
            self.save_high_scores()
            print(f"🏆 New {difficulty} high score recorded!")
    
    def view_high_scores(self):
        print("\n🏆 HIGH SCORES")
        print("=" * 40)
        
        for level in ["easy", "medium", "hard"]:
            if level in self.high_scores:
                score = self.high_scores[level]
                print(f"{level.upper()}: {score['name']} - {score['attempts']} attempts")
            else:
                print(f"{level.upper()}: No score yet")
        
        print("=" * 40)
    
    def how_to_play(self):
        print("\n📖 HOW TO PLAY")
        print("=" * 40)
        print("1. Select a difficulty level")
        print("2. Try to guess the secret number")
        print("3. Use hints (higher/lower) to narrow down")
        print("4. Win by guessing correctly within attempts")
        print("5. Beat high scores for glory!")
        print("=" * 40)
    
    def run(self):
        while True:
            self.display_menu()
            choice = input("Enter choice: ").strip()
            
            if choice == "1":
                self.play()
            elif choice == "2":
                self.view_high_scores()
            elif choice == "3":
                self.how_to_play()
            elif choice == "0":
                print("👋 Thanks for playing!")
                break
            else:
                print("❌ Invalid choice!")

if __name__ == "__main__":
    game = NumberGuessingGame()
    game.run()
```

### Skills Used
- Random number generation
- Conditional logic
- File persistence
- Game loop design
- User experience design

---

## 🎯 Project 3: Simple Calculator

### Problem Statement

Build a calculator that:
- Performs basic operations (+, -, *, /)
- Handles advanced operations (power, sqrt, %)
- Maintains calculation history
- Supports continuous calculations

### Complete Code

```python
import math
import json
from datetime import datetime

class Calculator:
    def __init__(self):
        self.history = []
        self.memory = 0
        self.last_result = 0
    
    def add(self, a, b):
        return a + b
    
    def subtract(self, a, b):
        return a - b
    
    def multiply(self, a, b):
        return a * b
    
    def divide(self, a, b):
        if b == 0:
            raise ValueError("Cannot divide by zero!")
        return a / b
    
    def power(self, a, b):
        return a ** b
    
    def square_root(self, a):
        if a < 0:
            raise ValueError("Cannot take square root of negative number!")
        return math.sqrt(a)
    
    def modulo(self, a, b):
        if b == 0:
            raise ValueError("Cannot modulo by zero!")
        return a % b
    
    def calculate(self, a, operator, b=None):
        """Perform calculation and save to history"""
        operations = {
            '+': self.add,
            '-': self.subtract,
            '*': self.multiply,
            '/': self.divide,
            '^': self.power,
            '%': self.modulo
        }
        
        try:
            if operator == 'sqrt':
                result = self.square_root(a)
                expression = f"√{a}"
            elif operator in operations:
                if b is None:
                    raise ValueError("Second number required!")
                result = operations[operator](a, b)
                expression = f"{a} {operator} {b}"
            else:
                raise ValueError(f"Unknown operator: {operator}")
            
            # Save to history
            self.history.append({
                "expression": expression,
                "result": result,
                "timestamp": datetime.now().strftime("%H:%M:%S")
            })
            self.last_result = result
            return result
            
        except Exception as e:
            return f"Error: {e}"
    
    def show_history(self):
        if not self.history:
            print("No calculations yet!")
            return
        
        print("\n📜 CALCULATION HISTORY")
        print("=" * 40)
        for i, calc in enumerate(self.history[-10:], 1):  # Last 10
            print(f"{i}. [{calc['timestamp']}] {calc['expression']} = {calc['result']}")
        print("=" * 40)
    
    def memory_store(self, value):
        self.memory = value
        print(f"💾 Stored {value} in memory")
    
    def memory_recall(self):
        print(f"📤 Memory: {self.memory}")
        return self.memory
    
    def memory_clear(self):
        self.memory = 0
        print("🗑️ Memory cleared")

def get_number(prompt, allow_ans=True):
    """Get a number from user, supporting 'ans' for last result"""
    while True:
        value = input(prompt).strip().lower()
        
        if allow_ans and value == 'ans':
            return None  # Will use last result
        
        try:
            return float(value)
        except ValueError:
            print("❌ Please enter a valid number!")

def main():
    calc = Calculator()
    
    print("\n🧮 PYTHON CALCULATOR")
    print("=" * 40)
    print("Operations: + - * / ^ % sqrt")
    print("Special: 'ans' for last result")
    print("Commands: history, mem+, mem-, clear, quit")
    print("=" * 40)
    
    while True:
        print()
        user_input = input("Enter expression (e.g., 5 + 3): ").strip().lower()
        
        # Handle commands
        if user_input == 'quit' or user_input == 'exit':
            print("👋 Goodbye!")
            break
        elif user_input == 'history':
            calc.show_history()
            continue
        elif user_input == 'mem+':
            calc.memory_store(calc.last_result)
            continue
        elif user_input == 'mem-':
            calc.memory_recall()
            continue
        elif user_input == 'clear':
            calc.memory_clear()
            continue
        
        # Parse expression
        parts = user_input.split()
        
        try:
            if len(parts) == 2 and parts[0] == 'sqrt':
                # Square root
                num = float(parts[1]) if parts[1] != 'ans' else calc.last_result
                result = calc.calculate(num, 'sqrt')
            elif len(parts) == 3:
                # Binary operation
                a = float(parts[0]) if parts[0] != 'ans' else calc.last_result
                operator = parts[1]
                b = float(parts[2]) if parts[2] != 'ans' else calc.last_result
                result = calc.calculate(a, operator, b)
            else:
                print("❌ Invalid expression! Use: number operator number")
                print("   Example: 5 + 3, sqrt 16, 2 ^ 8")
                continue
            
            print(f"   = {result}")
            
        except ValueError as e:
            print(f"❌ {e}")

if __name__ == "__main__":
    main()
```

### Skills Used
- Mathematical operations
- Exception handling
- String parsing
- Memory management
- Command pattern

---

## 📝 Project Checklist

After completing these projects, you should be able to:

- [ ] Handle user input and validation
- [ ] Use file I/O for data persistence
- [ ] Implement menu-driven interfaces
- [ ] Create and use classes
- [ ] Handle errors gracefully
- [ ] Write clean, readable code

---

## ⏭️ Next: Intermediate Projects

Ready for a challenge? Move on to **[Intermediate Projects](02_Intermediate_Projects.md)** to build more complex applications!

---

*Start small, think big, build often!* 🚀
