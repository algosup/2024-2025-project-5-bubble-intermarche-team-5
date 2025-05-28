# 📝 Meeting Report - Wine & Cheese Pairing Web App

**Date:** May 17, 2025  
**Timw:** 9:30 AM
**Duration:** 39 minutes  
**Location:** FaceTime call (Remote)

---

## 🎯 Meeting Objective

The objective of this meeting was to discuss the client’s feedback on the mockups, clarify expectations around the recommendation algorithm, and gather domain expertise from both the wine and cheese departments to define filtering logic for the application.

--- 

**Attendees:**  
- Laurent, Wine Department Manager
- Céline, Cheese Department Manager
- Célia, Client contact, Intermarché Saint-Rémy-de-Provence
- Chrys, Store Intern, Intermarché Saint-Rémy-de-Provence
- Additional members from Team 8

**Project Team:**
- Ian LAURENT, Project Manager
- Mathias DELILLE, Program Manager
- Benoit DE KEYN, Quality Assurance
- Paul NOWAK, Software Developer

---

## ✅ Summary of Feedback

### 🔸 General Impressions:
- Positive reception to the mockups and the overall user experience approach.
- The method of **proposing wine based on customer preferences** was well appreciated.

---

## 📌 Key Discussion Points

### 🍷 Wine Recommendation Logic:
- Laurent explained how wine is typically recommended depending on the **type of dish** (e.g., red meat with full-bodied red wine).
- The team agreed to implement a system in which the **database will link wine types with compatible food types** (e.g., "viande rouge" with robust reds).
- Further **refinement based on wine appellations** was discussed - e.g.:
  - Burgundy wines tend to have **higher alcohol content** and **stronger character**.
  - Wines from Montlouis tend to be **fruitier and lighter**.
- Customers should also be able to **filter suggestions by price**, as it is a key decision factor for Intermarché customers.

### 🧀 Cheese Pairing Logic:
- Céline emphasized the importance of **cheese strength**:
  - A **fresh goat cheese** should be paired with a **white wine**, not a red.
  - **Stronger cheeses** are better suited for **red wines**.
- She agreed to provide a **quick decision tree (mind flow chart)** showing how she guides customers through cheese pairings, which will serve as a base for our algorithm logic.

### 🥂 Occasion-Based Filtering:
- All participants liked the idea (seen in a mockup) of asking the customer about the **occasion** (e.g., sunny day, dinner, aperitif).
- Example: a customer is less likely to choose a **red wine under the sun**, and more likely to go for a **rosé**.
- This approach was validated as a helpful way to personalize recommendations.

---

## 📌 Action Items

| Task | Responsible | Deadline |
|------|-------------|----------|
| Provide cheese pairing decision flowchart | Céline | ASAP |
| Structure wine/food pairings in the database | Dev Team | In progress |
| Implement filtering logic by **price**, **occasion**, and **food type** | Dev Team | In upcoming sprint |
| Refine recommendation flow based on wine **origin and character** | Dev Team + Wine Expert | Next iteration |

---

## 5. 📝 Notes

- The meeting provided **clear, practical insights** directly from store expertise that will significantly improve the accuracy and usability of the app’s algorithm.
- The product strategy is now better aligned with actual **in-store recommendation logic**.
- The client expressed **high engagement and approval** of the approach.

---

## 📅 Next Meeting

**Proposed Date:** Post 28/05/2025, after first development phase, after client feedback on the V1 of the product
**Expected Agenda:** Review first version of the product, finalize technical specifications and conception of the V1 based on validated scope.