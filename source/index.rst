import random
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

# Размеры поля и количество мин
FIELD_SIZE = 5
MINES_COUNT = 3

class Minesweeper:
    def init(self):
        self.field = [[' ' for _ in range(FIELD_SIZE)] for _ in range(FIELD_SIZE)]
        self.mines = set()
        self.generate_mines()

    def generate_mines(self):
        while len(self.mines) < MINES_COUNT:
            x = random.randint(0, FIELD_SIZE - 1)
            y = random.randint(0, FIELD_SIZE - 1)
            self.mines.add((x, y))
            self.field[x][y] = '*'

    def display_field(self):
        return '\n'.join([' '.join(row) for row in self.field])

def start(update: Update, context: CallbackContext) -> None:
    update.message.reply_text('Привет! Введите /play для начала игры.')

def play(update: Update, context: CallbackContext) -> None:
    game = Minesweeper()
    context.user_data['game'] = game
    update.message.reply_text('Игра началась! Вот ваше поле:\n' + game.display_field())

def main() -> None:
    updater = Updater("7799933106:AAEpBEQgzV2ItWvqwPTlYMSbnbCEKsqMH-k")

    dispatcher = updater.dispatcher
    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CommandHandler("play", play))

    updater.start_polling()
    updater.idle()

if name == 'main':
    main()
