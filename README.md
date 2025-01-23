# cot-4500-Pro1
Assignment 1


import math

# Question 1: Double precision calculation
binary_string = "010000000111111010111001"

def binary_to_decimal(binary):
    sign = int(binary[0], 2)
    exponent = int(binary[1:12], 2) - 1023  # Exponent part, bias = 1023
    mantissa = 1 + sum(int(bit) * 2**-(i + 1) for i, bit in enumerate(binary[12:]))
    value = ((-1)**sign) * mantissa * (2**exponent)
    return round(value, 5)

result_double = binary_to_decimal(binary_string)
print(f"1. {result_double}")

# Question 2: Three-digit chopping arithmetic
def chop_to_three_digits(value):
    value_str = f"{value:.10g}"
    if "e" in value_str:  # Handle scientific notation
        coeff, exp = value_str.split("e")
        chopped_coeff = coeff[:4]
        return float(f"{chopped_coeff}e{exp}")
    else:
        return float(value_str[:4])

result_chopped = chop_to_three_digits(result_double)
print(f"2. {result_chopped}")

# Question 3: Three-digit rounding arithmetic
def round_to_three_digits(value):
    return round(value, 3 - int(math.floor(math.log10(abs(value))) + 1))

result_rounded = round_to_three_digits(result_double)
print(f"3. {result_rounded}")

# Question 4: Absolute and relative error
absolute_error = abs(result_double - result_rounded)
relative_error = absolute_error / abs(result_double)
print(f"4. {absolute_error}")
print(f"4. {relative_error}")

# Question 5: Minimum terms for f(1) with error < 10^-4
def compute_terms_for_f1(error_tolerance):
    x = 1
    series_sum = 0
    term = 1
    k = 1
    while abs(term) > error_tolerance:
        term = ((-1)**(k-1)) * (x**k) / (k**2)
        series_sum += term
        k += 1
    return k - 1

terms_needed = compute_terms_for_f1(1e-4)
print(f"5. {terms_needed}")

# Question 6a: Bisection Method
def bisection_method(f, a, b, tolerance):
    iterations = 0
    while abs(b - a) / 2 > tolerance:
        midpoint = (a + b) / 2
        if f(midpoint) == 0:
            return midpoint, iterations
        elif f(a) * f(midpoint) < 0:
            b = midpoint
        else:
            a = midpoint
        iterations += 1
    return (a + b) / 2, iterations

f = lambda x: x**3 + 4*x**2 - 10
root_bisection, iterations_bisection = bisection_method(f, -4, 7, 1e-4)
print(f"6a. {root_bisection}")
print(f"6a. {iterations_bisection}")

# Question 6b: Newton-Raphson Method
def newton_raphson_method(f, df, x0, tolerance):
    iterations = 0
    while True:
        x1 = x0 - f(x0) / df(x0)
        iterations += 1
        if abs(x1 - x0) < tolerance:
            break
        x0 = x1
    return x1, iterations


df = lambda x: 3*x**2 + 8*x
root_newton, iterations_newton = newton_raphson_method(f, df, 7, 1e-4)
print(f"6b. {root_newton}")
print(f"6b. {iterations_newton}")
