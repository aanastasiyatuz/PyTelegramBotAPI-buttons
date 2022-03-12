# Кнопки в PyTelegramBotApi

В телеграме есть 2 вида кнопок:
* которые находятся в клавиатуре
* которые находятся под сообщением

Чтобы пользоваться кнопками нужно импортировать библеотеку
```py
from telebot import types
```

# Кнопки, которые находятся в клавиатуре

Чтобы создать кнопки 1 типа нужно создать клавиатуру и добавить туда кнопки:

```py
# создаем клавиатуру:
keyboard = types.ReplyKeyboardMarkup(row_width=2)
# создаем кнопки, в них указываем текст, который в них будет хранится:
button1 = types.KeyboardButton('a')
button2 = types.KeyboardButton('b')
button3 = types.KeyboardButton('c')
# добавляем кнопки в клавиатуру:
keyboard.add(button1, button2, button3)
```

Чтобы указать, после какого сообщения будет отображаться клавиатура, мы к этому сообщению добавляем клавиатуру:

```py
bot.send_message(chat_id, 'Сообщение с клавиатурой', reply_markup=keyboard)
```

# Кнопки, которые находятся под сообщением

Чтобы создать кнопки 2 типа, мы так же должны создать клавиатуру и добавить туда кнопки:

```py
# создаем клавиатуру
keyboard = types.InlineKeyboardMarkup()
# создаем кнопки, в скобочках первое - сообщение внутри кнопки, 
# второе - текст по которому мы поймем, что именно эта кнопка была нажата (в телеграме этого не будет видно)
button1 = types.InlineKeyboardButton("Yes", callback_data="cb_yes")
button2 = types.InlineKeyboardButton("No", callback_data="cb_no")
# добавляем кнопки в клавиатуру
keyboard.add(button1, button2)
```

Чтобы указать, к какому сообщению будет прикреплена клавиатура, мы к этому сообщению добавляем клавиатуру:

```py
bot.send_message(chat_id, 'Сообщение с кнопками', reply_markup=keyboard)
```

Чтобы бот знал, что делать после нажатия на эти кнопки мы можем создать еще одну функцию

```py
@bot.callback_query_handler(func=lambda call: True)
def callback_query(call):
    # в call.data хранится то сообщение, которое мы указали в кнопках на втором месте
    if call.data == "cb_yes":
        # отправляем сообщение обратно в чат
        bot.answer_callback_query(call.id, "Answer is Yes")
    elif call.data == "cb_no":
        bot.answer_callback_query(call.id, "Answer is No")
```
