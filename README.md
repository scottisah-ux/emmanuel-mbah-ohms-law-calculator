# emmanuel-mbah-ohms-law-calculator
# Ohm's Law and Power Calculator
# Author: Emmanuel Mbah Mechatronics, YabaTech
# This program calculates Voltage, Current, Resistance and Power

# Function to get safe number input - checks for wrong typing
def get_number(prompt):
    while True:
        try:
            value = float(input(prompt))
            return value
        except ValueError:
            print("Invalid input! Please enter a number only.")

# Function to calculate - meets minimum requirement
def calculate_power(v, i, r):
    """Returns dictionary of all values using Ohm's law"""
    # This function uses formulas: V=IR, P=VI, P=I^2R, P=V^2/R
    if v == 0 and i != 0 and r != 0:
        v = i * r
    if i == 0 and v != 0 and r != 0:
        if r == 0:
            print("Error: Cannot divide by zero! Resistance cannot be 0")
            return None
        i = v / r
    if r == 0 and v != 0 and i != 0:
        if i == 0:
            print("Error: Cannot divide by zero! Current cannot be 0")
            return None
        r = v / i
    
    # Calculate power
    if v != 0 and i != 0:
        p = v * i
    elif i != 0 and r != 0:
        p = (i ** 2) * r
    elif v != 0 and r != 0:
        if r == 0:
            print("Error: Resistance is zero")
            return None
        p = (v ** 2) / r
    else:
        p = 0
        
    return {"V": v, "I": i, "R": r, "P": p}

# Main program - loop repeats until user quits
print("=== OHM'S LAW & POWER CALCULATOR ===")
print("Emmanuel Mbah Mechatronics Project\n")

history = []  # MY OWN TWIST 1: Store history of calculations

while True:
    print("\nWhat do you want to find?")
    print("1. Voltage (V) - Unit: Volts")
    print("2. Current (I) - Unit: Amperes")
    print("3. Resistance (R) - Unit: Ohms")
    print("4. Power (P) - Unit: Watts")
    print("5. Show History")
    print("6. Quit")

    choice = input("Enter choice (1-6): ")

    # MY OWN TWIST 2: Safety Warning System for high power/current
    if choice == "1":
        i = get_number("Enter Current (A): ")
        r = get_number("Enter Resistance (Ohms): ")
        result = calculate_power(0, i, r)
        if result:
            print(f"\nResult: Voltage = {result['V']:.2f} V")
            history.append(f"V = {result['V']:.2f} V from I={i}A, R={r}Ohms")
    
    elif choice == "2":
        v = get_number("Enter Voltage (V): ")
        r = get_number("Enter Resistance (Ohms): ")
        if r == 0:
            print("Error: Resistance cannot be 0, it will cause infinite current!")
        else:
            result = calculate_power(v, 0, r)
            if result:
                print(f"\nResult: Current = {result['I']:.2f} A")
                if result['I'] > 15:
                    print("WARNING! High current >15A - Not safe for home wiring!")
                history.append(f"I = {result['I']:.2f} A from V={v}V, R={r}Ohms")

    elif choice == "3":
        v = get_number("Enter Voltage (V): ")
        i = get_number("Enter Current (A): ")
        if i == 0:
            print("Error: Current cannot be 0 when finding resistance!")
        else:
            result = calculate_power(v, i, 0)
            if result:
                print(f"\nResult: Resistance = {result['R']:.2f} Ohms")
                history.append(f"R = {result['R']:.2f} Ohms from V={v}V, I={i}A")

    elif choice == "4":
        print("Find Power using: 1) V & I  2) I & R  3) V & R")
        sub = input("Choose method (1/2/3): ")
        if sub == "1":
            v = get_number("Enter Voltage (V): ")
            i = get_number("Enter Current (A): ")
            p = v * i
            print(f"\nResult: Power = {p:.2f} W")
            if p > 1000:
                print("WARNING! High power >1000W - Needs proper cooling!")
            history.append(f"P = {p:.2f} W from V={v}V, I={i}A")
        elif sub == "2":
            i = get_number("Enter Current (A): ")
            r = get_number("Enter Resistance (Ohms): ")
            p = (i**2) * r
            print(f"\nResult: Power = {p:.2f} W")
            history.append(f"P = {p:.2f} W from I={i}A, R={r}Ohms")
        elif sub == "3":
            v = get_number("Enter Voltage (V): ")
            r = get_number("Enter Resistance (Ohms): ")
            if r == 0:
                print("Error: Resistance cannot be 0")
            else:
                p = (v**2)/r
                print(f"\nResult: Power = {p:.2f} W")
                history.append(f"P = {p:.2f} W from V={v}V, R={r}Ohms")
        else:
            print("Invalid method!")

    elif choice == "5":
        print("\n--- Calculation History ---")
        if len(history) == 0:
            print("No calculations yet.")
        else:
            for h in history:
                print(h)

    elif choice == "6":
        print("Thank you for using the calculator. Goodbye!")
        break

    else:
        print("Invalid choice! Please enter 1 to 6 only.")