
Libraries used:
1)Tkinter
2)random

External sources:
1) https://www.youtube.com/watch?v=6wWTaRYzLyA
2) https://unicode.org/emoji/charts/full-emoji-list.html

Taken inspiration from:
 https://www.youtube.com/watch?v=6wWTaRYzLyA
 <!---
pythonprogrammes1403/pythonprogrammes1403 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->


import tkinter as tk

LARGE_FONT_STYLE = ("Calibri Light", 40)
SMALL_FONT_STYLE = ("Calibri Light", 16)
DIGITS_FONT_STYLE = ("Calibri Light", 24)
DEFAULT_FONT_STYLE = ("Calibri Light", 20)
import random
lst=["\U0001F63A","\U0001F601","\U0001F602","\U0001F606","\U0001F923","\U0001F642","\U0001F60A","\U0001F60B","\U0001F603","\U0001F604","\U0001F605","\U0001F923","\U0001F642"\
         "\U0001F643","\U0001F607","\U0001F970","\U0001F929","\U0001F618","\U0001F617","\U0001F639","\U0001F649"]

BLACK = "#323232"
ALICE_BLUE = "#1d548a"
GRAY = "#191919"
WHITE = "#e5e5e5"
YELLOW= '#f8d708'


class Calculator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.geometry("320x500")
        self.window.resizable(0, 0)
        self.window.title("Calculator")

        self.total_expression = ""
        self.current_expression = ""
        self.display_frame = self.create_display_frame()

        self.total_label, self.label = self.create_display_labels()

        self.digits = {
            7: (1, 1), 8: (1, 2), 9: (1, 3),
            4: (2, 1), 5: (2, 2), 6: (2, 3),
            1: (3, 1), 2: (3, 2), 3: (3, 3),
            0: (4, 2), '.': (4, 1)
        }
        self.operations = {"/": "\u00F7", "*": "\u00D7", "-": "-", "+": "+"}
        self.buttons_frame = self.create_buttons_frame()

        self.buttons_frame.rowconfigure(0, weight=1)
        for x in range(1, 5):
            self.buttons_frame.rowconfigure(x, weight=1)
            self.buttons_frame.columnconfigure(x, weight=1)
        self.create_digit_buttons()
        self.create_operator_buttons()
        self.create_special_buttons()
        self.bind_keys()

    def bind_keys(self):
        self.window.bind("<Return>", lambda event: self.evaluate())
        for key in self.digits:
            self.window.bind(str(key), lambda event, digit=key: self.add_to_expression(digit))

        for key in self.operations:
            self.window.bind(key, lambda event, operator=key: self.append_operator(operator))

    def create_special_buttons(self):
        self.create_clear_button()
        self.create_equals_button()
        self.create_square_button()
        self.create_sqrt_button()

    def create_display_labels(self):
        total_label = tk.Label(self.display_frame, text=self.total_expression, anchor=tk.E, bg=GRAY,
                               fg=WHITE, padx=24, font=SMALL_FONT_STYLE)
        total_label.pack(expand=True, fill='both')

        label = tk.Label(self.display_frame, text=self.current_expression, anchor=tk.E, bg=GRAY,
                         fg=WHITE, padx=24, font=LARGE_FONT_STYLE)
        label.pack(expand=True, fill='both')

        return total_label, label

    def create_display_frame(self):
        frame = tk.Frame(self.window, height=221, bg=GRAY)
        frame.pack(expand=True, fill="both")
        return frame

    def add_to_expression(self, value):
        self.current_expression += str(value)
        self.update_label()

    def create_digit_buttons(self):
        for digit, grid_value in self.digits.items():
            button = tk.Button(self.buttons_frame, text=str(digit), bg=BLACK, fg=WHITE, font=DIGITS_FONT_STYLE,
                               borderwidth=0, command=lambda x=digit: self.add_to_expression(x))
            button.grid(row=grid_value[0], column=grid_value[1], sticky=tk.NSEW)

    def append_operator(self, operator):
        self.current_expression += operator
        self.total_expression += self.current_expression
        self.current_expression = ""
        self.update_total_label()
        self.update_label()

    def create_operator_buttons(self):
        i = 0
        for operator, symbol in self.operations.items():
            button = tk.Button(self.buttons_frame, text=symbol, bg=GRAY, fg=WHITE, font=DEFAULT_FONT_STYLE,
                               borderwidth=0, command=lambda x=operator: self.append_operator(x))
            button.grid(row=i, column=4, sticky=tk.NSEW)
            i += 1

    def clear(self):
        self.current_expression = ""
        self.total_expression = ""
        self.update_label()
        self.update_total_label()

    def create_clear_button(self):
        button = tk.Button(self.buttons_frame, text="C", bg=GRAY, fg=WHITE, font=DEFAULT_FONT_STYLE,
                           borderwidth=0, command=self.clear)
        button.grid(row=0, column=1, sticky=tk.NSEW)

    def square(self):
        self.current_expression = random.choice(lst)
        self.update_label()

    def create_square_button(self):
        button = tk.Button(self.buttons_frame, text="x\u00b2", bg=GRAY, fg=WHITE, font=DEFAULT_FONT_STYLE,
                           borderwidth=0, command=self.square)
        button.grid(row=0, column=2, sticky=tk.NSEW)

    def sqrt(self):
        self.current_expression = random.choice(lst)
        self.update_label()

    def create_sqrt_button(self):
        button = tk.Button(self.buttons_frame, text="\u221ax", bg=GRAY, fg=WHITE, font=DEFAULT_FONT_STYLE,
                           borderwidth=0, command=self.sqrt)
        button.grid(row=0, column=3, sticky=tk.NSEW)

    def evaluate(self):
        self.update_total_label()
        try:
            self.current_expression = random.choice(lst)
        finally:
            self.update_label()

    def create_equals_button(self):
        button = tk.Button(self.buttons_frame, text="=", bg=ALICE_BLUE, fg=WHITE, font=DEFAULT_FONT_STYLE,
                           borderwidth=0, command=self.evaluate)
        button.grid(row=4, column=3, columnspan=2, sticky=tk.NSEW)

    def create_buttons_frame(self):
        frame = tk.Frame(self.window)
        frame.pack(expand=True, fill="both")
        return frame

    def update_total_label(self):
        expression = self.total_expression
        for operator, symbol in self.operations.items():
            expression = expression.replace(operator, f' {symbol} ')
        self.total_label.config(text=expression)

    def update_label(self):
        self.label.config(text=self.current_expression[:11])

    def run(self):
        self.window.mainloop()


if __name__ == "__main__":
    calc = Calculator()
    calc.run()
