# MC-DB Forge

CLI-инструмент для генерации SQL и Java-заготовок по YAML-конфигу и первичного применения схемы в PostgreSQL.

## Важное ограничение

Используйте `MC-DB Forge` только для новых проектов и первичного разворачивания новой БД, а не для обновления уже существующей базы данных.

В текущей версии повторный `apply` нельзя использовать как механизм миграций.

## Требования

- Node.js `20+`
- `npm`
- PostgreSQL `13+` (рекомендуется)

## Быстрый старт

```bash
sudo apt update
sudo apt install -y curl ca-certificates postgresql-client
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
mkdir -p mc-db-forge
cd mc-db-forge
```

Скопируйте файлы проекта в эту папку, затем:

```bash
npm install
npm install -g .
cp config.example.RU.yaml config.yaml
cp .env-example.RU .env
```

Заполните параметры в `config.yaml` и `.env`, затем запустите:

```bash
mcdb generate config.yaml --dry --lang ru
mcdb generate config.yaml --lang ru
mcdb apply config.yaml --safe --lang ru
```

## Структура

- `src/`
- `dist/`
- `tests/`
- `images/`
- `package.json`
- `package-lock.json`
- `tsconfig.json`
- `tsconfig.build.json`
- `config-simple.yaml`
- `config.example.yaml`
- `.env-example`
- `README.md`
- `demo.gif`

## Установка

### Локально

```bash
npm install
npm run build
```

### VPS/VDS (Ubuntu/Debian)

```bash
ssh user@your-server-ip
sudo apt update
sudo apt install -y curl ca-certificates git postgresql-client
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
git clone <your-repository-url> mc-db-forge
cd mc-db-forge
npm install
npm run build
```

## Конфиг и env

Основные файлы:

- `config.yaml`
- `.env`

Создать из примеров:

```bash
cp config.example.RU.yaml config.yaml
cp .env-example.RU .env
```

PowerShell:

```powershell
Copy-Item config.example.RU.yaml config.yaml
Copy-Item .env-example.RU .env
```

## Режим запуска

Основной режим:

```bash
npm install -g .
```

После этого запуск через `mcdb`.

Запасной вариант:

```bash
node dist/cli.js <команда> ...
```

## Команды CLI

Генерация файлов:

```bash
mcdb generate config.yaml --lang ru
mcdb generate config.yaml --dry --lang ru
mcdb generate config.yaml --out ./generated --lang ru
```

Применение в PostgreSQL:

```bash
mcdb apply config.yaml --lang ru
mcdb apply config.yaml --safe --lang ru
mcdb apply config.yaml --create-db --lang ru
mcdb apply config.yaml --db "postgres://user:password@127.0.0.1:5432/mcdb" --lang ru
```

Флаги:

- `--out <output-dir>` - папка для сохранения сгенерированных файлов.
- `--dry` - пробный запуск для `generate` без записи файлов.
- `--db <connection-string>` - строка подключения к PostgreSQL для `apply`.
- `--safe` - проверка SQL на потенциально опасные команды перед `apply`.
- `--create-db` - попытка создать БД перед `apply` (имя БД обязательно в URL).
- `--lang ru` - русские сообщения в консоли.
- `--lang en` - английские сообщения в консоли (по умолчанию).

## Выходные файлы

По умолчанию:

- `generated/sql/init.sql`
- `generated/sql/roles.sql`
- `generated/java/model/*.java`
- `generated/java/repository/*Repository.java`
- `generated/java/service/*Service.java`

## Частые ошибки

`Файл конфигурации не найден`

- неверный путь к `config.yaml`

`Environment variable not found: ...`

- в конфиге есть `${VAR}`, но переменная не задана в `.env` или окружении процесса

`Не задано подключение к БД`

- не передан `--db` и не заполнены обязательные `DB_*`

`Ошибка подключения к БД`

- неверные `host/port/user/password/db`
- проблемы сети/фаервола
- SSL-несовместимость

`--create-db требует имя базы`

- в URL отсутствует имя базы

Ошибки прав (создание БД/роли)

- недостаточные права пользователя PostgreSQL

## Рекомендации

- Перед первым реальным применением делайте `generate --dry`.
- Используйте `apply` только для первичного bootstrap.
- Для эволюции схемы используйте отдельный migration tool.

## Контакты

- Telegram: https://t.me/roman_svyatkov
- Email: `romansvyatkov222@gmail.com`

## Поддержка автора

- Boosty: https://boosty.to/romadev.dev

Крипто-поддержка:

- USDT (BEP20)
- Адрес: `0xfD36De3652368133Ab00872018c4c047c851cBBF`
- Сеть: BEP20 (Binance Smart Chain)
- Блокчейн: https://bscscan.com/address/0xfD36De3652368133Ab00872018c4c047c851cBBF
