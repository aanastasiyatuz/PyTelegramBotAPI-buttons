# Кнопки в PyTelegramBotApi

Итак у нас есть такой бот:

```py
import telebot

token = 'ваш токен'

bot = telebot.TeleBot(token)

@bot.message_handler(commands=['start', 'hi'])
def start_message(message):
    bot.send_message(message.chat.id, "Hello")

@bot.message_handler(content_types=['sticker'])
def send_sticker(message):
    bot.send_sticker(message.chat.id, message.sticker.file_id)

bot.polling()
```

Давайте добавим ему кнопки после того, как мы здороваемся, 
чтобы он мог либо прислать нам стикер либо попрощаться.
Чтобы прикрепить к боту кнопки, вам нужно создать клавиатуру и добавить в нее кнопки, 
после прикрепить к какому-нибудь сообщению.
Новый код будет писаться в таких разделителях """""", 
вам эти разделители писать не обязательно, 
это для того, чтобы вы понимали, какой код мы добавили.

```py
import telebot
""""""
from telebot import types
""""""

token = 'ваш токен'

bot = telebot.TeleBot(token)

""""""
# создаем клавиатуру
keyboard = types.InlineKeyboardMarkup()
# создаем кнопки, в скобочках первое - сообщение внутри кнопки, 
# второе - текст по которому мы поймем, что именно эта кнопка была нажата (в телеграме этого не будет видно)
button1 = types.InlineKeyboardButton("Прислать стикер", callback_data="yes")
button2 = types.InlineKeyboardButton("Попрощаться", callback_data="no")
# добавляем кнопки в клавиатуру
keyboard.add(button1, button2)
""""""

@bot.message_handler(commands=['start', 'hi'])
def start_message(message):
    bot.send_message(message.chat.id, "Hello")
    """"""
    # настройка reply_markup позволяет нам прикрепить клавиатуру к сообщению
    bot.send_message(message.chat.id, "Что сделать дальше?", reply_markup=keyboard)
    """"""

@bot.message_handler(content_types=['sticker'])
def send_sticker(message):
    bot.send_sticker(message.chat.id, message.sticker.file_id)

bot.polling()
```

Теперь наш бот после того как мы напишем `\start` или `\hi` будет нам присылать сообщение `Hello`,
и сообщение `Что сделать дальше?` с кнопками `Прислать стикер` `Попрощаться`.
Но при нажатии на кнопки, наш бот еще не знает, что делать. 
Нам нужно написать ему код, реагирующий на эти кнопки.

```py
import telebot
from telebot import types

token = 'ваш токен'

bot = telebot.TeleBot(token)

# создаем клавиатуру
keyboard = types.InlineKeyboardMarkup()
# создаем кнопки, в скобочках первое - сообщение внутри кнопки, 
# второе - текст по которому мы поймем, что именно эта кнопка была нажата (в телеграме этого не будет видно)
button1 = types.InlineKeyboardButton("Прислать стикер", callback_data="yes")
button2 = types.InlineKeyboardButton("Попрощаться", callback_data="no")
# добавляем кнопки в клавиатуру  
keyboard.add(button1, button2)

@bot.message_handler(commands=['start', 'hi'])
def start_message(message):
  bot.send_message(message.chat.id, "Hello")
  # настройка reply_markup позволяет нам прикрепить клавиатуру к сообщению
  bot.send_message(message.chat.id, "Что сделать дальше?", reply_markup=keyboard)

""""""
@bot.callback_query_handler(func=lambda call: True)
def callback_query(call):
  # в call.data хранится то сообщение, которое мы указали в кнопках на втором месте
  # id вашего чата хранится в call.from_user.id
  if call.data == "yes":
    # отправляем стикер в чат
    bot.send_sticker(call.from_user.id, "CAACAgIAAxkBAAI4dGI-in_cfLBwj4iaDIvR2qhlxWpiAAKpFgACc_QAAUtyCXMIz-lEyiME")
  elif call.data == "no":
    bot.send_message(call.from_user.id, "Пока!")
""""""

@bot.message_handler(content_types=['sticker'])
def send_sticker(message):
  bot.send_sticker(message.chat.id, message.sticker.file_id)

bot.polling()
```
