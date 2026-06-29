# Discord.py Components V2 — Guide pour débutants

> Guide rédigé par **itsfizys** et traduit en français par **contextualisation**\
> Serveur de support : **https://discord.gg/aerox** \
> Guide originel : [GITHUB CV2](https://github.com/itsfizys/discordpy-components-v2-guide/blob/main/README.md)

Les **Components V2** sont le nouveau système de mise en page des messages de Discord. Ils vous offrent un **contrôle total** sur l'apparence des messages de votre bot. Plus puissants et plus flexibles que les embeds traditionnels, ils permettent de créer des interfaces modernes et entièrement personnalisables.

---

# Quelles sont les nouveautés des Components V2 ?

* Mise en page entièrement personnalisable.
* Texte enrichi avec une prise en charge complète du Markdown.
* Interface moderne avec des sections, des images et des espacements.
* Les boutons et les menus déroulants peuvent être intégrés directement dans la mise en page.
* Les paramètres `content`, `embeds`, `stickers` et `polls` **ne peuvent pas** être utilisés en même temps que les Components V2.

---

# Les composants principaux

| Composant         | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| `ui.LayoutView`   | Conteneur principal. Tous les messages CV2 commencent par celui-ci.          |
| `ui.Container`    | Bloc de mise en page principal avec une couleur d'accent facultative.        |
| `ui.TextDisplay`  | Affiche du texte avec une prise en charge complète du Markdown.              |
| `ui.Section`      | Regroupe du texte avec une miniature (*thumbnail*) ou un bouton sur le côté. |
| `ui.Separator`    | Ajoute une ligne de séparation ou un espace entre les composants.            |
| `ui.MediaGallery` | Affiche plusieurs images sous forme de galerie.                              |
| `ui.File`         | Affiche un fichier directement dans le message.                              |
| `ui.ActionRow`    | Ajoute des boutons ou des menus de sélection dans la mise en page.           |

---

# Exemple simple

```python
import discord
from discord import ui
from discord.ext import commands

class View(ui.LayoutView):
    def __init__(self):
        super().__init__(timeout=120)
    
        container = ui.Container(accent_color=discord.Colour.blurple())
        container.add_item(ui.TextDisplay("# Bienvenue !\n**Cliquez sur le bouton ci-dessous.**"))
        button = ui.Button(label="Dire bonjour", style=discord.ButtonStyle.primary, custom_id="hello_btn")
        button.callback = self.hello_btn
        container.add_item(ui.ActionRow(button))
        
        self.add_item(container)
    
    async def hello_btn(self, interaction: discord.Interaction):
        await interaction.response.send_message("Salut !", ephemeral=True)

class Cog(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
    
    @commands.command(name="hello")
    @commands.has_permissions(administrator=True)
    async def hello(self, ctx):
        view = View()
        await ctx.send(view=view)
```

---

# Gérer le clic d'un bouton

```python
# Dans votre sous-classe de LayoutView
row = ui.ActionRow(ui.Button(label="Dire bonjour", custom_id="hello_btn"))
row.children[0].callback = self.on_hello
container.add_item(row)

async def on_hello(self, interaction: discord.Interaction):
    await interaction.response.send_message("Bonjour ! 👋", ephemeral=True)
```

# Tu peux aussi faire :
```python
bouton = ui.Button(label="Dire bonjour", custom_id="hello_btn")
bouton.callback = self.hello_btn
container.add_item(ui.ActionRow(bouton))
```
---

# Exemple de mise en page (style tableau de bord)

```python
container = ui.Container(accent_color=discord.Colour.dark_gray())
container.add_item(ui.TextDisplay("# Statistiques du serveur"))
container.add_item(ui.Separator())

section = ui.Section(
    accessory=ui.Button(label="Actualiser", style=discord.ButtonStyle.secondary, custom_id="refresh_stats")
)
section.add_item(ui.TextDisplay("**Membres en ligne :** 1 234"))
section.add_item(ui.TextDisplay("**Messages aujourd'hui :** 5 678"))
container.add_item(section)
```

---

# Afficher des images

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/image1.png")
gallery.add_item(media="https://example.com/image2.png")

container.add_item(gallery)
```

---

# Migrer depuis les embeds

| Ancien système          | Équivalent Components V2                  |
| ----------------------- | ----------------------------------------- |
| `embed.title`           | `ui.TextDisplay("# Titre")`               |
| `embed.description`     | `ui.TextDisplay("du texte")`              |
| `embed.add_field()`     | `ui.Section` contenant des `TextDisplay`  |
| `embed.set_image()`     | `ui.MediaGallery`                         |
| `embed.set_thumbnail()` | `ui.Section(accessory=ui.Thumbnail(...))` |
| `embed.color`           | `ui.Container(accent_color=...)`          |

---

# Conseils importants

* La **limite totale** est de **40 composants par message** (conteneurs compris).
* Installez **discord.py** directement depuis GitHub : la version disponible sur PyPI ne prend pas encore en charge les Components V2.
* Il faut **TOUJOURS** utiliser une class ui.LayoutView lors de l'utilisation d'un bouton ou d'un menu.
```bash
pip install git+https://github.com/Rapptz/discord.py
```

* Les anciennes `ui.View` continuent de fonctionner. Les Components V2 sont **optionnels** et peuvent être utilisés message par message.
* Utilisez `ui.Separator` pour améliorer la lisibilité grâce aux espacements et séparateurs.
* Pensez à tester vos mises en page sur téléphone pour ajuster votre View.

---

# Avancé : Exemple de pagination des administrateurs en Cog

```python
import discord
from discord import ui

class PaginatedLayout(ui.LayoutView):
    def __init__(self, ctx, items: list, page: int = 0):
        super().__init__(timeout=120)
        self.ctx = ctx
        self.items = items
        self.page = page
        self._build()

    def _build(self):
        chunk = self.items[self.page * 10 : (self.page + 1) * 10]

        container = ui.Container(accent_color=None)
        container.add_item(ui.TextDisplay(f"# Page {self.page + 1}"))
        container.add_item(ui.Separator())

        for item in chunk:
            container.add_item(ui.TextDisplay(
                    f"👥 Mention: {item['mention']}\n"
                    f"🆔 ID: `{item['id']}`\n"
                    f"🗓️ Rejoint:  <t:{item['timestamp']}:R>\n"
                    f"🛡️ Toprole: {item['toprole']}"
            ))

        buttons = []
        if self.page > 0:
            bouton_prev = ui.Button(label="Précédent", custom_id="prev_page", style=discord.ButtonStyle.secondary)
            bouton_prev.callback = self.prev_page
            buttons.append(bouton_prev)
        if (self.page + 1) * 10 < len(self.items):
            bouton_next = ui.Button(label="Suivant", custom_id="next_page", style=discord.ButtonStyle.secondary)
            bouton_next.callback = self.next_page
            buttons.append(bouton_next)
            
        if buttons:
            container.add_item(ui.ActionRow(*buttons))

        self.add_item(container)

    async def interaction_check(self, interaction: discord.Interaction):
        if self.ctx.author.id != interaction.user.id:
            await interaction.response.send_message(f"❌ Tu ne peux pas gérer ces boutons.", ephemeral=True)
            return False
        return True
        
    async def prev_page(self, interaction: discord.Interaction):
        if self.page == 0:
            return
        
        self.page -= 1
        self.clear_items()
        self._build()
        await interaction.response.edit_message(view=self)

    async def next_page(self, interaction: discord.Interaction):
        if (self.page + 1) * 10 >= len(self.items):
            return

        self.page += 1
        self.clear_items()
        self._build()
        await interaction.response.edit_message(view=self)
        
class Cog(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        
    @commands.command(name="alladmins")
    @commands.has_permissions(administrator=True)
    async def alladmins(self, ctx):
            admins = [m for m in ctx.guild.members if m.guild_permissions.administrator]
            items = [
                {
                    "mention": admin.mention,
                    "id": admin.id,
                    "timestamp": int(admin.joined_at.timestamp()) if admin.joined_at else 0,
                    "toprole": admin.top_role.mention if admin.top_role else "Aucun"
                }
                for admin in admins
            ]

            view = PaginatedLayout(ctx, items)
            view.message = await ctx.send(view=view, allowed_mentions=discord.AllowedMentions(users=[], roles=[]))

```

---

# Sommaire

| #  | Guide                   | Description                                                     |
| -- | ----------------------- | --------------------------------------------------------------- |
| 1  | **LayoutView**          | Conteneur principal de tous les messages Components V2.         |
| 2  | **Container**           | Regrouper des composants avec une couleur d'accent facultative. |
| 3  | **TextDisplay**         | Afficher du texte avec le Markdown.                             |
| 4  | **Separator**           | Ajouter des séparateurs et des espacements.                     |
| 5  | **Section & Thumbnail** | Associer du texte à une image ou un bouton.                     |
| 6  | **MediaGallery**        | Afficher une galerie d'images.                                  |
| 7  | **File Component**      | Afficher des fichiers directement dans le message.              |
| 8  | **ActionRow**           | Ajouter des boutons et des menus de sélection.                  |
| 9  | **Exemples concrets**   | Cas d'utilisation issus d'un véritable bot de tickets.          |
| 10 | **Conseils & pièges**   | Limites, erreurs fréquentes et bonnes pratiques.                |
