# Робота з форком Marlin: клонування, upstream та оновлення

Коротка інструкція, як склонити свій форк, підключити офіційний репо (upstream) і затягувати оновлення з нього в свій репо на GitHub.

---

## 1. Склонити свій форк

Підстав **свій** логін і назву репо (якщо відрізняється від `Marlin`):

```bash
git clone https://github.com/YOUR_USERNAME/Marlin.git
cd Marlin
```

Або через SSH:

```bash
git clone git@github.com:YOUR_USERNAME/Marlin.git
cd Marlin
```

Після клонування `origin` вказує на **твій** форк на GitHub.

---

## 2. Додати upstream (офіційний репо Marlin)

Один раз налаштуй віддалений репо офіційного Marlin:

```bash
git remote add upstream https://github.com/MarlinFirmware/Marlin.git
```

Перевірити віддалені репо:

```bash
git remote -v
```

Має бути щось на кшталт:

- `origin` — твій форк (fetch/push)
- `upstream` — MarlinFirmware/Marlin (fetch)

---

## 3. Затягнути оновлення з офіційного репо

Оновити посилання та гілки з upstream:

```bash
git fetch upstream
```

Влити оновлення з потрібної гілки (наприклад `bugfix-2.1.x`) у поточну гілку:

```bash
git checkout bugfix-2.1.x
git merge upstream/bugfix-2.1.x
```

Якщо виникнуть конфлікти — вирішити їх у файлах, потім:

```bash
git add .
git commit -m "Merge upstream/bugfix-2.1.x"
```

---

## 4. Відправити оновлення у свій форк на GitHub

Після успішного merge:

```bash
git push origin bugfix-2.1.x
```

Якщо гілка вперше пушиться:

```bash
git push -u origin bugfix-2.1.x
```

Далі можна просто `git push`.

---

## Швидкий цикл «оновлення з офіційного репо»

Коли upstream вже доданий, достатньо:

```bash
git checkout bugfix-2.1.x
git fetch upstream
git merge upstream/bugfix-2.1.x
# вирішити конфлікти, якщо є
git push origin bugfix-2.1.x
```

---

## Корисні команди

| Дія | Команда |
|-----|--------|
| Список віддалених репо | `git remote -v` |
| Оновити тільки свій форк | `git fetch origin` |
| Оновити тільки upstream | `git fetch upstream` |
| Подивитися гілки upstream | `git branch -r` |
| Поточна гілка | `git branch` або `git status` |

---

## Якщо upstream вже існує

Якщо раніше додавали інший upstream і хочеш змінити URL:

```bash
git remote set-url upstream https://github.com/MarlinFirmware/Marlin.git
```

Якщо потрібно видалити старий upstream і додати заново:

```bash
git remote remove upstream
git remote add upstream https://github.com/MarlinFirmware/Marlin.git
```

---

**Підсумок:** клонуєш **свій** форк → додаєш **upstream** = офіційний Marlin → періодично робиш `fetch upstream` + `merge upstream/bugfix-2.1.x` → пушиш у `origin`, щоб оновлення з’явилися в твоєму репо на GitHub.
