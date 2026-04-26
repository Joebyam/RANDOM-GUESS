[RANDOM GENERATOR.py](https://github.com/user-attachments/files/27104427/RANDOM.GENERATOR.py)
import threading
import tkinter as tk

secret_number = 9
guess_limit = 7

# --- GUI Logic ---
def run_gui():
    guess_count = 0  # local to GUI

    def check_guess():
        nonlocal guess_count
        try:
            guess = int(entry.get())
        except ValueError:
            result_label.config(text="⚠️ Invalid input! Please enter a number.")
            return

        guess_count += 1
        remaining = guess_limit - guess_count

        if guess == secret_number:
            result_label.config(text=f"🎉 You won! The number was {secret_number}. Great job!")
            attempts_label.config(text="Game finished.")
        elif guess_count >= guess_limit:
            result_label.config(text=f"💀 Game Over! The correct number was {secret_number}.")
            attempts_label.config(text="No attempts left.")
        elif guess < secret_number:
            if secret_number - guess <= 2:
                result_label.config(text="🔥 Very close, but still a bit low!")
            else:
                result_label.config(text="Too low! Keep trying!")
            attempts_label.config(text=f"Attempts left: {remaining}")
        else:
            if guess - secret_number <= 2:
                result_label.config(text="🔥 Very close, but just a bit high!")
            else:
                result_label.config(text="Too high! Don’t give up!")
            attempts_label.config(text=f"Attempts left: {remaining}")

    def reset_game():
        nonlocal guess_count
        guess_count = 0
        entry.delete(0, tk.END)
        result_label.config(text="")
        attempts_label.config(text=f"You have {guess_limit} attempts.")

    root = tk.Tk()
    root.title("Number Guessing Game")
    root.geometry("500x300")  # make GUI bigger

    tk.Label(root, text="Enter your guess:", font=("Arial", 14)).pack(pady=10)
    entry = tk.Entry(root, font=("Arial", 14))
    entry.pack(pady=10)

    tk.Button(root, text="Submit", command=check_guess, font=("Arial", 12)).pack(pady=10)
    result_label = tk.Label(root, text="", font=("Arial", 12))
    result_label.pack(pady=10)

    # Initial message about attempts
    attempts_label = tk.Label(root, text=f"You have {guess_limit} attempts.", font=("Arial", 12))
    attempts_label.pack(pady=10)

    # Reset button
    tk.Button(root, text="Reset Game", command=reset_game, font=("Arial", 12)).pack(pady=10)

    root.mainloop()

# --- Console Logic ---
def run_console():
    guess_count = 0
    while guess_count < guess_limit:
        try:
            guess = int(input("Guess: "))
        except ValueError:
            print("⚠️ Invalid input! Please enter a number.")
            continue  

        guess_count += 1

        if guess == secret_number:
            print(f"🎉 You won! The number was {secret_number}. Great job!")
            break
        elif guess < secret_number:
            if secret_number - guess <= 2:
                print("🔥 Very close, but still a bit low!")
            else:
                print("Too low! Keep trying!")
        else:
            if guess - secret_number <= 2:
                print("🔥 Very close, but just a bit high!")
            else:
                print("Too high! Don’t give up!")

    else:
        print(f"💀 Game Over! The correct number was {secret_number}.")

# --- Run both in parallel ---
threading.Thread(target=run_gui, daemon=True).start()
run_console()
