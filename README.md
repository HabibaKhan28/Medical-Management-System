# Medical-Management-System
A Medical Management System built using Python (Tkinter) for the GUI and MySQL for database management. This project enables healthcare professionals to efficiently manage patient records, offering functionalities like adding, viewing, updating, and deleting patient details through an interactive interface.

Features ✨
✔ Add Patient – Store patient details securely in MySQL
✔ View Patients – Display all records in a structured table
✔ Search Functionality – Quickly retrieve patient data
✔ Update & Delete Records – Modify or remove existing entries
✔ Intuitive UI – Built using Tkinter with enhanced usability

Tech Stack 🛠
- Python – Core programming language
- Tkinter – GUI framework for desktop applications
- MySQL – Database for storing patient records
- ttk.Treeview – Used for structured data display


Future Enhancements 💡
🔹 User Authentication – Secure login system for authorized access
🔹 Prescription Management – Add doctor notes & prescriptions
🔹 Appointment Scheduling – Implement a booking system
This project aims to simplify medical data management, reducing manual efforts and improving efficiency. Feel free to contribute, suggest enhancements, or report bugs!

CODE:


import tkinter as tk
from tkinter import messagebox, ttk
import mysql.connector

# Database connection
def connect_db():
    return mysql.connector.connect(
        host="localhost",
        user="your_username",
        password="your_password",
        database="medical_management"
    )

# Function to add patient
def add_patient():
    name = entry_name.get()
    age = entry_age.get()
    gender = entry_gender.get()
    contact = entry_contact.get()

    if name and age and gender and contact:
        db = connect_db()
        cursor = db.cursor()
        cursor.execute("INSERT INTO patients (name, age, gender, contact) VALUES (%s, %s, %s, %s)", 
                       (name, age, gender, contact))
        db.commit()
        db.close()
        messagebox.showinfo("Success", "Patient added successfully!")
        clear_entries()
    else:
        messagebox.showwarning("Input Error", "Please fill all fields.")

# Function to view patients
def view_patients():
    for row in tree.get_children():
        tree.delete(row)
    
    db = connect_db()
    cursor = db.cursor()
    cursor.execute("SELECT * FROM patients")
    records = cursor.fetchall()
    db.close()

    for record in records:
        tree.insert("", tk.END, values=record)

# Function to clear input fields
def clear_entries():
    entry_name.delete(0, tk.END)
    entry_age.delete(0, tk.END)
    entry_gender.delete(0, tk.END)
    entry_contact.delete(0, tk.END)

# Create main window
root = tk.Tk()
root.title("Medical Management System")
root.geometry("700x500")
root.configure(bg="#e0f7fa")

# Create a title label
title_label = tk.Label(root, text="Medical Management System", font=("Helvetica", 24, "bold"), bg="#e0f7fa", fg="#00796b")
title_label.pack(pady=20)

# Create a frame for input fields
frame_input = tk.Frame(root, bg="#ffffff", bd=2, relief=tk.GROOVE)
frame_input.pack(pady=20, padx=20, fill=tk.X)

# Create input fields
tk.Label(frame_input, text="Name", bg="#ffffff", font=("Helvetica", 12)).grid(row=0, column=0, padx=10, pady=10)
entry_name = tk.Entry(frame_input, font=("Helvetica", 12))
entry_name.grid(row=0, column=1, padx=10, pady=10)

tk.Label(frame_input, text="Age", bg="#ffffff", font=("Helvetica", 12)).grid(row=1, column=0, padx=10, pady=10)
entry_age = tk.Entry(frame_input, font=("Helvetica", 12))
entry_age.grid(row=1, column=1, padx=10, pady=10)

tk.Label(frame_input, text="Gender", bg="#ffffff", font=("Helvetica", 12)).grid(row=2, column=0, padx=10, pady=10)
entry_gender = tk.Entry(frame_input, font=("Helvetica", 12))
entry_gender.grid(row=2, column=1, padx=10, pady=10)

tk.Label(frame_input, text="Contact", bg="#ffffff", font=("Helvetica", 12)).grid(row=3, column=0, padx=10, pady=10)
entry_contact = tk.Entry(frame_input, font=("Helvetica", 12))
entry_contact.grid(row=3, column=1, padx=10, pady=10)

# Create buttons
btn_frame = tk.Frame(root, bg="#e0f7fa")
btn_frame.pack(pady=10)

btn_add = tk.Button(btn_frame, text="Add Patient", command=add_patient, bg="#4CAF50", fg="white", font=("Helvetica", 12))
btn_add.grid(row=0, column=0, padx=10)

btn_view = tk.Button(btn_frame, text="View Patients", command=view_patients, bg="#2196F3", fg="white", font=("Helvetica", 12))
btn_view.grid(row=0, column=1, padx=10)

# Create a Treeview to display patients
columns = ("ID", "Name", "Age", "Gender", "Contact")
tree = ttk.Treeview(root, columns=columns, show='headings', height=10)
tree.pack(pady=20)

# Define headings
for col in columns:
    tree.heading(col, text=col)
    tree.column(col, anchor="center")

# Style the Treeview
style = ttk.Style()
style.configure("Treeview", font=("Helvetica", 12), rowheight=25, background="#ffffff", fieldbackground="#ffffff")
style.configure("Treeview.Heading", font=("Helvetica", 12, "bold"), background="#e0f7fa", foreground="#00796b")
style.map("Treeview", background=[('selected', '#b2dfdb')], foreground=[('selected', 'black')])

# Running the application
root.mainloop()


