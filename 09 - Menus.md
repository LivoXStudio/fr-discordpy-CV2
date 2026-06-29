# Select Menus (discord.py)

Les menus déroulants (`Select`) sont placés dans une `ui.ActionRow` et utilisent la syntaxe par décorateurs pour définir les callbacks. Un seul select menu par row.

> Requiert **discord.py 2.7+**

---

## Types de Select Menus

| Classe | Fonction |
|-------|----------|
| `ui.Select` | Liste personnalisée d’options |
| `ui.UserSelect` | Sélectionner un utilisateur |
| `ui.RoleSelect` | Sélectionner un rôle |
| `ui.ChannelSelect` | Sélectionner un salon |
| `ui.MentionableSelect` | Sélectionner un utilisateur ou un rôle |

---

## Menu de sélection classique (String Select)

```python
import discord
from discord import ui


class MyLayout(ui.LayoutView):

    def __init__(self):
        super().__init__(timeout=120)

        container = ui.Container(
            discord.ui.TextDisplay("Choisis une catégorie de support ci-dessous.")
        )

        select = ui.Select(
            placeholder="Choisis une catégorie...",
            options=[
                discord.SelectOption(label="Général", value="general", description="Support général"),
                discord.SelectOption(label="Facturation", value="billing", description="Problèmes de paiement"),
                discord.SelectOption(label="Technique", value="technical", description="Aide technique"),
            ]
        )

        select.callback = self.category_select

        container.add_item(ui.ActionRow(select))
        self.add_item(container)

    async def category_select(self, interaction: discord.Interaction):
        value = interaction.data["values"][0]

        await interaction.response.send_message(
            f"Tu as choisi : {value}",
            ephemeral=True
        )
```

---

## Menu de sélection d'utilisateur (UserSelect)

```python
import discord
from discord import ui


class MyLayout(ui.LayoutView):

    def __init__(self):
        super().__init__(timeout=120)

        container = ui.Container(
            discord.ui.TextDisplay("Choisis une personne ci-dessous.")
        )

        user_select = ui.UserSelect(
            placeholder="Choisis une personne...",
            custom_id="user_select"
        )

        user_select.callback = self.user_callback

        container.add_item(ui.ActionRow(user_select))
        self.add_item(container)

    async def user_callback(self, interaction: discord.Interaction):
        user = interaction.data["values"][0]

        await interaction.response.send_message(
            f"Tu as choisi {user}",
            ephemeral=True
        )
```

---

## Menu de sélection de salons (ChannelSelect)

```python
import discord
from discord import ui


class MyLayout(ui.LayoutView):

    def __init__(self):
        super().__init__(timeout=120)

        container = ui.Container(
            discord.ui.TextDisplay("Choisis un salon ci-dessous.")
        )

        channel_select = ui.ChannelSelect(
            placeholder="Choisis un salon...",
            custom_id="channel_select",
            channel_types=[discord.ChannelType.text]
        )

        channel_select.callback = self.channel_callback

        container.add_item(ui.ActionRow(channel_select))
        self.add_item(container)

    async def channel_callback(self, interaction: discord.Interaction):
        channel = interaction.data["values"][0]

        await interaction.response.send_message(
            f"Tu as choisi {channel}",
            ephemeral=True
        )
```
**Ici, tu peux choisir entre différents salons : `Text | Voice | Category...`**

---

## Menu Mentionnable (MentionableSelect)

```python
import discord
from discord import ui


class MyLayout(ui.LayoutView):

    def __init__(self):
        super().__init__(timeout=120)

        container = ui.Container(
            discord.ui.TextDisplay("Choisis un utilisateur ou un rôle.")
        )

        mentionable_select = ui.MentionableSelect(
            placeholder="Choisis un utilisateur ou un rôle...",
            custom_id="mentionable_select"
        )

        mentionable_select.callback = self.mentionable_callback

        container.add_item(ui.ActionRow(mentionable_select))
        self.add_item(container)

    async def mentionable_callback(self, interaction: discord.Interaction):
        value = interaction.data["values"][0]

        await interaction.response.send_message(
            f"Tu as choisi {value}",
            ephemeral=True
        )
```

---

## Notes

- `ActionRow` contient **1 seul select menu**
- Les callbacks doivent être assignés manuellement en CV2
- `custom_id` est obligatoire pour les menus persistants
- Chaque type de select renvoie un type différent (user, role, channel, etc.)