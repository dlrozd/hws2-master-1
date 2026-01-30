# Деплой React-приложения на GitHub Pages с Yarn

## Шаг 1: Подготовка проекта (один раз)

### 1.1 Установка gh-pages
```bash
yarn add -D gh-pages
```

### 1.2 Настройка package.json
Откройте `package.json` и добавьте/измените:

```json
{
  "name": "your-project-name",
  "homepage": "https://<username>.github.io/<repo-name>",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "predeploy": "yarn build",
    "deploy": "gh-pages -d build"
  }
}
```

**ВАЖНО:**
- `<username>` — ваш GitHub username
- `<repo-name>` — название репозитория
- Пример: `"homepage": "https://dlrozd.github.io/hws2-master-1"`

## Шаг 2: Инициализация Git (только первый раз)

```bash
# Инициализация репозитория
git init

# Добавление файлов
git add .

# Первый коммит
git commit -m "Initial commit"

# Переименование ветки в main
git branch -M main

# Подключение удаленного репозитория
git remote add origin https://github.com/<username>/<repo-name>.git

# Первый push
git push -u origin main
```

## Шаг 3: Первый деплой

```bash
# Очистка кэша (на всякий случай)
rm -rf build node_modules/.cache

# Деплой
yarn deploy
```

Команда `yarn deploy` автоматически:
1. Выполнит `yarn build` (через `predeploy`)
2. Создаст ветку `gh-pages`
3. Запушит собранные файлы из папки `build` в ветку `gh-pages`

## Шаг 4: Настройка GitHub Pages

1. Откройте репозиторий на GitHub
2. Перейдите в **Settings** → **Pages**
3. В разделе **Source**:
   - Выберите ветку `gh-pages`
   - Папка: `/ (root)`
4. Нажмите **Save**

Сайт будет доступен по адресу: `https://<username>.github.io/<repo-name>/`

## Шаг 5: Обновление деплоя (после изменений)

После внесения изменений в код:

```bash
# Просто запустите деплой
yarn deploy
```

**Опционально:** если хотите сохранить изменения в основной ветке:

```bash
# Сохранение изменений в git
git add .
git commit -m "Update: описание изменений"
git push

# Деплой обновлений
yarn deploy
```

## Частые проблемы и решения

### Белая страница / 404 ошибки

**Причина:** Неправильный `homepage` в `package.json`

**Решение:**
1. Проверьте `homepage` в `package.json`:
   ```json
   "homepage": "https://<username>.github.io/<repo-name>"
   ```
   НЕ `https://github.com/<username>/<repo-name>`!

2. Очистите кэш и пересоберите:
   ```bash
   rm -rf build node_modules/.cache
   yarn deploy
   ```

3. Очистите кэш браузера (Ctrl+Shift+R / Cmd+Shift+R) или откройте в режиме инкогнито

### Изменения не отображаются

1. Подождите 1-2 минуты после деплоя
2. Очистите кэш браузера
3. Проверьте что деплой выполнился успешно (должно быть сообщение "Published")

### Ветка gh-pages не создается

Проверьте наличие ветки:
```bash
git fetch --all
git branch -a
```

Должна быть `remotes/origin/gh-pages`

## Структура команд

```bash
# ПЕРВЫЙ РАЗ (полная настройка)
yarn add -D gh-pages                          # Установка пакета
# настроить package.json (homepage + scripts)
git init && git add . && git commit -m "Init" # Git настройка
git remote add origin <url>                   # Подключение GitHub
git push -u origin main                       # Первый push
yarn deploy                                   # Первый деплой

# ПОСЛЕДУЮЩИЕ РАЗЫ (только обновления)
yarn deploy                                   # Только это!
```

## Проверка результата

После успешного деплоя:
1. Откройте `https://<username>.github.io/<repo-name>/`
2. Проверьте консоль браузера (F12) на ошибки
3. Если белая страница — проверьте `homepage` и очистите кэш

## Полезные команды

```bash
# Проверка текущего remote
git remote -v

# Проверка статуса
git status

# Проверка веток
git branch -a

# Обновление информации о ветках
git fetch --all

# Очистка кэша и билда
rm -rf build node_modules/.cache
```
