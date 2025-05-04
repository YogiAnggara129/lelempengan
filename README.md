# 🎮 Lelempengan – A Traditional Game from West Java, Reimagined for the Web

**Lelempengan** is a digital adaptation of a traditional board game originating from **West Java, Indonesia**. It’s a minimalist 3x3 grid strategy game that emphasizes pattern recognition, planning, and respect for cultural heritage — all recreated with modern web technologies.

![screenshot](./images/image.png) <!-- Optional: Add a screenshot of the game here -->

---

## 🔹 Game Rules

* Two players: **🔵 Blue** and **🔴 Red**
* Each player has **3 pieces**, placed on opposite sides of a 3x3 board.
* Players take turns moving one piece at a time.
* A piece can **only move along fold-symmetry lines** of the square (not regular up/down/left/right only).
* A piece **cannot jump over or capture** another piece.
* The first player to align all three pieces in a straight line (horizontal, vertical, or diagonal) wins — **except** when that line matches their initial starting formation.

---

## 🛠 Built With

* [SvelteKit](https://kit.svelte.dev/) – Frontend framework for building fast web apps
* [Tailwind CSS](https://tailwindcss.com/) – Utility-first CSS framework for rapid styling
* [TypeScript](https://www.typescriptlang.org/) – Typed JavaScript for safety and scalability

---

## 🚀 Getting Started

```bash
# Clone this repository
git clone https://github.com/your-username/lelempengan.git
cd lelempengan

# Install dependencies
npm install

# Start the development server
npm run dev
```

Visit `http://localhost:5173` in your browser and enjoy the game!

---

## ✨ Features

* ♟️ Accurate symmetric movement logic based on traditional fold lines
* 🎯 Valid move hints shown automatically when selecting a piece
* 🎨 Responsive and stylized interface with warm traditional tones
* 🧠 Win condition logic with validation to prevent false wins
* 🔁 Restart and turn indicators built in

---

## 💡 Cultural Note

Lelempengan is part of a rich tapestry of traditional games played across Indonesia. By reimagining it for the web, this project aims to preserve and promote local wisdom in a form that younger generations can enjoy and interact with.

> *“Tradition preserved is culture sustained — even in code.”*

---

## 📄 License

This project is open-source and free to use under the [MIT License](LICENSE).

---

## 🙌 Acknowledgements

* Inspired by the description from [Wikibooks: Permainan Lelempengan](https://id.wikibooks.org/wiki/Permainan_Tradisional_%22Catur%22_di_Indonesia/Lelempengan_%28Jawa_Barat%29)
* Made with ❤️ by Yogi Anggara
