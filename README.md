# inventory-management-system
import tkinter as tk
from tkinter import messagebox

# Sample user authentication data
users = {"admin": "password123"}

# Inventory data structure
inventory = {}

# Authentication function
def authenticate(username, password):
    return users.get(username) == password

# Add product to inventory
def add_product(product_id, name, quantity, price):
    if product_id in inventory:
        messagebox.showerror("Error", "Product ID already exists.")
    else:
        inventory[product_id] = {
            "name": name,
            "quantity": int(quantity),
            "price": float(price)
        }
        messagebox.showinfo("Success", "Product added successfully.")

# Edit product details
def edit_product(product_id, name, quantity, price):
    if product_id not in inventory:
        messagebox.showerror("Error", "Product ID not found.")
    else:
        inventory[product_id].update({
            "name": name,
            "quantity": int(quantity),
            "price": float(price)
        })
        messagebox.showinfo("Success", "Product updated successfully.")

# Delete product from inventory
def delete_product(product_id):
    if product_id not in inventory:
        messagebox.showerror("Error", "Product ID not found.")
    else:
        del inventory[product_id]
        messagebox.showinfo("Success", "Product deleted successfully.")

# Generate low-stock report
def generate_low_stock_report():
    low_stock = [f"ID: {pid}, Name: {details['name']}, Quantity: {details['quantity']}"
                 for pid, details in inventory.items() if details['quantity'] < 5]
    report = "\n".join(low_stock) if low_stock else "No low-stock items."
    messagebox.showinfo("Low Stock Report", report)

# GUI implementation
def main():
    def handle_login():
        if authenticate(entry_username.get(), entry_password.get()):
            login_frame.pack_forget()
            main_frame.pack(fill="both", expand=True)
        else:
            messagebox.showerror("Error", "Invalid credentials.")

    app = tk.Tk()
    app.title("Inventory Management System")

    login_frame = tk.Frame(app)
    login_frame.pack(pady=50)

    tk.Label(login_frame, text="Username:").grid(row=0, column=0, pady=5)
    entry_username = tk.Entry(login_frame)
    entry_username.grid(row=0, column=1, pady=5)

    tk.Label(login_frame, text="Password:").grid(row=1, column=0, pady=5)
    entry_password = tk.Entry(login_frame, show="*")
    entry_password.grid(row=1, column=1, pady=5)

    tk.Button(login_frame, text="Login", command=handle_login).grid(row=2, columnspan=2, pady=10)

    main_frame = tk.Frame(app)

    # Additional widgets for main functionality would be added here.

    app.mainloop()

if __name__ == "__main__":
    main()
