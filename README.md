# Maven Update Release Action

> 📦 GitHub Action для автоматического обновления версии Maven-проекта по семантическому версионированию (MAJOR.MINOR.PATCH)

---

## 📋 Описание

Запускает обновление релизной версии Maven-проекта с поддержкой:
- `X` — **MAJOR** (мажорная версия: breaking changes)
- `Y` — **MINOR** (минорная версия: backward-compatible features)
- `Z` — **PATCH** (патч-версия: backward-compatible bug fixes)

Она:
- Читает текущую версию из `pom.xml`
- Увеличивает нужный компонент и сбрасывает меньшие (если требуется)
- Применяет новую версию с помощью [`versions-maven-plugin`](https://www.mojohaus.org/versions-maven-plugin/)
- Фиксирует изменения в Git с предустановленным сообщением

---

## 🛠 Использование

```yaml
uses: demid1984/maven-update-release-action@v1
with:
  user-email: your.email@example.com
  user-name: Your Name
  release-version-key: Z  # X, Y или Z
```

### 📌 Пример: обновление патч-версии (Patch)

```yaml
- name: Bump patch version
  uses: demid1984/maven-update-release-action@v1
  with:
    user-email: devops@example.com
    user-name: CI Bot
    release-version-key: Z
```

### 📌 Пример: обновление минорной версии

```yaml
- name: Bump minor version
  uses: demid1984/maven-update-release-action@v1
  with:
    user-email: devops@example.com
    user-name: CI Bot
    release-version-key: Y
```

### 📌 Пример: обновление мажорной версии

```yaml
- name: Bump major version
  uses: demid1984/maven-update-release-action@v1
  with:
    user-email: devops@example.com
    user-name: CI Bot
    release-version-key: X
```

---

## 🔧 Входные параметры

| Параметр              | Обязательный | Описание |
|-----------------------|--------------|----------|
| `user-email`          | ✅ Да        | Email, используемый в коммите (например: `devops@example.com`) |
| `user-name`           | ✅ Да        | Имя пользователя, используемое в коммите (например: `CI Bot`) |
| `release-version-key` | ✅ Да        | Тип обновления: `X` (MAJOR), `Y` (MINOR), `Z` (PATCH) |

> ⚠️ **Важно:** Ключ `release-version-key` чувствителен к регистру. Используйте только `X`, `Y`, `Z` (большие буквы).

---

## 🔄 Выходные данные

После выполнения доступны:
- `new-version` — новая версия (например: `2.3.7`)
- `version` — предыдущая версия (например: `2.3.6`)

Пример использования:

```yaml
- name: Bump version and tag release
  id: bump
  uses: demid1984/maven-update-release-action@v1
  with:
    user-email: bot@ci.com
    user-name: Release Bot
    release-version-key: Z

- name: Create Git tag
  run: |
    git tag v${{ steps.bump.outputs.new-version }}
    git push origin v${{ steps.bump.outputs.new-version }}
```

---

## 🧩 Зависимости

Действие использует следующие Maven-плагины:
- [`build-helper-maven-plugin:3.6.0`](https://www.mojohaus.org/build-helper-maven-plugin/) — для парсинга версии
- [`exec-maven-plugin:3.1.0`](https://www.mojohaus.org/exec-maven-plugin/) — для извлечения версии
- [`versions-maven-plugin:2.18.0`](https://www.mojohaus.org/versions-maven-plugin/) — для установки и фиксации версии
- [`maven-scm-plugin:2.1.0`](https://maven.apache.org/scm/maven-scm-plugin/) — для коммита изменений

> ✅ Требуется наличие `./mvnw` (Maven Wrapper).
> 
> ✅ Требуется корректно заполненная секция scm

---

## 📜 Лицензия

Этот проект распространяется под лицензией [MIT](LICENSE).  
© 2026 [demid1984](https://github.com/demid1984)
```