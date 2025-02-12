# Интерфейс командной строки

## Сервер разработки

### `vite`

Запустите сервер разработки Vite в текущем каталоге. `vite dev` и `vite serve` являются псевдонимами для `vite`.

#### Использование

```bash
vite [root]
```

#### Опции

| Опции                    |                                                                                                                                     |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| `--host [host]`          | Укажите имя хоста (`string`)                                                                                                        |
| `--port <port>`          | Укажите порт (`number`)                                                                                                             |
| `--open [path]`          | Открыть браузер при запуске (`boolean \| string`)                                                                                   |
| `--cors`                 | Включить CORS (`boolean`)                                                                                                           |
| `--strictPort`           | Выйти, если указанный порт уже используется (`boolean`)                                                                             |
| `--force`                | Заставить оптимизатор игнорировать кеш и выполнить повторную сборку (`boolean`)                                                     |
| `-c, --config <file>`    | Использовать указанный файл конфигурации (`string`)                                                                                 |
| `--base <path>`          | Публичный базовый путь (по умолчанию: `/`) (`string`)                                                                               |
| `-l, --logLevel <level>` | info \| warn \| error \| silent (`string`)                                                                                          |
| `--clearScreen`          | Разрешить/отключить очистку экрана при входе в систему (`boolean`)                                                                  |
| `--profile`              | Запустите встроенный инспектор Node.js (проверьте [Узкие места производительности](/guide/troubleshooting#performance-bottlenecks)) |
| `-d, --debug [feat]`     | Показать журналы отладки (`string \| boolean`)                                                                                      |
| `-f, --filter <filter>`  | Фильтрация журналов отладки (`string`)                                                                                              |
| `-m, --mode <mode>`      | Установить режим окружения (`string`)                                                                                               |
| `-h, --help`             | тобразить доступные параметры CLI                                                                                                   |
| `-v, --version`          | Отображение номера версии                                                                                                           |

## Сборка

### `vite build`

Сборка для продакшена.

#### Использование

```bash
vite build [root]
```

#### Опции

| Опции                        |                                                                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| `--target <target>`            | Цель транспиляции (по умолчанию: `"modules"`) (`string`)                                                                  |
| `--outDir <dir>`               | Выходной каталог (по умолчанию: `dist`) (`string`)                                                                       |
| `--assetsDir <dir>`            | Каталог в outDir для размещения ресурсов (по умолчанию: `"assets"`) (`string`)                                          |
| `--assetsInlineLimit <number>` | Пороговое значение встроенного статического ресурса base64 в байтах (по умолчанию: `4096`) (`number`)                                          |
| `--ssr [entry]`                | Сборка указанной записи для рендеринга на стороне сервера (`string`)                                                          |
| `--sourcemap [output]`         | Карты исходного кода для сборки (по умолчанию: `false`) (`boolean \| "inline" \| "hidden"`)                                 |
| `--minify [minifier]`          | Включить/отключить минификацию или указать минификатор для использования (по умолчанию: `"esbuild"`) (`boolean \| "terser" \| "esbuild"`) |
| `--manifest [name]`            | Выдать сборку manifest json (`boolean \| string`)                                                                      |
| `--ssrManifest [name]`         | Выдать ssr manifest json (`boolean \| string`)                                                                        |
| `--emptyOutDir`                | Принудительно очистить outDir, если он находится за пределами корня (`boolean`)                                                            |
| `-w, --watch`                  | Пересборка при изменении модулей на диске (`boolean`)                                                              |
| `-c, --config <file>`          | Использовать указанный файл конфигурации (`string`)                                                                                |
| `--base <path>`                | Публичный базовый путь (по умолчанию: `/`) (`string`)                                                                          |
| `-l, --logLevel <level>`       | Info \| warn \| error \| silent (`string`)                                                                          |
| `--clearScreen`                | Разрешить/отключить очистку экрана при входе в систему (`boolean`)                                                                 |
| `--profile`                    | Запустить встроенный инспектор Node.js (проверить [Узкие места производительности](/guide/troubleshooting#performance-bottlenecks))  |
| `-d, --debug [feat]`           | Показать журналы отладки (`string \| boolean`)                                                                               |
| `-f, --filter <filter>`        | Фильтр журналов отладки (`string`)                                                                                        |
| `-m, --mode <mode>`            | Установить режим окружения (`string`)                                                                                             |
| `-h, --help`                   | Показать доступные параметры CLI                                                                                       |
| `--app`                        | Построить все среды, как и `builder: {}` (`boolean`, экспериментальный)                                             |

## Другие

### `vite optimize`

Предварительные зависимости.

#### Использование

```bash
vite optimize [root]
```

#### Опции

| Опции                    |                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------- |
| `--force`                | Заставить оптимизатор игнорировать кеш и выполнить повторную сборку (`boolean`) |
| `-c, --config <file>`    | Использовать указанный файл конфигурации (`string`)                             |
| `--base <path>`          | Публичный базовый путь (по умолчанию: `/`) (`string`)                           |
| `-l, --logLevel <level>` | Info \| warn \| error \| silent (`string`)                                      |
| `--clearScreen`          | Разрешить/отключить очистку экрана при входе в систему (`boolean`)              |
| `-d, --debug [feat]`     | Показать журналы отладки (`string \| boolean`)                                  |
| `-f, --filter <filter>`  | Фильтрация журналов отладки (`string`)                                          |
| `-m, --mode <mode>`      | Установить режим окружения (`string`)                                           |
| `-h, --help`             | Отобразить доступные параметры CLI                                              |

### `vite preview`

Локально просмотрите производственную сборку. Не используйте его в качестве рабочего сервера, поскольку он не предназначен для этого.

#### Использование

```bash
vite preview [root]
```

#### Опции

| Опции                    |                                                                    |
| ------------------------ | ------------------------------------------------------------------ |
| `--host [host]`          | Укажите имя хоста (`string`)                                       |
| `--port <port>`          | Укажите порт (`number`)                                            |
| `--strictPort`           | Выйти, если указанный порт уже используется (`boolean`)            |
| `--open [path]`          | Открыть браузер при запуске (`boolean \| string`)                  |
| `--outDir <dir>`         | Выходной каталог (по умолчанию: `dist`)(`string`)                  |
| `-c, --config <file>`    | Использовать указанный файл конфигурации (`string`)                |
| `--base <path>`          | Публичный базовый путь (по умолчанию: `/`) (`string`)              |
| `-l, --logLevel <level>` | Info \| warn \| error \| silent (`string`)                         |
| `--clearScreen`          | Разрешить/отключить очистку экрана при входе в систему (`boolean`) |
| `-d, --debug [feat]`     | Показать журналы отладки (`string \| boolean`)                     |
| `-f, --filter <filter>`  | Фильтрация журналов отладки (`string`)                             |
| `-m, --mode <mode>`      | Установить режим окружения (`string`)                              |
| `-h, --help`             | Отобразить доступные параметры CLI                                 |
