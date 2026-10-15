# Визначення версій і політика релізів

## 1. Схема версій (SemVer): MAJOR.MINOR.PATCH
| Частина | Коли змінюється | Приклад |
|---|---|---|
| MAJOR | несумісні зміни (наприклад, інша схема БД без міграції) | 1.1.1 → 2.0.0 |
| MINOR | нова функціональність, сумісна зі старою | 1.0.0 → 1.1.0 (захист адмінки) |
| PATCH | виправлення помилок | 1.1.0 → 1.1.1 (виправлено парсер) |

Повідомлення комітів: `feat(...)` — MINOR, `fix(...)` — PATCH, `feat!(...)` — MAJOR; також `docs`, `chore`, `ci`.

## 2. Git-стратегія (GitFlow)
- `main` — стабільні релізні версії;
- `develop` — інтеграція готових фіч;
- `feature/*` — окрема гілка на кожну фічу;
- `release/*` — підготовка релізу;
- `hotfix/*` — термінові виправлення з `main`.

## 3. Команди Git
```bash
# нова фіча
git checkout -b feature/add-login develop
git add .
git commit -m "feat(auth): add login endpoint"
git push origin feature/add-login

# після код-рев'ю
git checkout develop
git merge --no-ff feature/add-login
git push origin develop

# реліз
git checkout -b release/1.0.0 develop
git commit -am "chore(release): prepare 1.0.0"
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin main v1.0.0
git checkout develop && git merge --no-ff release/1.0.0

# hotfix
git checkout -b hotfix/1.0.1 main
git commit -am "fix(parser): handle missing lesson cell"
git checkout main && git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1 -m "Release 1.0.1"
git checkout develop && git merge --no-ff hotfix/1.0.1
```

## 4. Теги релізів
```bash
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
```
Історія релізів ведеться у `CHANGELOG.md` разом із тегами Git.
