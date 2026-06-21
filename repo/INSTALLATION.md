# Guide d'installation et d'utilisation

Ce notebook fonctionne **partout** : Google Colab, PC/portable avec GPU NVIDIA, et Mac Apple
Silicon. Il détecte automatiquement le matériel. Choisis la section qui correspond à ta machine.

---

## Sommaire

- [A. Google Colab (le plus simple, recommandé)](#a-google-colab)
- [B. PC/Portable avec GPU NVIDIA (Windows ou Linux)](#b-pc-avec-gpu-nvidia)
- [C. PC/Portable sans GPU (CPU seulement)](#c-pc-sans-gpu)
- [D. Mac Apple Silicon (M1 / M2 / M3)](#d-mac-apple-silicon)
- [Vérifier que tout marche](#vérifier-que-tout-marche)
- [Problèmes fréquents](#problèmes-fréquents)

---

## A. Google Colab

**Aucune installation.** PyTorch, pandas et matplotlib sont déjà présents.

1. Va sur https://colab.research.google.com/
2. `Fichier` → `Importer le notebook` → sélectionne `TP_benchmark_CPU_vs_GPU.ipynb`.
3. **Active le GPU** : `Exécution` → `Modifier le type d'exécution` → `Accélérateur matériel : GPU` → `Enregistrer`.
4. `Exécution` → `Tout exécuter`.

> Sans l'étape 3, Colab te donne un CPU seul : le notebook tourne quand même mais ne mesure pas le GPU.

---

## B. PC avec GPU NVIDIA

Valable **Windows** et **Linux**. Il faut une carte **NVIDIA** (GeForce, RTX, Quadro, etc.).

### 1. Vérifier le pilote NVIDIA

Ouvre un terminal (PowerShell sur Windows, Terminal sur Linux) et tape :

```bash
nvidia-smi
```

Si une table s'affiche avec ta carte et une version CUDA, le pilote est bon. Sinon, installe le
pilote depuis https://www.nvidia.com/Download/index.aspx

### 2. Créer un environnement Python isolé (recommandé)

```bash
python -m venv venv
# Windows :
venv\Scripts\activate
# Linux :
source venv/bin/activate
```

### 3. Installer PyTorch avec support CUDA

**Important :** ne fais PAS un simple `pip install torch` — la version par défaut peut être
CPU-only. Va sur https://pytorch.org/get-started/locally/ , sélectionne ton OS + « CUDA », et
copie la commande proposée. Elle ressemble à ceci (vérifie la version CUDA sur le site) :

```bash
pip install torch --index-url https://download.pytorch.org/whl/cu121
```

Puis les deux autres bibliothèques :

```bash
pip install pandas matplotlib jupyter
```

### 4. Lancer le notebook

```bash
jupyter notebook TP_benchmark_CPU_vs_GPU.ipynb
```

À l'exécution, la cellule 0 doit afficher `Accélérateur : cuda` et le nom de ta carte.

---

## C. PC sans GPU

Le notebook fonctionne, mais ne mesurera que le CPU (la partie GPU est automatiquement ignorée).

```bash
python -m venv venv
# Windows : venv\Scripts\activate   |   Linux/Mac : source venv/bin/activate
pip install torch pandas matplotlib jupyter
jupyter notebook TP_benchmark_CPU_vs_GPU.ipynb
```

La cellule 0 affichera `aucun (CPU seul)`. C'est normal et ce n'est pas une erreur.

---

## D. Mac Apple Silicon

Les Mac M1/M2/M3 **n'ont pas de GPU NVIDIA**, donc **pas de CUDA**. PyTorch utilise à la place
**MPS** (Metal Performance Shaders), le GPU intégré d'Apple. Le notebook le détecte tout seul.

```bash
python3 -m venv venv
source venv/bin/activate
pip install torch pandas matplotlib jupyter
jupyter notebook TP_benchmark_CPU_vs_GPU.ipynb
```

La cellule 0 doit afficher `Accélérateur : mps`. Le notebook compare alors CPU vs GPU Apple.

> Note : MPS est généralement plus lent qu'un vrai GPU NVIDIA, et certaines opérations peuvent
> retomber sur le CPU. Les résultats restent valables pour la comparaison, mais les speedups
> seront plus modestes qu'avec une carte NVIDIA ou sur Colab.

---

## Vérifier que tout marche

Avant de lancer tout le notebook, exécute juste la **cellule 0**. Elle doit afficher l'un de :

| Affichage | Signification |
|---|---|
| `Accélérateur : cuda` + nom de carte | GPU NVIDIA détecté — parfait |
| `Accélérateur : mps` | Mac Apple Silicon — parfait |
| `aucun (CPU seul)` | Pas de GPU — le notebook tourne en CPU |

---

## Problèmes fréquents

**« CUDA available : False » alors que j'ai une carte NVIDIA**
Tu as probablement installé la version CPU de PyTorch. Désinstalle puis réinstalle avec la commande
CUDA du site pytorch.org : `pip uninstall torch` puis l'étape B.3.

**Sur Colab, le GPU n'est pas détecté**
Tu as oublié `Exécution → Modifier le type d'exécution → GPU`. Refais-le et relance la cellule 0.

**« Out of memory » sur la taille 100 000 000**
Ton GPU a peu de mémoire. Dans la cellule des tailles, retire `100_000_000` de la liste `TAILLES`,
ou descends jusqu'à `10_000_000`.

**Le notebook est très lent sur la dernière taille**
C'est normal : 100M d'éléments, c'est lourd, surtout sur CPU. Compte quelques minutes.

**`nvidia-smi` n'est pas reconnu (Windows)**
Le pilote NVIDIA n'est pas installé ou pas à jour. Va sur le site NVIDIA Download.
