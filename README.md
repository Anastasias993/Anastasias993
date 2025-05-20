from telegram import Update, KeyboardButton, ReplyKeyboardMarkup
from telegram.ext import (
    Application,
    CommandHandler,
    MessageHandler,
    filters,
    ContextTypes
)

TOKEN = "7828105755:AAEuc-u-XtNStadlLoSYW5hm8fJeBhKcMQY"

# Структура данных для тем и слов
topics = {
    "Образование": [
        ("Student", "Студент (м.), Студентка (ж.)"),
        ("Teacher", "Учитель (м.), Учительница (ж.)"),
        ("School", "Школа"),
        ("University", "Университет"),
        ("Exam", "Экзамен"),
        ("Homework", "Домашнее задание"),
        ("Book", "Книга"),
        ("Lecture", "Лекция"),
        ("Grade", "Оценка"),
        ("Diploma", "Диплом")
    ],
    "Медицина": [
        ("Doctor", "Доктор, врач"),
        ("Hospital", "Больница"),
        ("Medicine", "Лекарство"),
        ("Nurse", "Медсестра"),
        ("Patient", "Пациент"),
        ("Surgery", "Операция"),
        ("Pain", "Боль"),
        ("Prescription", "Рецепт"),
        ("Vaccine", "Вакцина"),
        ("Ambulance", "Скорая помощь")
    ],
    "Технологии": [
        ("Computer", "Компьютер"),
        ("Internet", "Интернет"),
        ("Software", "Программное обеспечение (ПО)"),
        ("Smartphone", "Смартфон"),
        ("Robot", "Робот"),
        ("Artificial Intelligence (AI)", "Искусственный интеллект (ИИ)"),
        ("Data", "Данные"),
        ("Algorithm", "Алгоритм"),
        ("Cybersecurity", "Кибербезопасность"),
        ("Virtual Reality (VR)", "Виртуальная реальность (ВР)")
    ],
    "Политика": [
        ("President", "Президент"),
        ("Government", "Правительство"),
        ("Election", "Выборы"),
        ("Law", "Закон"),
        ("Democracy", "Демократия"),
        ("Minister", "Министр"),
        ("Party (political)", "Партия"),
        ("Corruption", "Коррупция"),
        ("Diplomacy", "Дипломатия"),
        ("Revolution", "Революция")
    ],
    "История": [
        ("War", "Война"),
        ("Empire", "Империя"),
        ("Revolution", "Революция"),
        ("Ancient", "Древний"),
        ("Civilization", "Цивилизация"),
        ("Battle", "Битва"),
        ("King", "Король"),
        ("Queen", "Королева"),
        ("Discovery", "Открытие"),
        ("Century", "Век")
    ]
}

def get_main_keyboard():
    buttons = [
        [KeyboardButton("Выбор темы")],
        [KeyboardButton("Начать изучение")],
        [KeyboardButton("Поиск слова")],
        [KeyboardButton("Помощь")],
        [KeyboardButton("О проекте")]
    ]
    return ReplyKeyboardMarkup(buttons, resize_keyboard=True)

def get_topics_keyboard():
    topics_buttons = [[KeyboardButton(topic)] for topic in topics]
    topics_buttons.append([KeyboardButton("Назад")])
    return ReplyKeyboardMarkup(topics_buttons, resize_keyboard=True)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "Hello! На связи бот, который поможет вам выучить новые слова по определенной теме. "
        "Скорее начинай пользоваться нашим ботом. Надеемся, он тебе поможет)",
        reply_markup=get_main_keyboard()
    )

async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    text = update.message.text
    
    if text == "Выбор темы":
        await update.message.reply_text(
            "Выберите тему:",
            reply_markup=get_topics_keyboard()
        )
    
    elif text in topics:
        words_list = [f"{i+1}. {eng} – {rus}" for i, (eng, rus) in enumerate(topics[text])]
        response = f"### {text}\n" + "\n".join(words_list)
        await update.message.reply_text(response)
    
    elif text == "Начать изучение":
        await update.message.reply_text("Функция изучения в разработке")
    
    elif text == "Поиск слова":
        await update.message.reply_text("Введите слово для поиска:")
    
    elif text == "Помощь":
        await update.message.reply_text("Возник вопрос? Обращайтесь по адресу: @nessska_a")
    
    elif text == "О проекте":
        await update.message.reply_text(
            "Данный бот выполнен в качестве итогового проекта студентами программы'Цифровой переводчик. Постредактор PRO':\n"
            "Савиных Анастасия - тимлид\n"
            "Чухванцева Дарья\n"
            "Шарифуллин Руслан\n"
            "Иголкин Владимир\n"
            "Рахмонкулов Азизбек\n"
            "Яруллин Эльдар"
        )
    
    elif text == "Назад":
        await update.message.reply_text(
            "Главное меню:",
            reply_markup=get_main_keyboard()
        )
    
    else:
        await update.message.reply_text("Неизвестная команда")

def main():
    app = Application.builder().token(TOKEN).build()
    
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT, handle_message))
    
    print("Бот успешно запущен!")
    app.run_polling()

if __name__ == "__main__":
    main()
