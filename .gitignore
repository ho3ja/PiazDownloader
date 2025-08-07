import os
import requests
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, ContextTypes, filters
from dotenv import load_dotenv

load_dotenv()
BOT_TOKEN = os.getenv("BOT_TOKEN")

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("سلام! لینک اینستاگرامتو بفرست تا برات دانلودش کنم.")

async def handle_instagram_link(update: Update, context: ContextTypes.DEFAULT_TYPE):
    url = update.message.text
    if "instagram.com" not in url:
        await update.message.reply_text("این لینک اینستاگرام نیست 😕 لطفاً لینک درست بفرست.")
        return

    await update.message.reply_text("⏳ در حال پردازش لینک...")

    try:
        api_url = "https://igram.io/api/ajaxSearch"
        headers = {
            "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
            "User-Agent": "Mozilla/5.0",
        }
        data = f"q={url}&t=media"
        response = requests.post(api_url, headers=headers, data=data)
        json_data = response.json()
        media_list = json_data.get("data", {}).get("medias", [])

        if not media_list:
            await update.message.reply_text("❌ نتونستم چیزی پیدا کنم.")
            return

        for media in media_list:
            m_url = media.get("url")
            m_type = media.get("type")
            if m_type == "video":
                await update.message.reply_video(video=m_url)
            else:
                await update.message.reply_photo(photo=m_url)

    except Exception as e:
        print(e)
        await update.message.reply_text("🚫 خطا در پردازش لینک. لطفاً دوباره امتحان کن.")

if __name__ == '__main__':
    app = ApplicationBuilder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_instagram_link))
    print("✅ ربات در حال اجراست...")
    app.run_polling()
