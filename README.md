import os
import hashlib  # Built-in library for password hashing

# Backpack rental options with daily prices
backpack_options = [
    {"size": "Small", "color": "Red", "design": "Classic", "price_per_day": 5},
    {"size": "Small", "color": "Blue", "design": "Sport", "price_per_day": 6},
    {"size": "Medium", "color": "Green", "design": "Urban", "price_per_day": 7},
    {"size": "Medium", "color": "Black", "design": "Travel", "price_per_day": 8},
    {"size": "Large", "color": "Yellow", "design": "Hiking", "price_per_day": 10},
    {"size": "Large", "color": "Gray", "design": "Adventure", "price_per_day": 9},
    {"size": "Extra Large", "color": "Orange", "design": "Expedition", "price_per_day": 12},
    {"size": "Extra Large", "color": "Purple", "design": "Camping", "price_per_day": 11},
    {"size": "Small", "color": "Pink", "design": "Trendy", "price_per_day": 5},
    {"size": "Medium", "color": "Camo", "design": "Tactical", "price_per_day": 8},
]

# Outdoor clothing apparel options with prices
clothing_apparel_options = [
    {"type": "Hoodie", "color": "Navy", "price": 25},
    {"type": "Hoodie", "color": "Forest Green", "price": 25},
    {"type": "Hoodie", "color": "Charcoal", "price": 25},
    {"type": "T-shirt", "color": "Light Blue", "price": 15},
    {"type": "T-shirt", "color": "Olive", "price": 15},
    {"type": "T-shirt", "color": "Burgundy", "price": 15},
    {"type": "Long Sleeve Shirt", "color": "Tan", "price": 20},
    {"type": "Long Sleeve Shirt", "color": "Teal", "price": 20},
    {"type": "Pants", "color": "Black", "price": 30},
    {"type": "Pants", "color": "Khaki", "price": 30},
    {"type": "Shorts", "color": "Camo", "price": 20},
    {"type": "Shorts", "color": "Navy Blue", "price": 20},
    {"type": "Jacket", "color": "Red", "price": 50},
    {"type": "Jacket", "color": "Gray", "price": 50},
    {"type": "Base Layer", "color": "White", "price": 20},
    {"type": "Base Layer", "color": "Black", "price": 20},
]

# Payment options
payment_options = [
    {"method": "Credit Card"},
    {"method": "Debit Card"},
    {"method": "PayPal"},
    {"method": "Apple Pay"},
]

# National Parks options
national_parks = [
    {"name": "Yellowstone National Park", "location": "Wyoming, Montana, Idaho"},
    {"name": "Yosemite National Park", "location": "California"},
    {"name": "Grand Canyon National Park", "location": "Arizona"},
    {"name": "Zion National Park", "location": "Utah"},
    {"name": "Great Smoky Mountains National Park", "location": "Tennessee, North Carolina"},
    {"name": "Rocky Mountain National Park", "location": "Colorado"},
    {"name": "Glacier National Park", "location": "Montana"},
    {"name": "Joshua Tree National Park", "location": "California"},
]

# In-memory storage for users and rental history
users = {}
rental_history = {}
purchase_history = {}

# Hash password
def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

# Verify password
def verify_password(stored_password, provided_password):
    return stored_password == hashlib.sha256(provided_password.encode()).hexdigest()

# Function to display backpack options with indices
def display_backpacks():
    print("\nBackpack Rental Options:")
    for index, backpack in enumerate(backpack_options, start=1):
        print(f"{index}. Size: {backpack['size']}, Color: {backpack['color']}, Design: {backpack['design']}, Price per Day: ${backpack['price_per_day']}")

# Function to display clothing options with indices
def display_clothing():
    print("\nOutdoor Clothing Apparel:")
    for index, clothing in enumerate(clothing_apparel_options, start=1):
        print(f"{index}. Type: {clothing['type']}, Color: {clothing['color']}, Price: ${clothing['price']}")

# Function to display payment options
def display_payment_options():
    print("\nPayment Options:")
    for payment in payment_options:
        print(f"Method: {payment['method']}")

# Function to display national parks
def display_national_parks():
    print("\nExplore National Parks:")
    for park in national_parks:
        print(f"{park['name']} - Location: {park['location']}")

# Home Section
def home():
    print("\nWelcome to the Outdoor Rental Service!")
    display_backpacks()
    display_clothing()
    display_payment_options()

# About Section
def about():
    print("\nAbout Us:")
    print("We provide high-quality outdoor gear for all your adventure needs. Rent a backpack or buy some clothing for your next trip!")

# User registration
def register_user():
    username = input("Choose a username: ")
    if username in users:
        print("Username already exists. Try a different one.")
        return
    password = input("Choose a password: ")
    hashed_password = hash_password(password)
    users[username] = hashed_password
    print("Registration successful!")

# Account login
def account_login():
    username = input("\nEnter your username: ")
    password = input("Enter your password: ")
    if username in users and verify_password(users[username], password):
        print("Login successful!")
        return username
    else:
        print("Login failed! Please check your credentials.")
        return None

# Rent an item
def rent_item(user):
    display_backpacks()
    backpack_choice_index = int(input("\nChoose a backpack to rent (number): ")) - 1
    
    if 0 <= backpack_choice_index < len(backpack_options):
        selected_backpack = backpack_options[backpack_choice_index]
        duration = int(input("Enter the rental duration in days: "))
        total_cost = selected_backpack['price_per_day'] * duration
        print(f"\nTotal cost for renting {selected_backpack['size']} {selected_backpack['color']} for {duration} day(s): ${total_cost}")
        
        shipping_address = input("Enter your shipping address: ")
        
        confirm = input("Do you want to proceed with the rental? (yes/no): ").strip().lower()
        if confirm == "yes":
            rental_history.setdefault(user, []).append({"item": f"{selected_backpack['size']} {selected_backpack['color']}", "duration": duration, "cost": total_cost, "shipping_address": shipping_address})
            print(f"\n{selected_backpack['size']} {selected_backpack['color']} rented successfully!")
            
            # Offer the option to view national parks
            view_parks = input("Would you like to view national parks for your adventure? (yes/no): ").strip().lower()
            if view_parks == "yes":
                display_national_parks()
            else:
                print("Enjoy your trip with the rented gear!")
        else:
            print("\nRental cancelled.")
    else:
        print("Invalid choice. Please select a valid backpack.")

# Return an item
def return_item(user):
    if user in rental_history and rental_history[user]:
        print("\nRented Items:")
        for index, rental in enumerate(rental_history[user], start=1):
            print(f"{index}. {rental['item']} - Duration: {rental['duration']} day(s), Total Cost: ${rental['cost']}, Shipping Address: {rental['shipping_address']}")
        
        item_index = int(input("\nEnter the number of the item you want to return: ")) - 1
        
        if 0 <= item_index < len(rental_history[user]):
            returned_item = rental_history[user].pop(item_index)
            print(f"\n{returned_item['item']} returned successfully!")
        else:
            print("Invalid choice. No item returned.")
    else:
        print("\nYou have no rented items.")

# Purchase an item
def purchase_item(user):
    display_clothing()
    clothing_choice_index = int(input("\nChoose clothing to purchase (number): ")) - 1
    
    if 0 <= clothing_choice_index < len(clothing_apparel_options):
        selected_clothing = clothing_apparel_options[clothing_choice_index]
        print(f"\nTotal cost for purchasing {selected_clothing['type']} {selected_clothing['color']}: ${selected_clothing['price']}")
        
        shipping_address = input("Enter your shipping address: ")
        
        confirm = input("Do you want to proceed with the purchase? (yes/no): ").strip().lower()
        if confirm == "yes":
            purchase_history.setdefault(user, []).append({"item": f"{selected_clothing['type']} {selected_clothing['color']}", "cost": selected_clothing['price'], "shipping_address": shipping_address})
            print(f"\n{selected_clothing['type']} {selected_clothing['color']} purchased successfully!")
        else:
            print("\nPurchase cancelled.")
    else:
        print("Invalid choice. Please select a valid clothing item.")

# Main function to run the program
def main():
    logged_in_user = None
    
    while True:
        print("\n===== Main Menu =====")
        print("1. Home")
        print("2. About")
        print("3. Register")
        print("4. Login")
        print("5. Rent an Item")
        print("6. Return an Item")
        print("7. Purchase an Item")
        print("8. Exit")
        
        choice = input("\nEnter your choice: ").strip()
        
        if choice == '1':
            home()
        elif choice == '2':
            about()
        elif choice == '3':
            register_user()
        elif choice == '4':
            logged_in_user = account_login()
        elif choice == '5':
            if logged_in_user:
                rent_item(logged_in_user)
            else:
                print("\nPlease login first to rent an item.")
        elif choice == '6':
            if logged_in_user:
                return_item(logged_in_user)
            else:
                print("\nPlease login first to return an item.")
        elif choice == '7':
            if logged_in_user:
                purchase_item(logged_in_user)
            else:
                print("\nPlease login first to purchase an item.")
        elif choice == '8':
            print("\nExiting the program. Goodbye!")
            break
        else:
            print("\nInvalid choice. Please enter a valid option.")

# Run the program
if __name__ == "__main__":
    main()
