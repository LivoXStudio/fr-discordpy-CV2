# File Component

`ui.File` permet d’afficher un fichier joint **directement dans une mise en page CV2**. C’est utilisé pour afficher des transcripts, logs ou tout fichier uploadé dans un message structuré.

---

## Utilisation de base

Tu as besoin de deux éléments :
1. Un objet `discord.File` (le fichier à envoyer)
2. Un composant `ui.File` qui pointe vers ce fichier via `attachment://nom_du_fichier`

```python
import discord
from discord import ui
import io

class FileView(ui.LayoutView):
    def __init__(self, ctx: commands.Context, file):
        super().__init__()
        self.ctx = ctx
        self.file = file

        container = ui.Container(accent_color=discord.Color.blurple())
        container.add_item(ui.File(media="attachment://fichier-exemple.txt"))
        self.add_item(container)

class FileCog(commands.Cog):
    def __init__(self, bot):
        self.bot = bot

    @commands.command(name="exemple")
    @checks.guild_only()
    @checks.owner()
    async def exemple(self, ctx: commands.Context):
        file = discord.File(io.BytesIO(b"Hello, this is the file content."), filename="fichier-exemple.txt")
        view = Exemple(ctx, file)
        await ctx.send(view=view, file=file)
```

---

## Depuis un buffer en mémoire

```python
import io

buffer = io.StringIO("Bonjour, voici le contenu du fichier.")
file = discord.File(buffer, filename="output.txt")

container.add_item(ui.File(media="attachment://output.txt"))

await channel.send(view=layout, file=file)
```

---

## Envoi en MP

```python
await user.send(view=layout, file=file)
```

---

## Exemple dans un log

```python
container.add_item(ui.TextDisplay("### Transcript du ticket"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Ticket fermé par {user.mention}"))
container.add_item(ui.File(media="attachment://ticket-0042-transcript.txt"))
```

---

## Notes

- Le nom dans `attachment://` doit être **exactement identique** au nom donné dans `discord.File`.
- Si tu écris dans un buffer, pense à faire `buffer.seek(0)` avant de l’envoyer.
- Discord limite généralement à **un fichier par message** dans ce contexte.