# LayoutView

`ui.LayoutView` est la **racine** de chaque message utilisant Components V2. Tu ajoutes tous tes composants dedans, puis tu l’envoies via l’argument `view=` lors de l’envoi du message.

---

## Utilisation de base

```python
from discord import ui

layout = ui.LayoutView()
await channel.send(view=layout)
```

---

## Envoi en éphémère

```python
await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## Timeout

```python
layout = ui.LayoutView(timeout=180)
```

---

## Structure

```
LayoutView
└── Container
    └── TextDisplay
    └── Separator
    └── ActionRow
        └── Button
```

---

## Classe (option propre)

```python
class MyLayout(ui.LayoutView):
    text = ui.TextDisplay("Hello")
```

---

## find_item()

```python
COUNTER_ID = 1

class Counter(ui.LayoutView):
    container = ui.Container(
        ui.TextDisplay("0", id=COUNTER_ID)
    )

async def callback(self, interaction):
    display = self.view.find_item(COUNTER_ID)
    display.content = "1"
    await interaction.response.edit_message(view=self.view)
```

---

## Notes

- Les `LayoutView` sont **invisibles** : toute l’interface visuelle vient uniquement des composants à l’intérieur.
- Tu peux ajouter jusqu’à **40 composants maximum** dans une seule `LayoutView`.
- Tu **ne peux pas mélanger** `LayoutView` et les anciennes `discord.ui.View` dans un même message (⚠️ Tu peux utiliser les deux dans ton bot, mais pas ensemble dans une même réponse.)
- Utilise `timeout=None` pour créer une view persistante qui continue de fonctionner même après un redémarrage du bot.