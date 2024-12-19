import tkinter as tk
from tkinter import ttk, messagebox
import subprocess


def get_wifi_profiles():
    try:
        
        profiles_data = subprocess.check_output("netsh wlan show profiles", shell=True).decode("utf-8", errors="backslashreplace")
        profiles = [line.split(":")[1].strip() for line in profiles_data.split("\n") if "All User Profile" in line]
        
        wifi_info = []
        for profile in profiles:
            try:
                
                profile_info = subprocess.check_output(f'netsh wlan show profile "{profile}" key=clear', shell=True).decode("utf-8", errors="backslashreplace")
                password_line = [line.split(":")[1].strip() for line in profile_info.split("\n") if "Key Content" in line]
                password = password_line[0] if password_line else "No Password"
                wifi_info.append((profile, password))
            except subprocess.CalledProcessError:
                wifi_info.append((profile, "Error retrieving password"))
        return wifi_info
    except Exception as e:
        messagebox.showerror("Error", f"Error retrieving Wi-Fi profiles: {e}")
        return []


def show_wifi_info():
    wifi_info = get_wifi_profiles()
    for i, (profile, password) in enumerate(wifi_info):
        tree.insert("", "end", values=(profile, password))


root = tk.Tk()
root.title("Wi-Fi Viewer")
root.geometry("400x300")

tree = ttk.Treeview(root, columns=("Profile", "Password"), show="headings")
tree.heading("Profile", text="Wi-Fi Name")
tree.heading("Password", text="Password")
tree.pack(fill="both", expand=True)


refresh_button = ttk.Button(root, text="Refresh", command=show_wifi_info)
refresh_button.pack(pady=10)

root.mainloop()
