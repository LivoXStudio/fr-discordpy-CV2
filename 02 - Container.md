# Container

`ui.Container` est le bloc principal d’une mise en page CV2. Il permet de regrouper plusieurs composants ensemble et peut ajouter une barre de couleur à gauche — comme sur les embeds Discord.

---

## Utilisation de base

```python
container = ui.Container()
container.add_item(ui.TextDisplay("Bonjour !"))

layout.add_item(container)
```

---

## Avec une couleur d’accent

```python
container = ui.Container(accent_color=discord.Colour.blurple())
```

Tu peux utiliser n’importe quelle couleur `discord.Colour` :

```python
ui.Container(accent_color=discord.Colour.green())
ui.Container(accent_color=discord.Colour.red())
ui.Container(accent_color=discord.Colour.from_rgb(255, 165, 0))
```

`accent_color` et `accent_colour` fonctionnent tous les deux (les deux orthographes sont acceptées).

---

## Sans couleur (par défaut)

```python
container = ui.Container(accent_color=None)
```

Mettre `None` affiche un conteneur sans barre de couleur, uniquement un cadre simple.

---

## Ajouter plusieurs éléments

```python
container = ui.Container(accent_color=discord.Colour.blue())
container.add_item(ui.TextDisplay("### Titre"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("Du texte ici."))
```

---

## Notes

- Une `LayoutView` peut contenir **plusieurs containers**.
- Un container peut contenir : `TextDisplay`, `Separator`, `Section`, `MediaGallery`, `File`, `ActionRow`.
- Les containers **ne peuvent pas être imbriqués entre eux**.
