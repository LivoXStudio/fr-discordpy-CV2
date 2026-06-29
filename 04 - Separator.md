# Separator

`ui.Separator` ajoute une ligne horizontale de séparation entre les composants dans un container. C’est l’équivalent CV2 d’une pause visuelle — parfait pour séparer un titre du contenu ou diviser des sections.

---

## Utilisation de base

```python
container.add_item(ui.TextDisplay("### Titre"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("Contenu ici."))
```

---

## Variantes d’espacement

Tu peux contrôler l’espace autour du séparateur :

```python
container.add_item(ui.Separator(spacing=discord.SeparatorSpacing.small))
container.add_item(ui.Separator(spacing=discord.SeparatorSpacing.large))
```

---

## Séparateur invisible (espace uniquement)

Si tu veux uniquement un espacement sans ligne visible :

```python
container.add_item(ui.Separator(divider=False))
```

Cela crée un espace vide sans afficher de ligne.

---

## Exemple courant

```python
container.add_item(ui.TextDisplay("### Ticket fermé"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Fermé par {user.mention}"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("Une transcription a été envoyée en MP."))
```

---

## Notes

- Les séparateurs sont uniquement visuels — ils ne contiennent aucune donnée.
- Tu peux en utiliser autant que tu veux dans un container.