# DigitaIAM — бизнес-системы

Лендинг компании **DigitaIAM**, разрабатывающей корпоративный софт для автоматизации бизнес-процессов.

## Продукты

- **Dori.pro** — облачная система управления аптечной сетью: POS-кассы, склад, автозакупки, автораспределение товара по аптекам (FEFO), логистика, отчётность и KPI.
- **Trucks.land** — облачная TMS для грузоперевозок: заказы, живой трекинг, взаиморасчёты с владельцами тягачей и диспетчерами, факторинг, отчётность.

## Технологии

- [Vue 3](https://vuejs.org/) (`<script setup>` SFC)
- [Vite](https://vite.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- GitHub Actions + GitHub Pages (автодеплой)

## Запуск локально

```bash
npm install
npm run dev
```

Сборка и предпросмотр продакшена:

```bash
npm run build
npm run preview
```

## Деплой

Сайт развёрнут на GitHub Pages: <https://digitaiam.com>.

При каждом `git push` в ветку `main` GitHub Actions собирает проект (`npm run build`) и публикует папку `dist` автоматически (`.github/workflows/deploy.yml`).

## Форма заявки

Заявки с сайта отправляются через [Formspree](https://formspree.io/) на почту компании.
