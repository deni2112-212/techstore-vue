# TechStore — магазин комп'ютерної техніки

Навчальний проєкт на **Vue.js 3 + Vite**. У каталозі є пошук, фільтр за категоріями та інтерактивний кошик.

## Можливості

- 8 товарів із назвою, ціною, зображенням і категорією.
- Каталог завантажується через `fetch()` із JSON-файлу.
- Пошук і фільтр реалізовані через `computed`.
- Додавання товарів до кошика. Якщо товар уже є — збільшується його кількість.
- Зміна кількості кнопками `+` та `−`, видалення товару.
- Лічильник і загальна сума автоматично перераховуються.
- Кошик зберігається в `localStorage` за допомогою `watch`.
- Компоненти `ProductCard.vue` та `CartItem.vue` використовують props та emit.
- Адаптивна верстка для телефонів і комп'ютерів.

## Як запустити у Visual Studio Code

1. Розпакуй ZIP-архів.
2. Відкрий папку `techstore-vue` у Visual Studio Code.
3. Відкрий термінал: **Terminal → New Terminal**.
4. Переконайся, що встановлений Node.js (рекомендовано LTS).
5. Виконай команди:

```bash
npm install
npm run dev
```

6. Відкрий адресу, яку покаже Vite (зазвичай `http://localhost:5173`).

> Важливо: каталог завантажується через `fetch`, тому запускай сайт через `npm run dev`, а не відкривай `index.html` подвійним кліком.

## Як перевірити кошик

1. Натисни «До кошика» на будь-якому товарі.
2. Додай ще один або кілька різних товарів.
3. Якщо додати той самий товар повторно, його кількість збільшиться.
4. У кошику натискай `+` або `−` — сума зміниться автоматично.
5. Онови сторінку браузера — товари залишаться в кошику.

## Структура проєкту

```text
techstore-vue/
├── index.html
├── package.json
├── README.md
├── vite.config.js
├── .gitignore
├── .github/
│   └── workflows/
│       └── deploy.yml
├── public/
│   └── products.json
└── src/
    ├── App.vue
    ├── main.js
    ├── style.css
    └── components/
        ├── ProductCard.vue
        └── CartItem.vue
```

## Публікація на GitHub

1. Створи новий порожній репозиторій на GitHub, наприклад `techstore-vue`.
2. У терміналі VS Code, перебуваючи в папці проєкту, виконай:

```bash
git init
git add .
git commit -m "Create TechStore Vue project"
git branch -M main
git remote add origin https://github.com/ТВІЙ-ЛОГІН/techstore-vue.git
git push -u origin main
```

Заміни `ТВІЙ-ЛОГІН` на свій GitHub username.

### Публікація сайту через GitHub Pages

Для Vite на GitHub Pages потрібно налаштувати `base` у `vite.config.js` відповідно до назви репозиторію, а потім додати workflow для GitHub Actions. Для здачі вихідного коду достатньо завантажити репозиторій за інструкцією вище.

## Публікація на GitHub Pages

У проєкті вже налаштовано `vite.config.js` для репозиторію `techstore-vue` та GitHub Actions workflow у `.github/workflows/deploy.yml`. Після завантаження файлів у гілку `main` GitHub Actions автоматично збере проєкт (`npm ci`, `npm run build`) і опублікує папку `dist`. У репозиторії відкрий **Settings → Pages** та встанови **Build and deployment → Source: GitHub Actions**. Перевір результат у вкладці **Actions**.
