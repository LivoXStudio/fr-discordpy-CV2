# Section & Thumbnail

`ui.Section` est un composant de mise en page qui place du texte **côte à côte** avec un accessoire (comme une image miniature). C’est utile pour afficher l’avatar d’un utilisateur à côté de ses informations, ou une icône de serveur à côté d’un titre.

---

## Utilisation de base

```python
section = ui.Section(accessory=ui.Thumbnail(media="https://example.com/image.png"))
section.add_item(ui.TextDisplay("**John Doe**\nTicket de support ouvert."))

container.add_item(section)
```

---

## Avec l’avatar d’un utilisateur

```python
section = ui.Section(accessory=ui.Thumbnail(media=user.display_avatar.url))
section.add_item(ui.TextDisplay(f"**{user.display_name}**\nCatégorie : Général"))

container.add_item(section)
```

---

## Avec l’icône d’un serveur

```python
icon_url = interaction.guild.icon.url if interaction.guild.icon else None

if icon_url:
    section = ui.Section(accessory=ui.Thumbnail(media=icon_url))
    section.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))
    container.add_item(section)
else:
    container.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))
```

---

## Comment ça s’affiche

```
┌─────────────────────────────────────────┐
│  Texte ici                        [img] │
│  Texte à gauche de l’image             │
└─────────────────────────────────────────┘
```

La miniature apparaît à **droite**, et le texte à **gauche**.

---

## Bouton comme accessoire

Un `ui.Button` peut aussi être utilisé comme accessoire de section, placé à droite du texte :

```python
section = ui.Section(accessory=ui.Button(label="+1", custom_id="increment_btn"))
section.add_item(ui.TextDisplay("Le compteur actuel est 0.", id=COUNT_TRACKER_ID))

container.add_item(section)
```

Cela permet de combiner action + texte de manière compacte sans utiliser un `ActionRow`.

---

## Notes

- `ui.Thumbnail` accepte uniquement une URL (pas de fichiers locaux).
- Une section peut contenir plusieurs `TextDisplay`.
- Une section doit être placée dans un `Container`.
- L’`accessory` peut être un `ui.Thumbnail` **ou** un `ui.Button` — les deux sont compatibles.