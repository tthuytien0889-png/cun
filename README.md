# cun
import os
import random
import discord
from discord import app_commands
from discord.ext import commands

intents = discord.Intents.default()

bot = commands.Bot(
    command_prefix="!",
    intents=intents
)


@bot.event
async def on_ready():
    await bot.tree.sync()
    print(f"🌷 Kwiie đã online: {bot.user}")


@bot.tree.command(name="ping", description="Kiểm tra Kwiie có online không 💗")
async def ping(interaction: discord.Interaction):
    await interaction.response.send_message(
        f"🌷 Pong! `{round(bot.latency * 1000)}ms`"
    )


@bot.tree.command(name="hello", description="Kwiie chào bạn 🌸")
async def hello(interaction: discord.Interaction):
    await interaction.response.send_message(
        f"૮ ˶ᵔ ᵕ ᵔ˶ ა Xin chào {interaction.user.mention}! 💗"
    )


@bot.tree.command(name="roll", description="Kwiie tung xúc xắc 🎲")
async def roll(interaction: discord.Interaction):
    number = random.randint(1, 6)
    await interaction.response.send_message(
        f"🎀 Kwiie tung xúc xắc...\n✨ Bạn nhận được **{number}**!"
    )


@bot.tree.command(name="coinflip", description="Tung đồng xu 🪙")
async def coinflip(interaction: discord.Interaction):
    result = random.choice(["Mặt ngửa 🌸", "Mặt sấp 🌷"])
    await interaction.response.send_message(
        f"🪙 Kwiie tung đồng xu...\n💗 **{result}**"
    )


@bot.tree.command(name="8ball", description="Hỏi Kwiie một câu 🔮")
@app_commands.describe(question="Câu hỏi của bạn")
async def eightball(interaction: discord.Interaction, question: str):
    answers = [
        "Có đó nhaaa 💗",
        "Mình nghĩ là có 🌷",
        "Khả năng cao đó ✨",
        "Hmm... chưa chắc đâu 🥺",
        "Có vẻ là không á 😭",
        "Thử lại sau nha 🌸"
    ]

    await interaction.response.send_message(
        f"🔮 **Câu hỏi:** {question}\n"
        f"🎀 **Kwiie:** {random.choice(answers)}"
    )


@bot.tree.command(name="avatar", description="Xem avatar của bạn 🖼️")
async def avatar(interaction: discord.Interaction):
    embed = discord.Embed(
        title=f"🌷 Avatar của {interaction.user.display_name}",
        color=discord.Color.from_rgb(180, 220, 255)
    )

    embed.set_image(url=interaction.user.display_avatar.url)

    await interaction.response.send_message(embed=embed)


@bot.tree.command(name="help", description="Xem các lệnh của Kwiie 📖")
async def help_command(interaction: discord.Interaction):
    embed = discord.Embed(
        title="🎀 Kwiie — Menu lệnh",
        description="Một chiếc bot nhỏ xinh cho server của bạn 🌷",
        color=discord.Color.from_rgb(180, 220, 255)
    )

    embed.add_field(
        name="🌸 Fun",
        value=(
            "`/ping` — kiểm tra bot\n"
            "`/hello` — Kwiie chào bạn\n"
            "`/roll` — tung xúc xắc\n"
            "`/coinflip` — tung đồng xu\n"
            "`/8ball` — hỏi Kwiie\n"
        ),
        inline=False
    )

    embed.add_field(
        name="🎀 Profile",
        value="`/avatar` — xem avatar",
        inline=False
    )

    embed.set_footer(text="Kwiie ♡ cute bot")

    await interaction.response.send_message(embed=embed)


TOKEN = os.getenv("DISCORD_TOKEN")

if not TOKEN:
    raise RuntimeError("Chưa cài DISCORD_TOKEN trên Render!")

bot.run(TOKEN)
