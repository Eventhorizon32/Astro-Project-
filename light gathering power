def calculate_light_gathering_power():
    while True:
        try:
            diameter1 = float(input("Enter the diameter of the first telescope (in mm): "))
            diameter2 = float(input("Enter the diameter of the second telescope (in mm): "))
            if diameter1 <= 0 or diameter2 <= 0:
                print("Not in range, try again")
                continue

            power_ratio = (diameter1 / diameter2) ** 2
            print(f"Light gathering power ratio is {power_ratio:.2f}")
            break  # Exit the loop after successful calculation
        except ValueError:
            print("Invalid input, please enter numeric values.")
