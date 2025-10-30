# ---------------------------------------------------------------
# Art Investment Advisor
# Application d’aide à la décision pour l’investissement en art contemporain
# Réalisée avec Streamlit
# ---------------------------------------------------------------

import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

# ---------------------------------------------------------------
# 1. Configuration générale
# ---------------------------------------------------------------
st.set_page_config(page_title="Art Investment Advisor")

st.title("Art Investment Advisor")
st.write("""
Cette application aide un investisseur à évaluer la pertinence d’un achat d’œuvre d’art contemporain.
Le score final est basé sur des critères objectifs concernant l'œuvre et l'artiste.
""")

# ---------------------------------------------------------------
# 2. Fonctions de calcul
# ---------------------------------------------------------------

def calcul_score_oeuvre(nature, taille, unicite, historique):
    """Calcule le score de l'œuvre (pondéré sur 20)."""
    notes = {
        "nature": {"Peinture/Sculpture": 18, "Photo/Vidéo": 12, "Performance/Protocole": 8},
        "taille": {"Monumentale": 18, "Moyenne": 12, "Petite": 8},
        "unicite": {"Pièce unique": 20, "Série limitée": 14, "Grande série": 8},
        "historique": {"Emblématique": 20, "Charnière": 14, "Commande": 8}
    }
    score = (
        notes["nature"][nature] * 0.25 +
        notes["taille"][taille] * 0.20 +
        notes["unicite"][unicite] * 0.40 +
        notes["historique"][historique] * 0.15
    )
    return score


def calcul_score_artiste(carriere, nb_expos, type_expo, lieu, prix, representation, prix_marche):
    """Calcule le score de l'artiste (moyenne sur 20, avec bonus/malus)."""
    notes = {
        "carriere": {"Confirmé (>20 ans)": 20, "Intermédiaire (5–15 ans)": 14, "Débutant (<5 ans)": 8},
        "type_expo": {"Solo show": 20, "Group show": 14},
        "lieu": {"Musée/Institution": 20, "Galerie/Foire": 14, "Indépendant": 10},
        "prix": {"Lauréat": 20, "Finaliste": 14, "Aucun": 8},
        "representation": {"Blue chip": 20, "Galerie locale": 14, "Indépendant": 8},
        "prix_marche": {"Élevé": 16, "Moyen": 14, "Bas": 12}
    }

    if nb_expos < 5 and carriere == "Confirmé (>20 ans)":
        expos_score = 16
    elif nb_expos < 5 and carriere == "Débutant (<5 ans)":
        expos_score = 8
    elif nb_expos > 15:
        expos_score = 20
    else:
        expos_score = 14

    base = (
        notes["carriere"][carriere] +
        expos_score +
        notes["type_expo"][type_expo] +
        notes["lieu"][lieu] +
        notes["prix"][prix] +
        notes["representation"][representation]
    ) / 6

    bonus = 0
    if prix_marche == "Bas" and base > 15:
        bonus = +2
    elif prix_marche == "Élevé" and base < 12:
        bonus = -2

    return min(base + bonus, 20)


def calcul_final(score_oeuvre, score_artiste):
    """Combine les scores œuvre et artiste selon pondération (25/75)."""
    total = (score_oeuvre * 0.25) + (score_artiste * 0.75)
    pourcentage = min(round((total / 20) * 100, 1), 100)
    return pourcentage

# ---------------------------------------------------------------
# 3. Interface utilisateur
# ---------------------------------------------------------------

st.header("Critères liés à l'œuvre")
col1, col2 = st.columns(2)

with col1:
    nature = st.selectbox("Nature de l'œuvre", ["Peinture/Sculpture", "Photo/Vidéo", "Performance/Protocole"])
    taille = st.selectbox("Taille", ["Monumentale", "Moyenne", "Petite"])
with col2:
    unicite = st.selectbox("Unicité", ["Pièce unique", "Série limitée", "Grande série"])
    historique = st.selectbox("Historique", ["Emblématique", "Charnière", "Commande"])

score_oeuvre = calcul_score_oeuvre(nature, taille, unicite, historique)

st.header("Critères liés à l'artiste")

col3, col4 = st.columns(2)
with col3:
    carriere = st.selectbox("Carrière", ["Confirmé (>20 ans)", "Intermédiaire (5–15 ans)", "Débutant (<5 ans)"])
    nb_expos = st.slider("Nombre d’expositions récentes (3 dernières années)", 0, 30, 10)
    type_expo = st.selectbox("Type d’expositions", ["Solo show", "Group show"])
with col4:
    lieu = st.selectbox("Lieu d’exposition principal", ["Musée/Institution", "Galerie/Foire", "Indépendant"])
    prix = st.selectbox("Prix et distinctions", ["Lauréat", "Finaliste", "Aucun"])
    representation = st.selectbox("Représentation", ["Blue chip", "Galerie locale", "Indépendant"])
    prix_marche = st.selectbox("Prix du marché (3 dernières années)", ["Élevé", "Moyen", "Bas"])

score_artiste = calcul_score_artiste(carriere, nb_expos, type_expo, lieu, prix, representation, prix_marche)

# ---------------------------------------------------------------
# 4. Résultat final
# ---------------------------------------------------------------
pourcentage = calcul_final(score_oeuvre, score_artiste)

st.subheader("Résultat de l'analyse")
st.metric(label="Indice de recommandation", value=f"{pourcentage}%")

if pourcentage > 80:
    st.success("Excellente opportunité : fort potentiel de valorisation à court terme.")
elif pourcentage > 60:
    st.info("Bon investissement : à confirmer selon le marché.")
else:
    st.warning("Risque élevé ou visibilité limitée de l'artiste.")

# ---------------------------------------------------------------
# 5. Visualisation graphique
# ---------------------------------------------------------------
st.header("Visualisation des scores")

data = pd.DataFrame({
    'Catégorie': ['Œuvre', 'Artiste'],
    'Score': [score_oeuvre, score_artiste]
})

fig, ax = plt.subplots()
ax.bar(data['Catégorie'], data['Score'], color=['#7D3C98', '#2471A3'])
ax.set_ylim(0, 20)
ax.set_ylabel("Score sur 20")
ax.set_title("Comparatif des scores")
st.pyplot(fig)
# Art-Investment-Avisor
# 🎨 Art Investment Advisor

**Art Investment Advisor** is a Streamlit-based web application designed to help investors evaluate the potential of purchasing a contemporary artwork. It combines objective data-driven criteria on both the **artwork** and the **artist** to produce a recommendation score.

---

## 🚀 Features

- Evaluate artworks based on:
  - Nature, size, uniqueness, and historical importance.
- Evaluate artists based on:
  - Career length, exhibitions, prizes, representation, and market value.
- Weighted scoring system:
  - **Artist: 75%**
  - **Artwork: 25%**
- Dynamic visual output using **Matplotlib**.
- Simple, clean **Streamlit interface**.

---

## 🧮 Methodology

### Artwork Scoring (out of 20)
| Criterion | Weight | Example Factors |
|------------|---------|----------------|
| Nature | 25% | Painting, Sculpture, Photo, Performance |
| Size | 20% | Monumental, Medium, Small |
| Uniqueness | 40% | Unique piece, Limited series, Large series |
| Historical Value | 15% | Emblematic, Transitional, Commission |

### Artist Scoring (out of 20)
| Criterion | Weight | Example Factors |
|------------|---------|----------------|
| Career Stage | 1/6 | Confirmed, Intermediate, Emerging |
| Exhibitions | 1/6 | Number and type of recent shows |
| Venue Quality | 1/6 | Museum, Fair, Independent |
| Prizes | 1/6 | Laureate, Finalist, None |
| Representation | 1/6 | Blue chip gallery, Local, Independent |
| Market Price | bonus/malus | Low (bonus) or High (malus) |

**Final recommendation index:**

`Final Score = (Artwork Score × 0.25) + (Artist Score × 0.75)`

---

## 🖥️ Example Output

The app displays:
- A global **Recommendation Index** (%)
- A **bar chart** comparing artwork and artist scores
- A color-coded message:
  - 🟢 Excellent opportunity
  - 🔵 Good investment
  - 🟠 Risky investment

---

## 🧰 Tech Stack

- **Python 3.13**
- **Streamlit**
- **Pandas**
- **Matplotlib**

---

## ⚙️ Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/art-investment-advisor.git
   cd art-investment-advisor
   ```

2. Install dependencies:
   ```bash
   python3 -m pip install -r requirements.txt
   ```

3. Run the app:
   ```bash
   python3 -m streamlit run ArtInvestmentAdvisor.py
   ```

---

## 📦 requirements.txt
```
streamlit
pandas
matplotlib
```

---

## 🧠 Project Context

This project was developed as part of a **Python programming assignment**, focusing on:
- Data modeling & weighted scoring systems
- UI design for non-technical users
- Cultural data analysis through computational thinking

---

## 🎯 Author

**Omar Zeroual**  
📍 Paris, France  
🖥️ Artist | Data Enthusiast | Museum Professional  
📧 omarzeroualpro@gmail.com  

---

## 🪶 License

This project is released under the **MIT License**. You are free to use, modify, and distribute it with attribution.

---
