# hangmangame
import random

WORD_LIST = ["python", "hangman", "program", "console", "coding"]
MAX_INCORRECT_GUESSES = 6

def get_word():
    """Chooses a random word from the list."""
    return random.choice(WORD_LIST)

def display_game_state(hidden_word, incorrect_guesses):
    """Prints the current state of the game."""
    print("\n--- Hangman State ---")
    print("Word:", ' '.join(hidden_word))
    print(f"Incorrect Guesses Left: {MAX_INCORRECT_GUESSES - len(incorrect_guesses)}")
    print("Letters Guessed:", ' '.join(sorted(incorrect_guesses)))
    print("---------------------\n")

# The core game logic remains the same
def play_round():
    """Runs a single round of the Hangman game."""
    word = get_word()
    word_letters = set(word)
    
    # Initialize game variables
    guessed_letters = set()
    incorrect_guesses = set()
    hidden_word_list = ['_' for _ in word]
    
    print(f"\nA new word has been selected! It has {len(word)} letters.")
    
    while len(incorrect_guesses) < MAX_INCORRECT_GUESSES and set(hidden_word_list) != word_letters:
        
        display_game_state(hidden_word_list, incorrect_guesses)
        
        guess = input("Guess a letter: ").lower()
        
        # Input validation (omitted some for brevity, but should be included)
        if len(guess) != 1 or not 'a' <= guess <= 'z':
            print("❌ Invalid input. Please enter a single letter.")
            continue
            
        if guess in guessed_letters:
            print(f"You already guessed '{guess}'. Try a different letter.")
            continue

        guessed_letters.add(guess)
        
        if guess in word_letters:
            print(f"✅ Good guess!")
            for i, letter in enumerate(word):
                if letter == guess:
                    hidden_word_list[i] = guess
                    
        else:
            print(f"😔 Sorry, '{guess}' is NOT in the word.")
            incorrect_guesses.add(guess)

    # --- Round End ---
    print("\n==============================")
    if set(hidden_word_list) == word_letters:
        print(f"🎉 CONGRATULATIONS! You guessed the word: **{word.upper()}**")
        return True # Player won
    else:
        print(f"💀 GAME OVER! You ran out of guesses.")
        print(f"The word was: **{word.upper()}**")
        return False # Player lost
    print("==============================")


# New main function to manage continuous play
def continuous_hangman_game():
    """Manages continuous play, score, and asking to play again."""
    print("Welcome to Continuous Text-Based Hangman! 📝")
    wins = 0
    losses = 0
    
    # This loop keeps the game running until the player decides to quit
    while True:
        print("\n--- Starting New Round ---")
        
        # Play the round and update score
        if play_round():
            wins += 1
        else:
            losses += 1
        
        # Display current score
        print(f"\n🏆 Score: Wins: {wins} | Losses: {losses}")
        
        # Ask the player to play again
        play_again = input("Do you want to play another round? (y/n): ").lower()
        
        if play_again != 'y':
            print("\nThanks for playing! Final Score: Wins: {wins} | Losses: {losses}")
            print("Goodbye! 👋")
            break # Exit the while loop to end the continuous game

if __name__ == "__main__":
    # Call the new function to start the continuous game
    continuous_hangman_game()
