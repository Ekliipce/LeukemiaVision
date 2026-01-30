# Correction du problème de métadonnées des widgets Jupyter

## Problème rencontré

Lors de la tentative de rendu ou de conversion des notebooks Jupyter, l'erreur suivante apparaissait :

```
Invalid Notebook
There was an error rendering your Notebook: the 'state' key is missing from 'metadata.widgets'.
Add 'state' to each, or remove 'metadata.widgets'.
Using nbformat v5.10.4 and nbconvert v7.16.6
```

Cette erreur se produisait spécifiquement avec :
- `LeukomiaVision_preprocess&baseline.ipynb`
- `LeukomiaVision_trainCNMC.ipynb`

## Cause du problème

Les widgets Jupyter (notamment `tqdm.notebook`) créent des métadonnées dans le notebook sous la clé `metadata.widgets`. La structure attendue par nbconvert v7+ est :

```json
{
  "metadata": {
    "widgets": {
      "application/vnd.jupyter.widget-state+json": {
        "state": {
          "widget-id-1": { ... },
          "widget-id-2": { ... }
        },
        "version_major": 2,
        "version_minor": 0
      }
    }
  }
}
```

Cependant, les notebooks avaient une structure incorrecte où les widgets étaient directement placés sans la clé intermédiaire `state` :

```json
{
  "metadata": {
    "widgets": {
      "application/vnd.jupyter.widget-state+json": {
        "widget-id-1": { ... },
        "widget-id-2": { ... }
      }
    }
  }
}
```

## Solution appliquée

Un script Python a été utilisé pour restructurer les métadonnées :

```python
import json

notebook_path = "path/to/notebook.ipynb"

with open(notebook_path, 'r', encoding='utf-8') as f:
    notebook = json.load(f)

widgets = notebook['metadata']['widgets']

if 'application/vnd.jupyter.widget-state+json' in widgets:
    widget_state = widgets['application/vnd.jupyter.widget-state+json']

    # Restructuration au bon format
    widgets['application/vnd.jupyter.widget-state+json'] = {
        'state': widget_state,
        'version_major': 2,
        'version_minor': 0
    }

    # Sauvegarde
    with open(notebook_path, 'w', encoding='utf-8') as f:
        json.dump(notebook, f, indent=1, ensure_ascii=False)
```

## Vérification

Pour vérifier qu'un notebook est correctement formaté :

```bash
# Tester la conversion
jupyter nbconvert --to html --stdout notebook.ipynb > /dev/null

# Vérifier la structure des métadonnées
cat notebook.ipynb | jq '.metadata.widgets."application/vnd.jupyter.widget-state+json" | keys'
```

Le résultat attendu doit inclure les clés : `["state", "version_major", "version_minor"]`

## Prévention future

Pour éviter ce problème à l'avenir :

1. Toujours utiliser des versions compatibles de Jupyter et nbconvert
2. Si vous partagez des notebooks avec des widgets, considérer l'utilisation de `jupyter nbconvert --clear-output` pour nettoyer les métadonnées de widgets avant le commit
3. Alternativement, ajouter un hook pre-commit pour valider la structure des notebooks

## Références

- [Jupyter Widgets Documentation](https://ipywidgets.readthedocs.io/)
- [nbconvert Documentation](https://nbconvert.readthedocs.io/)
- Format nbformat : [Jupyter Notebook Format](https://nbformat.readthedocs.io/)

---

**Date de correction** : 2026-01-30
**Notebooks corrigés** : `LeukomiaVision_preprocess&baseline.ipynb`, `LeukomiaVision_trainCNMC.ipynb`
