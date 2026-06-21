# Benchmark CPU vs GPU sur opérations vectorielles

> TP de Systèmes Répartis / Calcul Parallèle — comparaison des performances **CPU** et **GPU**
> sur la **somme**, le **produit scalaire** et la **norme** de vecteurs, de 1 000 à 100 000 000 éléments.

## Binôme

- **Hajar En-Najdy**
- **Mohamed Akrame Kraichy**

---

## Objectif

Mesurer et comparer le temps d'exécution de trois opérations vectorielles sur CPU et GPU, pour
des vecteurs de tailles croissantes (de 1K à 100M d'éléments), afin de mettre en évidence :

1. Le **point de bascule** : la taille à partir de laquelle le GPU devient plus rapide que le CPU.
2. Le **gain (speedup)** apporté par le GPU sur les grands vecteurs.
3. Le coût caché des petits calculs sur GPU (lancement de kernel, transfert mémoire).

## Opérations mesurées

| Opération | Formule | Données lues |
|---|---|---|
| Somme | `Σ aᵢ` | 1 vecteur |
| Produit scalaire | `a·b = Σ aᵢbᵢ` | 2 vecteurs |
| Norme | `‖a‖ = √(Σ aᵢ²)` | 1 vecteur |

## Tailles testées

`1 000` → `10 000` → `100 000` → `1 000 000` → `10 000 000` → `100 000 000`

(échelle logarithmique, ×10 à chaque palier)

---

## Structure du dépôt

```
.
├── README.md                       # ce fichier
├── requirements.txt                # dépendances Python
├── notebooks/
│   └── TP_benchmark_CPU_vs_GPU.ipynb   # le TP exécutable (Google Colab)
├── results/                        # résultats exportés (CSV + graphiques)
├── rapport/                        # rapport technique (PDF)
└── presentation/                   # support de présentation (PPTX)
```

---

## Comment exécuter

Le notebook **détecte automatiquement le matériel** et fonctionne dans tous les cas :

| Environnement | Accélérateur utilisé |
|---|---|
| Google Colab | GPU NVIDIA (CUDA) |
| PC/portable avec carte NVIDIA (Windows/Linux) | GPU NVIDIA (CUDA) |
| Mac Apple Silicon (M1/M2/M3) | GPU Apple (MPS) |
| Machine sans GPU | CPU uniquement |

**Aucune modification du code n'est nécessaire selon la machine** : le même notebook tourne partout.

➡️ **Les instructions d'installation détaillées (Colab, Windows, Linux, Mac, sans GPU) sont dans
[`INSTALLATION.md`](INSTALLATION.md).**

### Démarrage rapide sur Google Colab (recommandé)

1. Ouvrir [Google Colab](https://colab.research.google.com/).
2. `Fichier` → `Importer le notebook` → choisir `notebooks/TP_benchmark_CPU_vs_GPU.ipynb`.
3. Activer le GPU : `Exécution` → `Modifier le type d'exécution` → `Accélérateur matériel : GPU`.
4. `Exécution` → `Tout exécuter`.

> La boucle de mesure complète prend quelques minutes (les vecteurs de 100M d'éléments sont lourds).

---

## Résultats obtenus (Tesla T4, Google Colab)

Mesures réalisées sur GPU **Tesla T4** (PyTorch 2.11 + CUDA 12.8), temps médian en millisecondes.

| Opération | Point de bascule | Speedup max (à 100M) |
|---|---|---|
| Somme | entre 100K et 1M | **22,3×** |
| Produit scalaire | entre 100K et 1M | **19,2×** |
| Norme | entre 100K et 1M | **22,6×** |

**Conclusions principales :**
- Sur les **petits vecteurs (1K–100K)**, le CPU est plus rapide : le coût fixe de lancement du
  kernel GPU (~0,02–0,04 ms) domine le calcul.
- Le **point de bascule** se situe autour de **1 000 000 d'éléments** pour les trois opérations.
- Sur les **grands vecteurs (100M)**, le GPU est **~20× plus rapide** grâce au parallélisme massif.
- Le **produit scalaire** (qui lit 2 vecteurs) est le plus coûteux sur CPU (60 ms à 100M).

Détails complets, tableaux et graphiques dans [`rapport/rapport_technique.pdf`](rapport/rapport_technique.pdf)
et [`results/`](results/).

## Méthodologie de mesure

Trois précautions garantissent des mesures fiables :

1. **Warm-up** — on exécute l'opération quelques fois sans mesurer, pour écarter le surcoût de la
   première exécution (allocation mémoire, initialisations internes).
2. **Synchronisation GPU** — `torch.cuda.synchronize()` force l'attente de la fin réelle du calcul.
   Sans elle, on mesurerait seulement le temps de *lancer* l'ordre, pas de l'*exécuter*.
3. **Médiane sur plusieurs répétitions** — robuste face à une mesure ponctuellement perturbée par
   le système, contrairement à la moyenne.

---

## Technologies

- **Python 3**
- **PyTorch** (calcul CPU/GPU, API unifiée)
- **pandas** (mise en forme des résultats)
- **matplotlib** (graphiques)
