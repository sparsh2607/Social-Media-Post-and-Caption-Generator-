# Social-Media-Post-and-Caption-Generator-
import random
import datetime

# --- 1. User Interface (UI) - Command Line Interface (CLI) Example ---

def display_menu():
    print("\n--- Social Media Caption Generator ---")
    print("1. Generate Caption")
    print("2. View Saved Captions")
    print("3. Manage Keywords/Preferences")
    print("4. Exit")
    print("--------------------------------------")

def get_user_choice():
    while True:
        try:
            choice = int(input("Enter your choice: "))
            if 1 <= choice <= 4:
                return choice
            else:
                print("Invalid choice. Please enter a number between 1 and 4.")
        except ValueError:
            print("Invalid input. Please enter a number.")

# --- 2. Caption Generation Logic ---

class CaptionGenerator:
    def __init__(self):
        self.keywords = []  # User-defined keywords
        self.styles = ["humorous", "inspirational", "witty", "informative", "casual"]
        self.emojis = ["😊", "🌟", "💡", "😂", "💖", "✨"]
        self.hashtags = ["#socialmedia", "#captions", "#contentcreator", "#inspiration", "#dailymotivation"]

    def add_keyword(self, keyword):
        self.keywords.append(keyword.lower())
        print(f"Keyword '{keyword}' added.")

    def remove_keyword(self, keyword):
        if keyword.lower() in self.keywords:
            self.keywords.remove(keyword.lower())
            print(f"Keyword '{keyword}' removed.")
        else:
            print(f"Keyword '{keyword}' not found.")

    def generate_caption(self, image_description="", desired_style=None):
        caption_parts = []

        # Part 1: Based on image description (very basic NLP - could be enhanced)
        if image_description:
            caption_parts.append(f"Here's what I see: {image_description}.")

        # Part 2: Incorporate keywords
        if self.keywords:
            random.shuffle(self.keywords)
            # Take a few random keywords
            selected_keywords = self.keywords[:min(len(self.keywords), 3)]
            caption_parts.append(f"A perfect moment. Featuring {' '.join(selected_keywords)}.")

        # Part 3: Style-based phrases (very simple implementation)
        if desired_style and desired_style in self.styles:
            if desired_style == "humorous":
                caption_parts.append(random.choice(["Laughing all the way!", "Can't handle this much fun.", "Seriously funny."]))
            elif desired_style == "inspirational":
                caption_parts.append(random.choice(["Believe in yourself!", "Inspire and be inspired.", "Dream big, work hard."]))
            elif desired_style == "witty":
                caption_parts.append(random.choice(["Cleverly done.", "Thinking outside the box.", "Sharp and on point."]))
            elif desired_style == "informative":
                caption_parts.append(random.choice(["Did you know...?", "Learn something new today.", "Facts you need to know."]))
            elif desired_style == "casual":
                caption_parts.append(random.choice(["Just chillin'.", "Good vibes only.", "Simple pleasures."]))
        else:
            # If no specific style, pick a random general one
            caption_parts.append(random.choice([
                "Feeling good!", "Making memories.", "Enjoying the little things.",
                "What an amazing day!", "Creating magic."
            ]))

        # Part 4: Add a random emoji
        caption_parts.append(random.choice(self.emojis))

        # Part 5: Add a few random hashtags
        random.shuffle(self.hashtags)
        selected_hashtags = self.hashtags[:min(len(self.hashtags), 3)]
        caption_parts.append(" ".join(selected_hashtags))

        # Combine parts into a full caption
        full_caption = " ".join(caption_parts)
        return full_caption

# --- 3. Data Storage (Simple Text File Example) ---

def save_caption(caption):
    with open("saved_captions.txt", "a") as f:
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        f.write(f"[{timestamp}] {caption}\n")
    print("Caption saved!")

def load_saved_captions():
    try:
        with open("saved_captions.txt", "r") as f:
            captions = f.readlines()
        if captions:
            print("\n--- Saved Captions ---")
            for caption in captions:
                print(caption.strip())
            print("----------------------")
        else:
            print("No captions saved yet.")
    except FileNotFoundError:
        print("No captions saved yet.")

# --- Main Application Flow ---

def main():
    generator = CaptionGenerator()

    # Initial setup for keywords (can be loaded from a file in a real app)
    generator.add_keyword("travel")
    generator.add_keyword("nature")
    generator.add_keyword("adventure")
    generator.add_keyword("food")

    while True:
        display_menu()
        choice = get_user_choice()

        if choice == 1:
            description = input("Enter a brief description of your image (optional): ")
            style = input(f"Enter desired style ({', '.join(generator.styles)}) or leave blank for random: ").lower()
            if style not in generator.styles:
                style = None # Reset to None if invalid style

            generated_caption = generator.generate_caption(description, style)
            print(f"\nGenerated Caption: {generated_caption}")
            save_choice = input("Save this caption? (y/n): ").lower()
            if save_choice == 'y':
                save_caption(generated_caption)

        elif choice == 2:
            load_saved_captions()

        elif choice == 3:
            while True:
                print("\n--- Manage Keywords/Preferences ---")
                print("1. Add Keyword")
                print("2. Remove Keyword")
                print("3. View Current Keywords")
                print("4. Back to Main Menu")
                keyword_choice = get_user_choice() # Reuse general choice function

                if keyword_choice == 1:
                    new_keyword = input("Enter keyword to add: ")
                    generator.add_keyword(new_keyword)
                elif keyword_choice == 2:
                    rem_keyword = input("Enter keyword to remove: ")
                    generator.remove_keyword(rem_keyword)
                elif keyword_choice == 3:
                    print("\nCurrent Keywords:", ", ".join(generator.keywords) if generator.keywords else "None")
                elif keyword_choice == 4:
                    break
                else:
                    print("Invalid choice.")

        elif choice == 4:
            print("Exiting Caption Generator. Goodbye!")
            break

        else:
            print("An unexpected error occurred.")

if __name__ == "__main__":
    main()
