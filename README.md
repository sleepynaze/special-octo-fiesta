# special-octo-fiesta
import random
import time
import tkinter as tk

def generate_problem():
    OPERATORS = ["+", "-", "*"]
    MIN_OPERAND = 9
    MAX_OPERAND = 56
    left = random.randint(MIN_OPERAND, MAX_OPERAND)
    right = random.randint(MIN_OPERAND, MAX_OPERAND)
    operator = random.choice(OPERATORS)
    expr = str(left) + " " + operator + " " + str(right)
    answer = eval(expr)
    return expr, answer

def check_answer():
    global wrong, start_time, i
    expr, answer = problems[i]
    guess = answer_entry.get()
    if guess == str(answer):
        i += 1
        if i < TOTAL_PROBLEMS:
            next_problem()
        else:
            end_quiz()
    else:
        wrong += 1
        result_label.config(text="Incorrect! Try again.")
    answer_entry.delete(0, tk.END)

def next_problem():
    global i
    expr, answer = problems[i]
    problem_label.config(text="Problem #" + str(i+1) + ":   " + expr + " = ")
    result_label.config(text="")

def end_quiz():
    end_time = time.time()
    total_time = round(end_time - start_time, 2)
    result_label.config(text="Well done! That only took you {} seconds and you got {} wrong.".format(total_time, wrong))
    answer_entry.config(state=tk.DISABLED)

def start_quiz():
    global start_time, problems, i
    start_time = time.time()
    problems = [generate_problem() for _ in range(TOTAL_PROBLEMS)]
    i = 0
    next_problem()

# Global variables
TOTAL_PROBLEMS = 10
problems = []
i = 0
wrong = 0

# Create the main window
root = tk.Tk()
root.title("Zain's Math Quiz")

# Label to display problems
problem_label = tk.Label(root, text="Press start when you're ready.", font=('Helvetica', 14))
problem_label.pack()

# Entry widget to enter answers
answer_entry = tk.Entry(root)
answer_entry.pack()
answer_entry.bind("<Return>", lambda event: check_answer())

# Button to start the quiz
start_button = tk.Button(root, text="Start", command=start_quiz)
start_button.pack()

# Label to display results or errors
result_label = tk.Label(root, text="", font=('Helvetica', 14))
result_label.pack()

# Run the GUI event loop
root.mainloop()
