# Exemples

Ces exemples sont des patterns complets utilisés dans un bot de tickets en production.

---

## 1. Notification éphémère simple

Le cas le plus courant : envoyer un message rapide à l’utilisateur.

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)
container.add_item(ui.TextDisplay("Tu es blacklisté et ne peux pas créer de ticket."))
layout.add_item(container)

await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## 2. Message d’information (titre + contenu)

Remplace un embed classique.

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)

container.add_item(ui.TextDisplay("### Ticket rouvert"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Rouver par {interaction.user.mention}"))

layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

## 3. Panel de ticket (Section + boutons)

Panel complet avec avatar + actions.

```python
layout = ui.LayoutView(timeout=None)
container = ui.Container(accent_color=None)

container.add_item(ui.TextDisplay("# Ticket de support"))
container.add_item(ui.Separator())

section = ui.Section(accessory=ui.Thumbnail(media=user.display_avatar.url))
section.add_item(ui.TextDisplay(
    f"**Bienvenue** {user.mention}\n"
    f"**Catégorie :** {category}\n\n"
    f"Un membre de l’équipe va vous répondre."
))

container.add_item(section)
container.add_item(ui.Separator())

row = ui.ActionRow(
    ui.Button(label="Claim", style=discord.ButtonStyle.primary, custom_id="ticket_claim_btn"),
    ui.Button(label="Close", style=discord.ButtonStyle.danger, custom_id="ticket_close_btn")
)

container.add_item(row)
layout.add_item(container)

await channel.send(view=layout)
```

---

## 4. Panel avec menu déroulant

Sélection de catégorie avant création du ticket.

```python
layout = ui.LayoutView(timeout=None)
container = ui.Container(accent_color=discord.Colour.blurple())

section = ui.Section(accessory=ui.Thumbnail(media=guild.icon.url))
section.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))

container.add_item(section)
container.add_item(ui.Separator())

options = [discord.SelectOption(label=name, value=name) for name, _ in categories]

row = ui.ActionRow(
    ui.Select(
        placeholder="Choisis une catégorie...",
        options=options,
        custom_id="ticket_category_select"
    )
)

container.add_item(row)
layout.add_item(container)

await channel.send(view=layout)
```

---

## 5. Log avec fichier (transcript)

Envoi d’un log dans un salon staff.

```python
transcript_file.seek(0)
file = discord.File(
    transcript_file,
    filename=f"ticket-{ticket_number:04d}-transcript.txt"
)

layout = ui.LayoutView()
container = ui.Container(accent_color=None)

container.add_item(ui.TextDisplay("### Logs - Ticket supprimé"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(
    f"Ticket `#{ticket_number:04d}` supprimé par {interaction.user.display_name}\n"
    f"**Catégorie :** {category}"
))

container.add_item(ui.Separator())
container.add_item(ui.File(media=f"attachment://ticket-{ticket_number:04d}-transcript.txt"))

layout.add_item(container)

await log_channel.send(view=layout, file=file)
```

---

## 6. Avatar avec MediaGallery

Afficher un avatar utilisateur.

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)

container.add_item(ui.TextDisplay(f"### Avatar de {user.display_name}"))
container.add_item(ui.Separator())

container.add_item(
    ui.MediaGallery(
        discord.ui.MediaGalleryItem(media=user.display_avatar.url)
    )
)

container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"[PNG]({avatar_url}.png) | [JPG]({avatar_url}.jpg)"))

layout.add_item(container)

await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## 7. LayoutView en classe (propre production)

Structure persistante avec callbacks.

```python
class TicketConfirmView(ui.LayoutView):
    def __init__(self, bot, category, user_id):
        super().__init__(timeout=180)

        container = ui.Container(accent_colour=discord.Colour.blue())

        container.add_item(ui.TextDisplay("### Règles du support"))
        container.add_item(ui.Separator())
        container.add_item(ui.TextDisplay("Veuillez lire les règles avant de créer un ticket."))

        row = ui.ActionRow(
            ui.Button(label="Ouvrir le ticket", style=discord.ButtonStyle.primary, custom_id="confirm_open"),
            ui.Button(label="Annuler", style=discord.ButtonStyle.danger, custom_id="cancel_ticket")
        )

        row.children[0].callback = self.confirm
        row.children[1].callback = self.cancel

        container.add_item(row)
        self.add_item(container)

    async def confirm(self, interaction):
        await interaction.response.send_modal(TicketModal(...))

    async def cancel(self, interaction):
        await interaction.response.edit_message(content="Annulé.", view=None)
```