while True:
    try:
        focal_length = int(input("Enter focal length: "))
        if focal_length <= 0:
            print("Not in range, try again")
            continue

        lens_diameter = int(input("Enter lens diameter: "))
        if lens_diameter <= 0:
            print("Not in range, try again")
            continue

        focal_ratio = focal_length / lens_diameter
        print("Focal ratio is", focal_ratio)
        break  # Exit the loop after successful calculation
    except ValueError:
        print("Invalid input, please enter numeric values.")
