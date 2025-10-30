
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
