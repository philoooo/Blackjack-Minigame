<h1 align="center"> Blackjack Minigame </h1>

Blackjack, or twenty-one, is a card game where the objective is to get a hand total closer to 21 than the dealer without going over. The user receives two cards, and has the option to "hit" (take another card) or "stand" (not take another card). Card values are their face value, face cards are worth 10, and an ace is either 1 or 11, whichever is more beneficial. 


1. The game uses the following rules: The deck is unlimited. There are no jokers. The Jack/Queen/King all count as 10. The Ace can count as 11 or 1. The deal card function is called to randomly generate a card in the user and the computer's hand.
```
import random
from art import logo

def deal_card():
    """Returns a random card from the deck"""
    cards = [11, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10]
    card = random.choice(cards)
    return card

```

2. This function checks for a Blackjack (2 cards with an ace + 10), representing it as 0. It also checks for an 11 (ace) if the score is already over 21. It removes the 11 and replace it with a 1.

```
def calculate_score(cards):
    """Take a list of cards and return the score calculated from cards"""
    if sum(cards) == 21 and len(cards) ==2:
        return 0
    if sum(cards) > 21 and 11 in cards:
        cards.remove(11)
        cards.append(1)
    return sum(cards)
```
3. This function references the user and the computer's hands to determine the outcome of the game. Using inputs, the user can restart the game again and again.
   
```
def compare(u_score, c_score):
    if u_score == c_score:
        return "It's a draw."
    elif c_score == 0:
        return "You lose. The opponent has a Blackjack"
    elif u_score == 0:
        return "Blackjack! You win!"
    elif u_score > 21:
        return "You lose. You went over."
    elif c_score > 21:
        return "Opponent went over. You win!"
    elif u_score > c_score:
        return "You win!"
    else:
        return "You lose."

def play_game():
    print(logo)
    user_cards = []
    computer_cards = []
    computer_score = -1
    user_score = -1

    is_game_over = False

    for _ in range(2):
        user_cards.append(deal_card())
        computer_cards.append(deal_card())

    while not is_game_over:
        user_score = calculate_score(user_cards)
        computer_score = calculate_score(computer_cards)
        print(f"Your cards: {user_cards}, current score: {user_score}")
        print(f"Computer's first card: {computer_cards[0]}")

        if user_score == 0 or computer_score == 0 or user_score > 21:
            is_game_over = True
        else:
            user_should_deal = input("Type 'y' to hit, or type 'n' to stand.")
            if user_should_deal == "y":
                user_cards.append(deal_card())
            else:
                is_game_over = True

    while computer_score != 0 and computer_score < 17:
        computer_cards.append(deal_card())
        computer_score = calculate_score(computer_cards)

    print(f"Your final hand: {user_cards}, final score: {user_score}")
    print(f"Computer's final hand: {computer_cards}, final score: {computer_score}")
    print(compare(user_score, computer_score))

while input("Do you want to play a game of Blackjack? Type 'y' or 'n'.") == 'y':
    print("\n" * 20)
    play_game()
```

Here is a test case showing the user experience from my editor:
```
.------.            _     _            _    _            _    
|A_  _ |.          | |   | |          | |  (_)          | |   
|( \/ ).-----.     | |__ | | __ _  ___| | ___  __ _  ___| | __
| \  /|K /\  |     | '_ \| |/ _` |/ __| |/ / |/ _` |/ __| |/ /
|  \/ | /  \ |     | |_) | | (_| | (__|   <| | (_| | (__|   < 
`-----| \  / |     |_.__/|_|\__,_|\___|_|\_\ |\__,_|\___|_|\_\\
      |  \/ K|                            _/ |                
      `------'                           |__/           

Your cards: [9, 6], current score: 15
Computer's first card: 11
Type 'y' to hit, or type 'n' to stand.y
Your cards: [9, 6, 7], current score: 22
Computer's first card: 11
Your final hand: [9, 6, 7], final score: 22
Computer's final hand: [2, 10, 1, 6], final score: 19
You lose. You went over.
Do you want to play a game of Blackjack? Type 'y' or 'n'.n
```
