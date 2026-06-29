# ActionRow & Buttons (CV2)

`ui.ActionRow` est utilisé en CV2 pour placer des **boutons** et des **menus déroulants** directement dans un `Container`.  
Il remplace l’ancien système basé sur `discord.ui.View`.

---

# ActionRow (CV2)

## Boutons dans un Container

```python
row = ui.ActionRow(
    ui.Button(label="Confirmer", style=discord.ButtonStyle.success, custom_id="confirm"),
    ui.Button(label="Annuler", style=discord.ButtonStyle.danger, custom_id="cancel")
)

container.add_item(row)
```

---

## Callback manuel (sans décorateurs)

Dans CV2, tu attaches les callbacks directement :

```python
row = ui.ActionRow(
    ui.Button(label="Fermer le ticket", style=discord.ButtonStyle.danger, custom_id="close_btn")
)

row.children[0].callback = self.close_callback

container.add_item(row)
```

---

## Select menu dans une ActionRow

```python
options = [
    discord.SelectOption(label="Général", value="general"),
    discord.SelectOption(label="Facturation", value="billing"),
]

row = ui.ActionRow(
    ui.Select(
        placeholder="Choisis une catégorie...",
        options=options,
        custom_id="category_select"
    )
)

row.children[0].callback = self.select_callback

container.add_item(row)
```

---

## Limite de boutons

Une ActionRow peut contenir :
- maximum 5 boutons
- ou 1 select menu

```python
buttons = [
    ui.Button(label=name, custom_id=f"btn_{name}")
    for name in categories[:5]
]

row = ui.ActionRow(*buttons)
container.add_item(row)
```

---

## Plusieurs lignes de boutons

```python
for i in range(0, len(buttons), 5):
    row = ui.ActionRow(*buttons[i:i+5])
    container.add_item(row)
```

---

## Notes CV2

- ActionRow est obligatoire pour regrouper les composants interactifs
- Pas de décorateurs `@ui.button` en CV2
- `custom_id` est obligatoire pour les boutons persistants

---

# Buttons (discord.py traditionnel / View)

> Version alternative avec `discord.ui.View` (hors CV2)

## Styles de boutons

```python
import discord

discord.ButtonStyle.primary
discord.ButtonStyle.secondary
discord.ButtonStyle.success
discord.ButtonStyle.danger
discord.ButtonStyle.link
```

`link` n’utilise pas de callback, uniquement `url=`.

---

## Exemple simple avec décorateur

```python
import discord
from discord import ui

class MyView(discord.ui.View):

    @discord.ui.button(label="Clique-moi", style=discord.ButtonStyle.primary)
    async def button_callback(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Tu as cliqué !", ephemeral=True)
```

---

## Envoi du View

```python
@bot.command()
async def hello(ctx):
    await ctx.send(view=MyView())
```

---

## Plusieurs boutons

```python
class ConfirmView(discord.ui.View):

    @discord.ui.button(label="Oui", style=discord.ButtonStyle.success)
    async def yes(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Confirmé", ephemeral=True)

    @discord.ui.button(label="Non", style=discord.ButtonStyle.danger)
    async def no(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Annulé", ephemeral=True)
```

---

## Notes View classique

- Les décorateurs fonctionnent uniquement dans `discord.ui.View`
- Non compatible avec `LayoutView`
- Plus simple mais moins flexible que CV2