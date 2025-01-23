# cot-4500-Pro1
Assignment 1

# Convert binary to decimal with double precision
binary_string = "0100000001111111101010111001"

# Function to convert binary string to double precision decimal
def binary_to_decimal(binary):
    integer_part = int(binary[:1], 2)  # sign bit
    exponent = int(binary[1:12], 2) - 1023  # exponent part, bias is 1023
    mantissa = 1 + sum(int(bit) * 2**-(i + 1) for i, bit in enumerate(binary[12:]))
    value = ((-1)**integer_part) * mantissa * (2**exponent)
    return round(value, 5)

result = binary_to_decimal(binary_string)
print("Result in double precision:", result)
