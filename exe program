import customtkinter as ctk
from tkinter import messagebox
from tkinter import ttk
from tkinter.scrolledtext import ScrolledText
import requests
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import numpy as np
from PIL import Image, ImageTk
import nltk
from nltk.chat.util import Chat, reflections
import json
import os
import webbrowser

# Download NLTK data if not already downloaded
nltk.download('punkt')

# Define chat pairs for the NLTK chatbot
pairs = [
    [
        r"my name is (.*)",
        ["Hello %1, How are you today ?",]
    ],
    [
        r"what is your name ?",
        ["My name is AstroBot and I'm here to assist you with astrophysics calculations.",]
    ],
    [
        r"how are you ?",
        ["I'm doing good. How about you ?",]
    ],
    [
        r"sorry (.*)",
        ["It's alright","It's OK, never mind",]
    ],
    [
        r"I am fine",
        ["Great to hear that. How can I assist you today?",]
    ],
    [
        r"quit",
        ["Bye, take care. See you soon :) ","It was nice talking to you. Take care :)"]
    ],
]

# Initialize the NLTK chatbot
chatbot = Chat(pairs, reflections)

# File path for user activity log
activity_log_file = 'user_activity.log'

# Function to load user activity log
def load_user_activity():
    if os.path.exists(activity_log_file):
        with open(activity_log_file, 'r') as file:
            return json.load(file)
    return []

# Function to save user activity log
def save_user_activity(activity):
    with open(activity_log_file, 'w') as file:
        json.dump(activity, file)

# Load previous user activity
user_activity = load_user_activity()

class AstroApp(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Astrophysics Calculator and Chatbot")
        self.geometry("1200x900")
        self.iconbitmap('C:/path/to/your/icon.ico')  # Replace with the actual path to your icon

        # Set space-themed background
        self.background_image = ctk.CTkImage(Image.open("path/to/space_background.jpg"))  # Replace with the actual path to your image
        self.background_label = ctk.CTkLabel(self, image=self.background_image)
        self.background_label.place(relwidth=1, relheight=1)

        self.create_widgets()

    def create_widgets(self):
        # Configure the grid layout
        self.grid_columnconfigure(0, weight=1)
        self.grid_rowconfigure(0, weight=1)

        # Create a Notebook widget
        self.notebook = ttk.Notebook(self)
        self.notebook.grid(row=0, column=0, sticky="nsew")

        # Add tabs to the Notebook
        self.create_calculator_tab()
        self.create_chatbot_tab()
        self.create_articles_tab()
        self.create_history_tab()
        self.create_constants_tab()
        self.create_formulas_tab()
        self.create_plots_tab()
        self.create_suggestions_tab()

        # Add website link
        self.website_button = ctk.CTkButton(self, text="Visit HorizonData", command=self.open_website)
        self.website_button.grid(row=1, column=0, pady=10)

    def create_calculator_tab(self):
        self.calculator_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.calculator_frame, text="Calculations")

        # Add widgets for calculations
        self.calculation_type_label = ctk.CTkLabel(self.calculator_frame, text="Select Calculation:")
        self.calculation_type_label.grid(row=0, column=0, padx=10, pady=10)
        self.calculation_type = ctk.CTkComboBox(self.calculator_frame, values=[
            "Gravitational Force", "Kinetic Energy", "Escape Velocity", "Orbital Period",
            "Luminosity", "Blackbody Radiation", "Distance Modulus", "Redshift",
            "Schwarzschild Radius", "Stellar Magnitude"
        ])
        self.calculation_type.grid(row=0, column=1, padx=10, pady=10)
        self.calculation_type.bind("<<ComboboxSelected>>", self.update_inputs)

        self.mass_label = ctk.CTkLabel(self.calculator_frame, text="Mass (kg):")
        self.mass_label.grid(row=1, column=0, padx=10, pady=10)
        self.mass_entry = ctk.CTkEntry(self.calculator_frame)
        self.mass_entry.grid(row=1, column=1, padx=10, pady=10)

        self.radius_label = ctk.CTkLabel(self.calculator_frame, text="Radius (m):")
        self.radius_label.grid(row=2, column=0, padx=10, pady=10)
        self.radius_entry = ctk.CTkEntry(self.calculator_frame)
        self.radius_entry.grid(row=2, column=1, padx=10, pady=10)

        self.velocity_label = ctk.CTkLabel(self.calculator_frame, text="Velocity (m/s):")
        self.velocity_label.grid(row=3, column=0, padx=10, pady=10)
        self.velocity_entry = ctk.CTkEntry(self.calculator_frame)
        self.velocity_entry.grid(row=3, column=1, padx=10, pady=10)

        self.temperature_label = ctk.CTkLabel(self.calculator_frame, text="Temperature (K):")
        self.temperature_label.grid(row=4, column=0, padx=10, pady=10)
        self.temperature_entry = ctk.CTkEntry(self.calculator_frame)
        self.temperature_entry.grid(row=4, column=1, padx=10, pady=10)

        self.distance_label = ctk.CTkLabel(self.calculator_frame, text="Distance (m):")
        self.distance_label.grid(row=5, column=0, padx=10, pady=10)
        self.distance_entry = ctk.CTkEntry(self.calculator_frame)
        self.distance_entry.grid(row=5, column=1, padx=10, pady=10)

        self.redshift_label = ctk.CTkLabel(self.calculator_frame, text="Redshift (z):")
        self.redshift_label.grid(row=6, column=0, padx=10, pady=10)
        self.redshift_entry = ctk.CTkEntry(self.calculator_frame)
        self.redshift_entry.grid(row=6, column=1, padx=10, pady=10)

        self.calculate_button = ctk.CTkButton(self.calculator_frame, text="Calculate", command=self.calculate)
        self.calculate_button.grid(row=7, column=0, columnspan=2, padx=10, pady=10)

        self.result_label = ctk.CTkLabel(self.calculator_frame, text="")
        self.result_label.grid(row=8, column=0, columnspan=2, padx=10, pady=10)

    def update_inputs(self, event):
        calculation = self.calculation_type.get()
        self.mass_label.grid_forget()
        self.mass_entry.grid_forget()
        self.radius_label.grid_forget()
        self.radius_entry.grid_forget()
        self.velocity_label.grid_forget()
        self.velocity_entry.grid_forget()
        self.temperature_label.grid_forget()
        self.temperature_entry.grid_forget()
        self.distance_label.grid_forget()
        self.distance_entry.grid_forget()
        self.redshift_label.grid_forget()
        self.redshift_entry.grid_forget()

        if calculation in ["Gravitational Force", "Escape Velocity", "Orbital Period", "Luminosity"]:
            self.mass_label.grid(row=1, column=0, padx=10, pady=10)
            self.mass_entry.grid(row=1, column=1, padx=10, pady=10)
        if calculation in ["Gravitational Force", "Escape Velocity", "Orbital Period"]:
            self.radius_label.grid(row=2, column=0, padx=10, pady=10)
            self.radius_entry.grid(row=2, column=1, padx=10, pady=10)
        if calculation == "Kinetic Energy":
            self.velocity_label.grid(row=3, column=0, padx=10, pady=10)
            self.velocity_entry.grid(row=3, column=1, padx=10, pady=10)
        if calculation == "Blackbody Radiation":
            self.temperature_label.grid(row=4, column=0, padx=10, pady=10)
            self.temperature_entry.grid(row=4, column=1, padx=10, pady=10)
        if calculation == "Distance Modulus":
            self.distance_label.grid(row=5, column=0, padx=10, pady=10)
            self.distance_entry.grid(row=5, column=1, padx=10, pady=10)
        if calculation == "Redshift":
            self.redshift_label.grid(row=6, column=0, padx=10, pady=10)
            self.redshift_entry.grid(row=6, column=1, padx=10, pady=10)

    def calculate(self):
        try:
            calculation = self.calculation_type.get()
            if calculation == "Gravitational Force":
                mass = float(self.mass_entry.get())
                radius = float(self.radius_entry.get())
                gravity = 6.67430e-11 * mass / (radius ** 2)
                self.result_label.configure(text=f"Gravitational Force: {gravity:.2e} N")
            elif calculation == "Kinetic Energy":
                mass = float(self.mass_entry.get())
                velocity = float(self.velocity_entry.get())
                kinetic_energy = 0.5 * mass * (velocity ** 2)
                self.result_label.configure(text=f"Kinetic Energy: {kinetic_energy:.2e} J")
            elif calculation == "Escape Velocity":
                mass = float(self.mass_entry.get())
                radius = float(self.radius_entry.get())
                escape_velocity = np.sqrt(2 * 6.67430e-11 * mass / radius)
                self.result_label.configure(text=f"Escape Velocity: {escape_velocity:.2e} m/s")
            elif calculation == "Orbital Period":
                mass = float(self.mass_entry.get())
                radius = float(self.radius_entry.get())
                orbital_period = 2 * np.pi * np.sqrt(radius**3 / (6.67430e-11 * mass))
                self.result_label.configure(text=f"Orbital Period: {orbital_period:.2e} s")
            elif calculation == "Luminosity":
                mass = float(self.mass_entry.get())
                luminosity = 3.828e26 * (mass / 1.989e30) ** 3.5
                self.result_label.configure(text=f"Luminosity: {luminosity:.2e} W")
            elif calculation == "Blackbody Radiation":
                temperature = float(self.temperature_entry.get())
                blackbody_radiation = 5.670374419e-8 * temperature ** 4
                self.result_label.configure(text=f"Blackbody Radiation: {blackbody_radiation:.2e} W/m²")
            elif calculation == "Distance Modulus":
                distance = float(self.distance_entry.get())
                distance_modulus = 5 * (np.log10(distance) - 1)
                self.result_label.configure(text=f"Distance Modulus: {distance_modulus:.2f} mag")
            elif calculation == "Redshift":
                redshift = float(self.redshift_entry.get())
                speed_of_light = 299792458  # m/s
                velocity = redshift * speed_of_light
                self.result_label.configure(text=f"Velocity due to Redshift: {velocity:.2e} m/s")
            elif calculation == "Schwarzschild Radius":
                mass = float(self.mass_entry.get())
                schwarzschild_radius = 2 * 6.67430e-11 * mass / (299792458 ** 2)
                self.result_label.configure(text=f"Schwarzschild Radius: {schwarzschild_radius:.2e} m")
            elif calculation == "Stellar Magnitude":
                luminosity = float(self.luminosity_entry.get())
                distance = float(self.distance_entry.get())
                magnitude = -2.5 * np.log10(luminosity / (4 * np.pi * (distance ** 2) * 3.828e26))
                self.result_label.configure(text=f"Stellar Magnitude: {magnitude:.2f}")
            else:
                self.result_label.configure(text="Select a valid calculation.")
            user_activity.append(calculation)
            save_user_activity(user_activity)
            self.update_suggestions()
            self.update_history()
        except ValueError:
            messagebox.showerror("Error", "Please enter valid numbers.")
        except Exception as e:
            messagebox.showerror("Error", f"An error occurred: {e}")

    def create_chatbot_tab(self):
        self.chatbot_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.chatbot_frame, text="Chatbot")

        # Add widgets for chatbot interaction
        self.chat_log = ScrolledText(self.chatbot_frame, state='disabled', height=15)
        self.chat_log.grid(row=0, column=0, columnspan=2, padx=10, pady=10)

        self.user_input = ctk.CTkEntry(self.chatbot_frame, placeholder_text="Type your question here...")
        self.user_input.grid(row=1, column=0, padx=10, pady=10, sticky="ew")

        self.send_button = ctk.CTkButton(self.chatbot_frame, text="Send", command=self.send_message)
        self.send_button.grid(row=1, column=1, padx=10, pady=10)

    def send_message(self):
        user_message = self.user_input.get()
        if user_message:
            self.chat_log.configure(state='normal')
            self.chat_log.insert('end', f"User: {user_message}\n")
            self.chat_log.configure(state='disabled')
            self.user_input.delete(0, 'end')
            self.get_chatbot_response(user_message)

    def get_chatbot_response(self, message):
        try:
            response = chatbot.respond(message)
            if response:
                self.chat_log.configure(state='normal')
                self.chat_log.insert('end', f"Chatbot: {response}\n")
                self.chat_log.configure(state='disabled')
            else:
                self.chat_log.configure(state='normal')
                self.chat_log.insert('end', "Chatbot: I did not understand that.\n")
                self.chat_log.configure(state='disabled')
        except Exception as e:
            messagebox.showerror("Error", f"Failed to get response from chatbot: {e}")

    def create_articles_tab(self):
        self.articles_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.articles_frame, text="Articles")

        # Add widgets for articles
        self.articles_log = ScrolledText(self.articles_frame, state='disabled', height=15)
        self.articles_log.grid(row=0, column=0, columnspan=2, padx=10, pady=10)

        self.fetch_articles_button = ctk.CTkButton(self.articles_frame, text="Fetch Articles", command=self.fetch_articles)
        self.fetch_articles_button.grid(row=1, column=0, columnspan=2, padx=10, pady=10)

    def fetch_articles(self):
        try:
            api_key = "your_newsroom_api_key_here"  # Replace with your Newsroom API key
            response = requests.get(f"https://newsroomapi.io/api/v1/news?apiKey={api_key}")
            articles = response.json().get("data", [])
            self.articles_log.configure(state='normal')
            self.articles_log.delete(1.0, 'end')
            for article in articles:
                self.articles_log.insert('end', f"Title: {article['title']}\nLink: {article['url']}\n\n")
            self.articles_log.configure(state='disabled')
        except Exception as e:
            messagebox.showerror("Error", f"Failed to fetch articles: {e}")

    def create_history_tab(self):
        self.history_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.history_frame, text="History")

        # Add widgets for history
        self.history_log = ScrolledText(self.history_frame, state='disabled', height=15)
        self.history_log.grid(row=0, column=0, columnspan=2, padx=10, pady=10)
        self.update_history()

    def update_history(self):
        self.history_log.configure(state='normal')
        self.history_log.delete(1.0, 'end')
        for activity in user_activity:
            self.history_log.insert('end', f"{activity}\n")
        self.history_log.configure(state='disabled')

    def create_constants_tab(self):
        self.constants_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.constants_frame, text="Constants")

        constants = {
            "Gravitational Constant": "6.67430e-11 m^3 kg^-1 s^-2",
            "Speed of Light": "299792458 m/s",
            "Planck's Constant": "6.62607015e-34 m^2 kg / s",
            "Elementary Charge": "1.602176634e-19 C",
            "Boltzmann Constant": "1.380649e-23 J/K",
        }

        for i, (const, value) in enumerate(constants.items()):
            ctk.CTkLabel(self.constants_frame, text=f"{const}: {value}").grid(row=i, column=0, padx=10, pady=10)

    def create_formulas_tab(self):
        self.formulas_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.formulas_frame, text="Formulas")

        formulas = {
            "Gravitational Force": "F = G * (m1 * m2) / r^2",
            "Kinetic Energy": "KE = 0.5 * m * v^2",
            "Einstein's Mass-Energy Equivalence": "E = mc^2",
            "Planck's Law": "E = h * f",
            "Ideal Gas Law": "PV = nRT",
            "Luminosity": "L = 3.828e26 * (M / 1.989e30)^3.5",
            "Blackbody Radiation": "E = σ * T^4",
            "Distance Modulus": "m - M = 5 log10(d) - 5",
            "Redshift": "v = z * c",
            "Schwarzschild Radius": "R_s = 2 * G * M / c^2",
            "Stellar Magnitude": "m = -2.5 * log10(L / (4 * π * d^2 * L_sun))"
        }

        for i, (formula, equation) in enumerate(formulas.items()):
            ctk.CTkLabel(self.formulas_frame, text=f"{formula}: {equation}").grid(row=i, column=0, padx=10, pady=10)

    def create_plots_tab(self):
        self.plots_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.plots_frame, text="Plots")

        self.plot_button = ctk.CTkButton(self.plots_frame, text="Create Plot", command=self.create_plot)
        self.plot_button.grid(row=0, column=0, padx=10, pady=10)

        self.plot_type_label = ctk.CTkLabel(self.plots_frame, text="Select Plot Type:")
        self.plot_type_label.grid(row=1, column=0, padx=10, pady=10)
        self.plot_type = ctk.CTkComboBox(self.plots_frame, values=["Gravitational Force vs Distance", "Kinetic Energy vs Velocity"])
        self.plot_type.grid(row=1, column=1, padx=10, pady=10)

        self.save_plot_button = ctk.CTkButton(self.plots_frame, text="Save Plot", command=self.save_plot)
        self.save_plot_button.grid(row=2, column=0, padx=10, pady=10)

    def create_plot(self):
        plot_type = self.plot_type.get()
        if plot_type == "Gravitational Force vs Distance":
            self.plot_gravitational_force_vs_distance()
        elif plot_type == "Kinetic Energy vs Velocity":
            self.plot_kinetic_energy_vs_velocity()
        else:
            messagebox.showerror("Error", "Select a valid plot type.")

    def plot_gravitational_force_vs_distance(self):
        mass = float(self.mass_entry.get())
        distances = np.linspace(1e6, 1e7, 100)
        forces = 6.67430e-11 * mass / distances ** 2

        self.fig, self.ax = plt.subplots()
        self.ax.plot(distances, forces)
        self.ax.set_xlabel('Distance (m)')
        self.ax.set_ylabel('Gravitational Force (N)')
        self.ax.set_title('Gravitational Force vs Distance')
        self.ax.grid(True)

        canvas = FigureCanvasTkAgg(self.fig, master=self.plots_frame)
        canvas.draw()
        canvas.get_tk_widget().grid(row=3, column=0, columnspan=2, padx=10, pady=10)

    def plot_kinetic_energy_vs_velocity(self):
        mass = float(self.mass_entry.get())
        velocities = np.linspace(1e3, 1e4, 100)
        energies = 0.5 * mass * velocities ** 2

        self.fig, self.ax = plt.subplots()
        self.ax.plot(velocities, energies)
        self.ax.set_xlabel('Velocity (m/s)')
        self.ax.set_ylabel('Kinetic Energy (J)')
        self.ax.set_title('Kinetic Energy vs Velocity')
        self.ax.grid(True)

        canvas = FigureCanvasTkAgg(self.fig, master=self.plots_frame)
        canvas.draw()
        canvas.get_tk_widget().grid(row=3, column=0, columnspan=2, padx=10, pady=10)

    def save_plot(self):
        plot_type = self.plot_type.get()
        if plot_type:
            file_path = f"{plot_type.replace(' ', '_').lower()}.png"
            self.fig.savefig(file_path)
            messagebox.showinfo("Plot Saved", f"Plot saved as {file_path}")

    def create_suggestions_tab(self):
        self.suggestions_frame = ctk.CTkFrame(self.notebook)
        self.notebook.add(self.suggestions_frame, text="Suggestions")

        self.suggestions_log = ScrolledText(self.suggestions_frame, state='disabled', height=15)
        self.suggestions_log.grid(row=0, column=0, columnspan=2, padx=10, pady=10)
        self.update_suggestions()

    def update_suggestions(self):
        self.suggestions_log.configure(state='normal')
        self.suggestions_log.delete(1.0, 'end')
        suggestions = {
            "Gravitational Force": "Check out the Universal Law of Gravitation.",
            "Kinetic Energy": "Learn more about Kinetic Energy in classical mechanics.",
            "Escape Velocity": "Explore Escape Velocity concepts in space science.",
            "Orbital Period": "Understand Orbital Period calculations for celestial bodies.",
            "Luminosity": "Read about Stellar Luminosity and its significance.",
            "Blackbody Radiation": "Study Blackbody Radiation and Planck's Law.",
            "Distance Modulus": "Learn how Distance Modulus is used in astronomy.",
            "Redshift": "Explore the concept of Redshift in cosmology.",
            "Schwarzschild Radius": "Understand the Schwarzschild Radius of black holes.",
            "Stellar Magnitude": "Learn about Stellar Magnitude and its calculation."
        }
        for activity in set(user_activity):
            suggestion = suggestions.get(activity, "")
            if suggestion:
                self.suggestions_log.insert('end', f"{activity}: {suggestion}\n")
        self.suggestions_log.configure(state='disabled')

    def open_website(self):
        webbrowser.open("https://horizondata.ir")

if __name__ == "__main__":
    app = AstroApp()
    app.mainloop()
