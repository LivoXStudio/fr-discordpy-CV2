# TextDisplay

`ui.TextDisplay` permet d’afficher du texte Markdown dans une mise en page CV2. C’est le principal moyen d’afficher du contenu écrit : titres, descriptions, informations type “fields”, etc.

---

## Utilisation de base

```python
container.add_item(ui.TextDisplay("Bonjour, monde !"))
```

---

## Markdown entièrement supporté

```python
container.add_item(ui.TextDisplay("### Ticket ouvert"))
container.add_item(ui.TextDisplay("**Statut :** Ouvert"))
container.add_item(ui.TextDisplay(">>> Ceci est une citation"))
container.add_item(ui.TextDisplay("`code en ligne` fonctionne aussi"))
```

---

## Texte sur plusieurs lignes

```python
container.add_item(ui.TextDisplay(
    "**Utilisateur :** John\n"
    "**Catégorie :** Support\n"
    "**Créé :** <t:1700000000:R>"
))
```

---

## Timestamps Discord

Le format de timestamps Discord fonctionne dans `TextDisplay` :

```python
import discord

ts = int(discord.utils.utcnow().timestamp())

container.add_item(ui.TextDisplay(f"Ouvert <t:{ts}:R>"))  # ex : "il y a 5 minutes"
container.add_item(ui.TextDisplay(f"Ouvert <t:{ts}:F>"))  # ex : "lundi 1 janvier 2024 12:00"
```

---

## Mentions utilisateurs et salons

```python
container.add_item(ui.TextDisplay(f"Créé par {user.mention}"))
container.add_item(ui.TextDisplay(f"Dans {channel.mention}"))
```

---

## Notes

- Tu peux ajouter autant de `TextDisplay` que tu veux dans un container.
- Il n’existe pas de système titre/description comme les embeds — utilise le Markdown (`###`, `**gras**`, etc.).
- Les emojis (Unicode et emojis Discord personnalisés) s’affichent correctement.