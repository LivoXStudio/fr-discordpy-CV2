# MediaGallery

`ui.MediaGallery` permet d’afficher une ou plusieurs images sous forme de grille dans un container. C’est l’équivalent CV2 des `embed.set_image()` ou des aperçus de pièces jointes.

---

## Image unique

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/banner.png")

container.add_item(gallery)
```

---

## Plusieurs images (grille)

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/image1.png")
gallery.add_item(media="https://example.com/image2.png")
gallery.add_item(media="https://example.com/image3.png")

container.add_item(gallery)
```

Plusieurs images sont affichées sous forme de grille dans Discord.

---

## Utiliser un MediaGalleryItem directement

```python
item = discord.ui.MediaGalleryItem(media="https://example.com/avatar.png")
gallery = ui.MediaGallery(item)

container.add_item(gallery)
```

---

## Exemple avec avatar utilisateur

```python
gallery = ui.MediaGallery()
gallery.add_item(media=user.display_avatar.url)

container.add_item(gallery)
```

---

## Bon pattern de placement

Les galeries sont souvent placées après un séparateur pour une meilleure lisibilité :

```python
container.add_item(ui.Separator())

gallery = ui.MediaGallery()
gallery.add_item(media=image_url)

container.add_item(gallery)
```

---

## Notes

- Seules les URL sont acceptées — pas de chemins de fichiers locaux.
- Pour afficher un fichier local, utilise `attachment://nom_du_fichier.png` avec un `discord.File`.
- Jusqu’à **10 images** par galerie.