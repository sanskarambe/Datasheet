import csv
import os

# File name for storing expenses
FILE_NAME = "expenses.csv"

# Initialize CSV file if it doesn't exist
def initialize_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, mode="w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Amount", "Description"])  # Header row

# Function to add an expense
def add_expense():
    date = input("Enter date (YYYY-MM-DD): ")
    category = input("Enter category (Food, Transport, Entertainment, etc.): ")

    # Validate amount input
    try:
        amount = float(input("Enter amount: "))
    except ValueError:
        print("❌ Invalid amount! Please enter a number.")
        return

    description = input("Enter description: ")

    # Store expense in CSV file
    with open(FILE_NAME, mode="a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([date, category, amount, description])

    print("✅ Expense added successfully!")

# Function to view all expenses
def view_expenses():
    try:
        with open(FILE_NAME, mode="r") as file:
            reader = csv.reader(file)
            next(reader)  # Skip header row
            print("\n📜 All Expenses:")
            for row in reader:
                print(f"📅 Date: {row[0]}, 📂 Category: {row[1]}, 💰 Amount: ₹{row[2]}, 📝 Description: {row[3]}")
    except FileNotFoundError:
        print("❌ No expense records found.")

# Function to analyze expenses
def analyze_expenses():
    expenses = {}
    total = 0

    try:
        with open(FILE_NAME, mode="r") as file:
            reader = csv.reader(file)
            next(reader)  # Skip header row
            for row in reader:
                category = row[1]
                amount = float(row[2])
                total += amount
                expenses[category] = expenses.get(category, 0) + amount

        print(f"\n💰 Total Expenses: ₹{total}")
        print("📊 Category-wise Breakdown:")
        for category, amt in expenses.items():
            print(f"   {category}: ₹{amt}")
    except FileNotFoundError:
        print("❌ No expense records found.")

# Main menu function
def main():
    initialize_file()
    while True:
        print("\n📌 Expense Tracker Menu:")
        print("1️⃣ Add Expense")
        print("2️⃣ View Expenses")
        print("3️⃣ Analyze Expenses")
        print("4️⃣ Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            analyze_expenses()
        elif choice == "4":
            print("👋 Exiting... Have a great day!")
            break
        else:
            print("❌ Invalid choice. Please try again.")

# Run the program
if __name__ == "__main__":
    main()
