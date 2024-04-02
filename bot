pip install pymorphy2
pip install pytelegrambotapi


import telebot
import pymorphy2
from nltk.stem import SnowballStemmer

bot = telebot.TeleBot('6749518645:AAEZKuSWC9-YfsVJjuUxMmKJOS9_2z7t5l0')
is_bot_active = False


dictionary = {
    'а': 'a',
    'б': '(b|6)',
    'в': 'v',
    'г': 'g',
    'д': 'd',
    'е': 'e',
    'ё': 'e',
    'ж': 'zh',
    'з': 'z',
    'и': '(i|1)',
    'й': 'i',
    'к': 'k',
    'л': '(l|1)',
    'м': 'm',
    'н': 'n',
    'о': '(o|0)',
    'п': 'p',
    'р': 'r',
    'с': 's',
    'т': 't',
    'у': '(u|у)',
    'ф': 'f',
    'х': 'h',
    'ц': 'c',
    'ч': '(cz|4)',
    'ш': 'sh',
    'щ': 'scz',
    'ъ': '',
    'ы': 'y',
    'ь': "'",
    'э': 'e',
    'ю': 'u',
    'я': 'ja',
    'А': 'A',
    'Б': 'B',
    'В': 'V',
    'Г': 'G',
    'Д': 'D',
    'Е': 'E',
    'Ё': 'E',
    'Ж': 'ZH',
    'З': 'Z',
    'И': 'I',
    'Й': 'I',
    'К': 'K',
    'Л': 'L',
    'М': 'M',
    'Н': 'N',
    'О': 'O',
    'П': 'P',
    'Р': 'R',
    'С': 'S',
    'Т': 'T',
    'У': 'U',
    'Ф': 'F',
    'Х': 'H',
    'Ц': 'C',
    'Ч': 'CZ',
    'Ш': 'SH',
    'Щ': 'SCH',
    'Ъ': '',
    'Ы': 'y',
    'Ь': 'b',
    'Э': 'E',
    'Ю': 'U',
    'Я': 'YA',
    ',': ',',
    '?': '?',
    ' ': '_',
    '~': '~',
    '!': '!',
    '@': '@',
    '#': '#',
    '$': '$',
    '%': '%',
    '^': '^',
    '&': '&',
    '*': '*',
    '(': '(',
    ')': ')',
    '-': '-',
    '=': '=',
    '+': '+',
    ':': ':',
    ';': ';',
    '<': '<',
    '>': '>',
    '\'': '\'',
    '"': '"',
    '\\': '\\',
    '/': '/',
    '№': '#',
    '[': '[',
    ']': ']',
    '{': '{',
    '}': '}',
    'ґ': 'r',
    'ї': 'r',
    'є': 'e',
    'Ґ': 'g',
    'Ї': 'i',
    'Є': 'e',
    '—': '-'
}


def process_word(word):
    morph = pymorphy2.MorphAnalyzer()
    stemmer = SnowballStemmer("russian")
    parsed_words = morph.parse(word)
    unique_forms = set()

    # Транслитерация основы слова
    stem = stemmer.stem(word)
    transliterated_stem = ''.join([dictionary.get(char, char) for char in stem])

    for parsed_word in parsed_words:
        normal_form = parsed_word.normal_form
        forms = parsed_word.lexeme
        if all(form.word == word for form in forms):
            forms = [parsed_word]
        suffixes_and_endings = [(form.word[:len(stem)], form.word[len(stem):]) for form in forms]
        unique_suffixes = set([e for s, e in suffixes_and_endings if e != ""])
        unique_forms.update(unique_suffixes)

    endings_str = "|".join(unique_forms)
    formatted_output = f"{stem}({endings_str})"

    # Транслитерация окончаний
    transliterated_endings = ''.join([dictionary.get(char, char) for char in endings_str])

    return f"{transliterated_stem}({transliterated_endings})\n\n{formatted_output}"


@bot.message_handler(commands=['start'])
def start_message(message):
    global is_bot_active
    is_bot_active = True
    bot.reply_to(message, "Бот работает. Впиши слово для окончаний")

@bot.message_handler(func=lambda message: is_bot_active)
def process_message(message):
    words = message.text.split()
    separator = "\n"
    result = f"{separator}".join([process_word(word) for word in words])
    bot.reply_to(message, result)


bot.remove_webhook()


bot.polling()
