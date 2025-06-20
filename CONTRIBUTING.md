# Contributing to ITMO-Plus

## Содержание

1. Развёртывание проекта локально
1. Создание задач (Issues)
1. Процесс работы через fork (GitHub Flow)
1. Код-стайл коммитов
1. Тесты и линтеры
1. Оформление Pull Request
1. Code Review и слияние
1. Документация
1. Лицензия и правовая информация

---

## Работа через fork (GitHub Flow)

### Инициализация

```bash
git clone https://github.com/<your_github>/ITMO-Plus.git
cd ./ITMO-Plus
git remote add upstream https://github.com/r4m63/ITMO-Plus.git
```

Проверить `git remote -v` -- должно показывать origin (ваш fork) + upstream (центральный репозиторий).

Выполнить вход если не выполнен `gh auth login`.

### Цикл разработки

Синхронизация с основным репозиторием.

```bash
git fetch upstream
git switch main
git rebase upstream/main
git push origin main
```

Создать рабочую ветку.

```bash
git switch -c <feature/login>
```

Пишем код и затем коммитим.

```bash
git add .
git commit -m "<message>"
git push -u origin <feature/login>
```

GitHub предложит Create pull request.

После удачного pull request удалить локально ветку.

```bash
git switch main
git pull upstream main
git branch -d <feature/login>
git push origin --delete <feature/login>
```

### Срочный Hotfix

- `git fetch upstream && git switch -c <hotfix/crash> upstream/main`
- commit -> `git push origin <hotfix/crash>` -> Create pull request

## Код-стайл коммитов

В качестве основы
используется [Angular Git commit Message Convention](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format) -
это наиболее авторитетный источник. Все остальные вариации конвенций по коммитам ссылаются на эту.

### Шаблон коммита

Для оформления сообщения коммита следует использовать следующий шаблон:

    <type>(<scope>): <description>
        <BLANK LINE>
    <body> 
        <BLANK LINE>
    <footer>

Type, scope и description вместе составляют заголовок коммита (header).

- **Type** - [тип коммита](#тип-коммита) (рефакторинг, исправление багов, новая фича и т.п.)
- **Scope** - область действия (где были изменения). Это может быть отдельный файл, директория или затронутая часть
  проекта (rendering, routing и т.д.). Не обязательно для заполнения (но желательно).
- **Description** - описание сути коммита. Обычно отвечает на вопрос "что было изменено/добавлено/удалено?" Например:
  add comment section
- **Blank line** - пустая строка, ей отделяется тело коммита от заголовка, и тело от футера.
- **Body** - не обязательно для заполнения. Здесь обычно отвечают на вопрос "Зачем были изменения?" и "Почему сделаны
  именно такие изменения?" (почему не стоит делать по-другому).
- **Footer** - не обязательно для заполнения. Может содержать информацию о критических изменениях (breaking changes), а
  также является местом для указания задач из бэклога, GitHub issues, тикетов, и других проблем, которые этот коммит
  закрывает или с которыми он связан.

### Пример коммита

    chore: drop Node 6 from testing matrix
    refactor(Block.ts): update render method
    
    see the issue for details on the typos fixed
    
    BREAKING CHANGE: dropping Node 6 which hits end of life in April
    closes issue #12

### Правила

1. Коммиты пишутся на английском языке.
2. Заголовок коммита пишется с маленькой буквы.
3. Точка в конце не ставится.
4. Длина заголовка не должна превышать 100 символов, а лучше 50. Подробности коммита выносятся в тело и футер.
5. Description начинается с глагола. Глагол указывается в настоящем времени, например: add, update, improve, remove (не
   added, updates) и тд.

### Как писать коммит при Pull Request

1. Сообщение коммита при merge PR формируется по такому же принципу как описано выше.
2. В конец заголовка коммита включается номер PR в скобочках после основного сообщения. Например:
   ``refactor(Block.ts): update render method (#67)``
3. Все сообщения коммитов из сливаемой ветки вносятся в тело коммита.

### Тип коммита

* **feat** - используется при добавлении новой функциональности.
* **fix** - исправление багов.
* **refactor** - изменения кода, которые не исправляет баги и не добавляют функционал.
* **chore** - изменение конфигов, системы сборки, обновление зависимостей и т.д.
* **test** - всё, что связано с тестированием.
* **style** - исправление опечаток, изменение форматирования кода (переносы, отступы, точки с запятой и т.п.) без
  изменения смысла кода.
* **docs** - изменения только в документации.

Эти типы расширяют вышеописанные, но их использовать не обязательно:

* **perf** - изменения кода, повышающие производительность.
* **build** - изменения, влияющие на систему сборки или внешние зависимости (webpack, npm).
* **ci** - изменения в файлах конфигурации.

### BREAKING CHANGE (критические изменения)

BREAKING CHANGE: указывается в футере и автоматически добавляется в конец заголовка. Критические изменения - это
изменения, нарушающие обратную совместимость. Может быть частью коммита любого типа.

Должен начинаться с фразы `BREAKING CHANGE:`, за которой следует краткое изложение критического изменения, пустая
строка и подробное описание критического изменения.
