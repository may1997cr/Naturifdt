import discord
from discord.ext import commands
from discord import app_commands
import asyncio

intents = discord.Intents.default()
intents.messages = True
intents.message_content = True

bot = commands.Bot(command_prefix="/", intents=intents)
questions = []  # Liste, um die Fragen zu speichern
current_question = None  # Speichert die aktuell gesendete Frage
CHANNEL_ID = 1165197233692610566  # Die ID des Kanals, in den die Frage gesendet wird

@bot.event
async def on_ready():
    print(f'Bot ist online! Eingeloggt als {bot.user}')
    try:
        synced = await bot.tree.sync()
        print(f"Slash-Commands synchronisiert: {len(synced)} Befehle.")
    except Exception as e:
        print(f"Fehler beim Synchronisieren: {e}")

# /fdt - Frage einreichen
@bot.tree.command(name="fdt", description="Reiche eine Frage für die Frage des Tages ein.")
async def fdt(interaction: discord.Interaction, frage: str):
    questions.append(frage)
    await interaction.response.send_message(f"Die Frage wurde erfolgreich eingereicht: `{frage}`", ephemeral=True)

# /fdtlist - Fragen anzeigen
@bot.tree.command(name="fdtlist", description="Zeige die Liste der eingereichten Fragen an.")
async def fdtlist(interaction: discord.Interaction):
    if not questions:
        await interaction.response.send_message("Es gibt aktuell keine eingereichten Fragen.", ephemeral=True)
        return

    question_list = "\n".join([f"{idx + 1}. {q}" for idx, q in enumerate(questions)])
    await interaction.response.send_message(f"Eingereichte Fragen:\n{question_list}", ephemeral=True)

# /fdtnow - Frage senden
@bot.tree.command(name="fdtnow", description="Sende die aktuelle Frage des Tages in den Kanal.")
async def fdtnow(interaction: discord.Interaction):
    global current_question

    if not questions:
        await interaction.response.send_message("Es gibt keine Fragen in der Liste, die gesendet werden können.", ephemeral=True)
        return

    # Hole die erste Frage aus der Liste
    current_question = questions.pop(0)

    # Sende die Frage in den spezifischen Kanal
    channel = bot.get_channel(CHANNEL_ID)
    if channel is None:
        await interaction.response.send_message("Der spezifizierte Kanal konnte nicht gefunden werden.", ephemeral=True)
        return

    thread_message = await channel.send(f"**Frage des Tages:** {current_question}")

    # Erstelle einen Thread für die Frage
    thread = await thread_message.create_thread(name=f"Diskussion: {current_question[:50]}...")
    await interaction.response.send_message("Die Frage des Tages wurde gesendet und ein Thread wurde erstellt!", ephemeral=True)

# Füge Deinen Bot-Token hier ein
bot.run("MTIxODU4MDI2MTM5MTA0NDYzOA.GaPeC_.e4tk7cWuoxDqICMbIUJP-xGa8CQXb2rr9Jq-lc")
