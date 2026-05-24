# 🛹 Skate Legend — New Tokyo

> Concrétisation — L1 MI-Informatique · 2025–2026

---

## Description

**Skate Legend** est un jeu de cartes compétitif pour 2 à 4 joueurs.
Chaque joueur incarne un skateur légendaire à New Tokyo et enchaîne des
figures pour accumuler des **Points de Prestige**.
Le danger : chuter en accumulant 3 Skate Cassé ou 3 cartes de même couleur.

---

## Structure du projet

```
SkateLegend/
├── cartes.py     — 120 cartes Figure + 10 Légendaires (dictionnaires)
├── moteur.py     — logique du jeu : tours, manches, chute, validation
├── strategie.py  — interface JoueurStrategie + 3 stratégies
├── main.py       — menu, affichage, partie interactive
└── README.md     — ce fichier
```

---

## Lancer le jeu

**Prérequis :** Python 3.10 ou supérieur

```bash
# Vérifier la version Python
python --version

# Lancer le jeu
python main.py

# Ou directement
python main.py --jouer    # partie interactive
python main.py --demo     # démo IA vs IA
```

---

## Règles du jeu

### À chaque tour, le joueur choisit :

| Action | Description |
|--------|-------------|
| **Faire une figure** | Tirer une carte depuis une pioche ou sa main |
| **S'arrêter** | Valider l'enchaînement et marquer les points |

### Conditions de chute 💥

- **3 symboles Skate Cassé** dans l'enchaînement
- **3 cartes de même couleur** dans l'enchaînement

> Le **Jeton Casque** neutralise une couleur.
> Chaque jeton non utilisé vaut **+2 pts** en fin de partie.

### Les 3 effets de carte

| Effet | Description |
|-------|-------------|
| `+CARTE` | Piocher une carte supplémentaire |
| `RÉVÈLE` | Retourner la première carte d'une pioche |
| `REJOUE` | Rejouer immédiatement une figure |

### Cartes Légendaires

Le dernier joueur debout remporte une Carte Légendaire.
Les points ne comptent que si la condition est remplie.

| Carte | Pts | Condition |
|-------|-----|-----------|
| 360° | 4 | 2 cartes ROUGES |
| Casper Slide | 3 | 2 cartes même couleur |
| 180° | 3 | 4 cartes |
| 720° | 5 | 5 cartes |
| 900° | 4 | 1 VERTE + 1 JAUNE |
| 1080° | 6 | 3 couleurs différentes |
| Airwalk Grab | 4 | 2 Skate Cassé |
| Perfect Landing | 5 | Aucune condition |

---

## Architecture

### Les 4 fichiers

**`cartes.py`** — Données pures, aucune logique.
Chaque carte est un dictionnaire avec les clés :
`nom`, `couleur`, `récompense`, `piocher`, `rejouer`, `visible`, `cassé`, `condition`

**`moteur.py`** — Toutes les règles du jeu sous forme de fonctions :
- `nouvelle_partie(nb_joueurs)` — initialise la partie
- `jouer_tour(etat, strategie)` — fait jouer le joueur courant
- `manche_terminee(etat)` — vérifie si la manche est finie
- `partie_terminee(etat)` — vérifie si la partie est finie
- `scores_finaux(etat)` — retourne les scores

**`strategie.py`** — Interface imposée + 3 stratégies :
- `JoueurStrategie` — classe de base (interface)
- `StrategieAleatoire` — joue au hasard
- `StrategieRisque` — évalue le risque avant de jouer
- `StrategieAgressive` — continue presque toujours

**`main.py`** — Point d'entrée, affichage terminal, saisie clavier.

### Interface JoueurStrategie

```python
class JoueurStrategie:
    def continuer_jouer(self, votre_joueur_id, etat_partie) -> bool
    def jouer_carte(self, votre_joueur_id, etat_partie) -> (source, index)
    def piocher_carte(self, votre_joueur_id, etat_partie) -> (source, index)
    def jouer_casque(self, votre_joueur_id, etat_partie) -> bool
    def notifier_choix(self, joueur_id, continuer, carte_jouee, etat_partie)
    def notifier_pioche(self, joueur_id, carte_visible_prise)
```

### Format etat_partie

```python
{
    "joueurs": [{"id": 0, "en_jeu": True, "points": 6,
                 "casques_reserve": 1, "casque_actuel": None,
                 "enchainement_actuel": [...]}],
    "id_manche": 2,
    "cartes_legendaires": [...],
    "pioche": [carte_ou_None, carte_ou_None],
    "moi": { ...joueur courant avec main... }
}
```

---

## Concepts utilisés

| Concept | Utilisation |
|---------|-------------|
| Dictionnaires | Cartes, joueurs, état de la partie |
| Listes | Pioches, mains, enchaînements |
| Fonctions | Toute la logique du moteur |
| Classes / Héritage | Interface `JoueurStrategie` |
| Boucles `while` | Boucle de jeu principale |
| Conditions `if/elif` | Détection de chute, effets |

---

## Auteurs

| Nom | Rôle |
|-----|------|
| Denis | Développeur |
| Prénom Nom | Développeur |

---

## Références

- Règles officielles : *Skate Legend — Règles du jeu* (édition française)
- Documentation Python : [docs.python.org](https://docs.python.org)
- Concrétisation — L1 MI-Informatique, Université d'Angers
