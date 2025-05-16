from telegram import Update
from telegram.ext import ApplicationBuilder, MessageHandler, filters, ContextTypes

BOT_TOKEN = '7995821899:AAH3b-ARXz1-vSK5QqlWaua4PEk1Kx-36ZA'
OWNER_ID = 7775841397  # آیدی عددی شما

async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.effective_user.id == OWNER_ID:
        # پیام شما برای یکی از افراد
        try:
            parts = update.message.text.split(\" \", 1)
            user_id = int(parts\\[0\\])
            message = parts\\[1\\]
            await context.bot.send_message(chat_id=user_id, text=message)
        except:
            await update.message.reply_text(\"فرمت اشتباه! مثلاً: 123456789 سلام\")
    else:
        # پیام از طرف بقیه، فرستادنش به شما
        text = update.message.text
        sender_id = update.effective_user.id
        await context.bot.send_message(chat_id=OWNER_ID, text=f\"پیام جدید:\\\\n{text}\\\\n\\\\nUserID: {sender_id}\")

app = ApplicationBuilder().token(BOT_TOKEN).build()
app.add_handler(MessageHandler(filters.TEXT & \\~filters.COMMAND, handle_message))
app.run_polling()