# Tips & Gotchas

Erreurs fréquentes, limites et points importants à connaître avant de déployer.

---

## Installer la bonne version

```bash
git+https://github.com/Rapptz/discord.py
```

CV2 n’est **pas encore sur PyPI**.  
Si tu vois :
```
AttributeError: module 'discord.ui' has no attribute 'LayoutView'
```

👉 tu n’utilises pas la bonne version.

---

## `ui.View` classique fonctionne toujours

CV2 est **optionnel par message** — tu n’es pas obligé de migrer tout ton bot.

Tu peux utiliser :
- `ui.LayoutView` sur certains messages
- `ui.View` sur d’autres

Ils coexistent sans conflit.

---

## Pas d’embed, content, stickers ou polls avec CV2

Quand tu envoies un message CV2, tu ne peux PAS utiliser :

- `embed=` / `embeds=`
- `content=`
- `stickers=`
- `poll=`

CV2 remplace tout ça.

```python
# Faux
await channel.send(embed=embed, view=layout)
await channel.send(content="Hello", view=layout)

# Correct
await channel.send(view=layout)
```

Utilise `ui.TextDisplay` à la place.

---

## Les containers ne peuvent pas être imbriqués

Tu ne peux pas mettre un `Container` dans un autre :

```
LayoutView
├── Container
└── Container
    └── Container ❌ interdit
```

---

## `accent_color` vs `accent_colour`

Les deux fonctionnent :

```python
ui.Container(accent_color=discord.Colour.red())
ui.Container(accent_colour=discord.Colour.red())
```

---

## Toujours reset les buffers

Si tu utilises `io.StringIO` ou `io.BytesIO` :

```python
buffer.seek(0)
file = discord.File(buffer, filename="transcript.txt")
```

Sinon tu envoies un fichier vide.

---

## `attachment://` doit être exact

```python
file = discord.File(buf, filename="report.txt")

container.add_item(ui.File(media="attachment://report.txt"))  # OK
container.add_item(ui.File(media="attachment://Report.txt"))  # Faux (case sensitive)
```

---

## Views persistantes = `custom_id` obligatoire

Pour les boutons/selects qui survivent à un restart :

```python
bot.add_view(MyPersistentView())
```

Sans ça, ils ne fonctionnent plus après redémarrage.

---

## Callbacks dans CV2

Pas de décorateurs dans `LayoutView`.  
Tu assignes manuellement :

```python
row = ui.ActionRow(
    ui.Button(label="Clique", custom_id="btn")
)

row.children[0].callback = self.callback
```

---

## Ephemeral

Un message ephemeral :
- est invisible pour les autres utilisateurs
- ne peut être modifié que par le bot

---

## Limites importantes

| Élément | Limite |
|--------|--------|
| Composants par LayoutView | 40 max |
| Boutons par ActionRow | 5 |
| Options de Select | 25 |
| Images MediaGallery | 10 |
| TextDisplay | ~2000 caractères |

---

## Point le plus important

La limite des 40 composants inclut tout :
Containers, textes, séparateurs, boutons, sections, rows, même imbriqués.