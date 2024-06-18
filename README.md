# Number-Guessing-Game
import random

leaderboard = []

def print_leaderboard():
    if not leaderboard:
        print("No scores yet. Be the first to play!")
        return
    print("Leaderboard:")
    for idx, score in enumerate(sorted(leaderboard, key=lambda x: x['attempts'])):
        print(f"{idx + 1}. {score['name']} - {score['attempts']} attempts")

def get_difficulty_range():
    print("Select Difficulty Level:")
    print("1. Easy (1-10)")
    print("2. Medium (1-50)")
    print("3. Hard (1-100)")
    difficulty = int(input("Enter your choice: "))
    if difficulty == 1:
        return 1, 10
    elif difficulty == 2:
        return 1, 50
    elif difficulty == 3:
        return 1, 100
    else:
        print("Invalid choice. Defaulting to Easy level.")
        return 1, 10

def give_hint(number_to_guess, guess, guess_count):
    hints = []
    
    # Range hint
    if guess < number_to_guess:
        hints.append("The number is greater than your guess.")
    else:
        hints.append("The number is less than your guess.")
    
    # Proximity hint
    difference = abs(number_to_guess - guess)
    if difference == 0:
        return "Correct!"
    elif difference <= 3:
        hints.append("Very close!")
    elif difference <= 10:
        hints.append("Close!")
    else:
        hints.append("Far off!")
    
    # Even/Odd hint
    if guess_count % 2 == 0:  # Provide even/odd hint on every alternate guess
        if number_to_guess % 2 == 0:
            hints.append("The number is even.")
        else:
            hints.append("The number is odd.")
    
    # Divisibility hint
    if guess_count % 3 == 0:  # Provide divisibility hint every third guess
        for i in [3, 5, 7]:  # Choose a few numbers to check divisibility
            if number_to_guess % i == 0:
                hints.append(f"The number is divisible by {i}.")
                break
    
    return " ".join(hints)

def number_guessing_game():
    print("Welcome to the Number Guessing Game!")
    X, Y = get_difficulty_range()
    
    number_to_guess = random.randint(X, Y)
    guess_count = 0
    print(f"Guess the number between {X} and {Y}")

    while True:
        guess_count += 1
        guess = int(input("Enter your guess: "))
        
        if guess < X or guess > Y:
            print(f"Please enter a number within the range {X} and {Y}.")
            continue
        
        hint = give_hint(number_to_guess, guess, guess_count)
        
        if "Correct!" in hint:
            print(hint)
            name = input("Congratulations! You've guessed the number. What's your name? ")
            leaderboard.append({'name': name, 'attempts': guess_count})
            print(f"You've guessed the number in {guess_count} attempts.")
            break
        else:
            print(hint)

    print_leaderboard()
    
    replay = input("Do you want to play again? (yes/no): ").strip().lower()
    if replay == "yes":
        number_guessing_game()
    else:
        print("Thank you for playing!")

if __name__ == "__main__":
    number_guessing_game()

