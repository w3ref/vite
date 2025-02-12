# Параметры работника

Если не указано иное, параметры в этом разделе применяются ко всем версиям dev, build и preview.

## worker.format

- **Тип:** `'es' | 'iife'`
- **По умолчанию:** `'iife'`

Выходной формат для пакета рабочего.

## worker.plugins

- **Тип:** [`() => (Plugin | Plugin[])[]`](./shared-options#plugins)

Плагины Vite, которые применяются к рабочим пакетам. Обратите внимание, что [config.plugins](./shared-options#plugins) применяется только к воркерам в разработке, вместо этого его следует настроить здесь для сборки.
Функция должна возвращать новые экземпляры плагинов, поскольку они используются в параллельных сборках рабочих групп. Таким образом, изменение параметров `config.worker` в хуке `config` будет игнорироваться.

## worker.rollupOptions

- **Тип:** [`RollupOptions`](https://rollupjs.org/configuration-options/)

Rollup для создания рабочего пакета.
