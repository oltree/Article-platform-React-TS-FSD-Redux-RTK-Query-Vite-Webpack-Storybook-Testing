# Article Hub

Платформа для статей с профилями пользователей, комментариями и рейтингами. SPA на React с архитектурой Feature Sliced Design.

---

## Стек технологий

| Категория | Технологии |
|-----------|------------|
| **UI** | React 18, React DOM, React Router v6 |
| **Стили** | Sass/SCSS |
| **Язык** | TypeScript 4.x |
| **Сборка** | Webpack 5, Vite 3 (поддерживаются оба) |
| **Состояние** | Redux Toolkit, React-Redux, RTK Query |
| **i18n** | i18next, react-i18next, i18next-browser-languagedetector, i18next-http-backend |
| **HTTP** | Axios (через RTK Query) |
| **UI-библиотеки** | Headless UI, React Spring, use-gesture |
| **Линтинг** | ESLint (Airbnb, Prettier, TypeScript, i18next, FSD-плагин), Stylelint |
| **Тесты** | Jest, React Testing Library, Cypress (e2e), Loki (скриншоты) |
| **Документация UI** | Storybook 6 (Webpack 5) |
| **Гит-хуки** | Husky, lint-staged |
| **Прочее** | Babel, json-server (mock API) |

---

## Быстрый старт

```bash
npm install
npm run start:dev        # Webpack + mock API
# или
npm run start:dev:vite   # Vite + mock API
```

Приложение: `http://localhost:3000`. Бэкенд (json-server) поднимается вместе с фронтом.

---

## Скрипты

### Запуск

| Команда | Описание |
|---------|----------|
| `npm run start` | Dev-сервер на Webpack (порт 3000) |
| `npm run start:vite` | Dev-сервер на Vite |
| `npm run start:dev` | Webpack + json-server |
| `npm run start:dev:vite` | Vite + json-server |
| `npm run start:dev:server` | Только json-server |

### Сборка

| Команда | Описание |
|---------|----------|
| `npm run build:prod` | Production-сборка (Webpack) |
| `npm run build:dev` | Development-сборка без минификации |

### Линтинг и формат

| Команда | Описание |
|---------|----------|
| `npm run lint:ts` | Проверка TS/TSX (ESLint) |
| `npm run lint:ts:fix` | Автоисправление TS/TSX |
| `npm run lint:scss` | Проверка SCSS (Stylelint) |
| `npm run lint:scss:fix` | Автоисправление SCSS |
| `npm run prettier` | Форматирование кода (Prettier) |

### Тесты

| Команда | Описание |
|---------|----------|
| `npm run test:unit` | Unit-тесты (Jest + React Testing Library) |
| `npm run test:e2e` | E2E (Cypress, интерактивно) |
| `npm run test:ui` | Скриншотные тесты (Loki) |
| `npm run test:ui:ok` | Подтверждение новых скриншотов Loki |
| `npm run test:ui:ci` | Скриншотные тесты для CI |
| `npm run test:ui:report` | Полный отчёт по скриншотам (JSON + HTML) |

### Storybook

| Команда | Описание |
|---------|----------|
| `npm run storybook` | Запуск Storybook (порт 6006) |
| `npm run storybook:build` | Сборка статического Storybook |

### Утилиты

| Команда | Описание |
|---------|----------|
| `npm run generate:slice` | Генерация FSD-слайса |
| `npm run remove-feature` | Удаление feature-флага (аргументы: имя, on/off) |
| `npm run prepare` | Установка Husky (pre-commit) |

---

## Архитектура (Feature Sliced Design)

Проект следует методологии [Feature Sliced Design](https://feature-sliced.design/docs/get-started/tutorial).

Слои (сверху вниз): `app` → `pages` → `widgets` → `features` → `entities` → `shared`.

Используется **eslint-plugin-ulbi-tv-plugin** для контроля FSD:

1. **path-checker** — запрет абсолютных импортов внутри одного модуля.
2. **layer-imports** — проверка зависимостей слоёв (например, widgets не импортируются из features/entities).
3. **public-api-imports** — импорт из других модулей только через public API (есть auto fix).

---

## Структура сущностей и фич

**Entities:** Article, Comment, Country, Currency, Notification, Profile, Rating, User.

**Features:** addCommentForm, articleEditForm, articleRating, articleRecommendationsList, AuthByUsername, avatarDropdown, editableProfileCard, LangSwitcher, notificationButton, profileRating, ThemeSwitcher, UI.

Пути: `src/entities/*`, `src/features/*`.

---

## Конфигурация

- **Webpack:** `./config/build`
- **Vite:** `vite.config.ts`
- **Babel:** `./config/babel`
- **Jest:** `./config/jest`
- **Storybook:** `./config/storybook`
- **Скрипты:** `./scripts` (генерация слайсов, отчёты, remove-feature и др.)

---

## Работа с данными

- **Стейт:** Redux Toolkit. Переиспользуемые сущности нормализуются через EntityAdapter.
- **Запросы:** [RTK Query](src/shared/api/rtkApi.ts).
- **Ленивая подгрузка редюсеров:** [DynamicModuleLoader](src/shared/lib/components/DynamicModuleLoader/DynamicModuleLoader.tsx).

---

## Переводы (i18n)

Используется **i18next**. Переводы лежат в `public/locales`. Документация: [react.i18next.com](https://react.i18next.com/). Для IDE удобно поставить плагин i18next.

---

## Feature flags

Разрешено использовать только хелпер **toggleFeatures** с объектом `{ name, on, off }`. Для удаления фичи — скрипт `npm run remove-feature <имя> <on|off>`.

---

## Тестирование

1. **Unit:** Jest + React Testing Library — `npm run test:unit`.
2. **Скриншоты:** Loki — `npm run test:ui`, подтверждение — `npm run test:ui:ok`.
3. **E2E:** Cypress — `npm run test:e2e`.

Подробнее: [документация по тестам](docs/tests.md).

---

## Storybook

Рядом с компонентом создаётся `*.stories.tsx`. Моки запросов — **storybook-addon-mock**. Запуск: `npm run storybook`. Подробнее: [docs/storybook.md](docs/storybook.md).

Пример стори:

```tsx
import React from 'react';
import { ComponentStory, ComponentMeta } from '@storybook/react';
import { ThemeDecorator } from '@/shared/config/storybook/ThemeDecorator/ThemeDecorator';
import { Button, ButtonSize, ButtonTheme } from './Button';
import { Theme } from '@/shared/const/theme';

export default {
  title: 'shared/Button',
  component: Button,
  argTypes: { backgroundColor: { control: 'color' } },
} as ComponentMeta<typeof Button>;

const Template: ComponentStory<typeof Button> = (args) => <Button {...args} />;

export const Primary = Template.bind({});
Primary.args = { children: 'Text' };

export const Clear = Template.bind({});
Clear.args = { children: 'Text', theme: ButtonTheme.CLEAR };
```

---

## CI и pre-commit

- **GitHub Actions:** `/.github/workflows` — тесты, сборка, Storybook, линтинг.
- **Pre-commit (Husky):** `/.husky` — линтеры и формат по коммитируемым файлам.

---
