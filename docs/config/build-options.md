# Параметры сборки

Unless noted, the options in this section are only applied to build.

## build.target

- **Тип:** `string | string[]`
- **По умолчанию:** `'modules'`
- **Связанный:** [Browser Compatibility](/guide/build#browser-compatibility)

Целевая совместимость браузера для финального пакета. Значением по умолчанию является специальное значение Vite, `'modules'`, предназначенное для браузеров с [нативными модулями ES](https://caniuse.com/es6-module), [нативным динамическим импортом ESM](https://caniuse.com/es6-module-dynamic-import) и поддержку [`import.meta`](https://caniuse.com/mdn-javascript_operators_import_meta). Vite заменит `'modules'` на `['es2020', 'edge88', 'firefox78', 'chrome87', 'safari14']`

Еще одно специальное значение — `'esnext'`, которое предполагает собственную поддержку динамического импорта и будет выполнять только минимальную транспиляцию.

Преобразование выполняется с помощью esbuild, и значение должно быть допустимым [опция цели esbuild](https://esbuild.github.io/api/#target). Настраиваемыми целями могут быть версия ES (например, `es2015`), браузер с версией (например, `chrome58`) или массив из нескольких целевых строк.

Обратите внимание, что сборка завершится ошибкой, если код содержит функции, которые нельзя безопасно перенести с помощью esbuild. Подробнее смотрите в [документации esbuild](https://esbuild.github.io/content-types/#javascript).

## build.modulePreload

- **Тип:** `boolean | { polyfill?: boolean, resolveDependencies?: ResolveModulePreloadDependenciesFn }`
- **По умолчанию:** `{ polyfill: true }`

По умолчанию автоматически внедряется [модуль предварительной загрузки полифилла](https://guybedford.com/es-module-preloading-integrity#modulepreload-polyfill). Полифилл автоматически внедряется в прокси-модуль каждой записи `index.html`. Если сборка настроена на использование пользовательской записи, отличной от HTML, через `build.rollupOptions.input`, необходимо вручную импортировать полифилл в вашу пользовательскую запись:

Следует ли автоматически вводить [модуль предварительной загрузки полифилл](https://guybedford.com/es-module-preloading-integrity#modulepreload-polyfill).

Если установлено значение `true`, полифилл автоматически внедряется в прокси-модуль каждой записи `index.html`. Если сборка настроена на использование пользовательской записи, отличной от HTML, через `build.rollupOptions.input`, необходимо вручную импортировать полифилл в вашу пользовательскую запись:

```js
import 'vite/modulepreload-polyfill'
```

Примечание: Полифилл **не** применяется к [режиму библиотеки](/guide/build#library-mode). Если вам нужно поддерживать браузеры без собственного динамического импорта, вам, вероятно, следует избегать его использования в вашей библиотеке.

Полифилл можно отключить с помощью `{ polyfill: false }`.

Список фрагментов для предварительной загрузки для каждого динамического импорта вычисляется Vite. По умолчанию при загрузке этих зависимостей будет использоваться абсолютный путь, включая `base`. Если `base` является относительной (`''` или `'./'`), `import.meta.url` используется во время выполнения, чтобы избежать абсолютных путей, которые зависят от конечной развернутой базы.

Существует экспериментальная поддержка детального контроля над списком зависимостей и их путями с помощью функции `resolveDependencies`. [Оставить отзыв](https://github.com/vitejs/vite/discussions/13841). Он ожидает функцию типа `ResolveModulePreloadDependenciesFn`:

```ts
type ResolveModulePreloadDependenciesFn = (
  url: string,
  deps: string[],
  context: {
    hostId: string
    hostType: 'html' | 'js'
  },
) => string[]
```

Функция `resolveDependencies` будет вызываться для каждого динамического импорта со списком фрагментов, от которых он зависит, а также она будет вызываться для каждого фрагмента, импортированного в HTML-файлах входа. Новый массив зависимостей может быть возвращен с этими отфильтрованными или более внедренными зависимостями и их измененными путями. Пути `deps` являются относительными к `build.outDir`. Возвращаемое значение должно быть относительным путем к `build.outDir`.

```js twoslash
/** @type {import('vite').UserConfig} */
const config = {
  // prettier-ignore
  build: {
// ---cut-before---
modulePreload: {
  resolveDependencies: (filename, deps, { hostId, hostType }) => {
    return deps.filter(condition)
  },
},
// ---cut-after---
  },
}
```

Разрешенные пути зависимостей можно дополнительно изменить с помощью [`experimental.renderBuiltUrl`](../guide/build.md#advanced-base-options).

## build.polyfillModulePreload

- **Тип:** `boolean`
- **По умолчанию:** `true`
- **Устарело** вместо этого используйте `build.modulePreload.polyfill`
  Следует ли автоматически вводить [module preload polyfill](https://guybedford.com/es-module-preloading-integrity#modulepreload-polyfill).

## build.outDir

- **Тип:** `string`
- **По умолчанию:** `dist`

Укажите выходной каталог (относительно [корень проекта](/guide/#index-html-and-project-root)).

## build.assetsDir

- **Тип:** `string`
- **По умолчанию:** `assets`

Укажите каталог для вложения сгенерированных ресурсов (относительно `build.outDir`. Он не используется в [режиме библиотеки](/guide/build#library-mode)).

## build.assetsInlineLimit

- **Тип:** `number` | `((filePath: string, content: Buffer) => boolean | undefined)`
- **По умолчанию:** `4096` (4 KiB)

Импортированные ресурсы или ресурсы, на которые есть ссылки, размер которых меньше этого порога, будут встроены как URL-адреса base64, чтобы избежать дополнительных HTTP-запросов. Установите `0`, чтобы полностью отключить встраивание.

Если передан обратный вызов, может быть возвращено логическое значение для согласия или отказа. Если ничего не возвращено, применяется логика по умолчанию.

Заполнители Git LFS автоматически исключаются из встраивания, поскольку они не содержат содержимое файла, который они представляют.

::: tip Примечание
Если вы укажете `build.lib`, `build.assetsInlineLimit` будет игнорироваться, а активы всегда будут встроенными, независимо от размера файла или быть заполнителем Git LFS.
:::

## build.cssCodeSplit

- **Тип:** `boolean`
- **По умолчанию:** `true`

Включить/отключить разделение CSS-кода. Если этот параметр включен, CSS, импортированный в асинхронные фрагменты JS, будет сохраняться как фрагменты и извлекаться вместе при извлечении фрагмента.

Если этот параметр отключен, все CSS во всем проекте будут извлечены в один файл CSS.

::: tip Примечание
Если вы укажете `build.lib`, `build.cssCodeSplit` будет по умолчанию `false`.
:::

## build.cssTarget

- **Тип:** `string | string[]`
- **По умолчанию:** то же, что и [`build.target`](#build-target)

Этот параметр позволяет пользователям установить другую цель браузера для минификации CSS, отличную от той, которая используется для транспиляции JavaScript.

Его следует использовать только в том случае, если вы ориентируетесь на неосновной браузер.
Одним из примеров является Android WeChat WebView, который поддерживает большинство современных функций JavaScript, но не поддерживает [`#RGBA` шестнадцатеричное обозначение цвета в CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#rgb_colors).
В этом случае вам нужно установить для `build.cssTarget` значение `chrome61`, чтобы vite не преобразовывал цвета `rgba()` в шестнадцатеричные обозначения `#RGBA`.

## build.cssMinify

- **Тип:** `boolean | 'esbuild' | 'lightningcss'`
- **По умолчанию:** то же, что и [`build.minify`](#build-minify) для клиента, `'esbuild'` для SSR

This option allows users to override CSS minification specifically instead of defaulting to `build.minify`, so you can configure minification for JS and CSS separately. Vite uses `esbuild` by default to minify CSS. Set the option to `'lightningcss'` to use [Lightning CSS](https://lightningcss.dev/minification.html) instead. If selected, it can be configured using [`css.lightningcss`](./shared-options.md#css-lightningcss).

## build.sourcemap

- **Тип:** `boolean | 'inline' | 'hidden'`
- **По умолчанию:** `false`

Создавайте исходные карты производства. Если `true`, будет создан отдельный файл исходной карты. Если `'inline'`, исходная карта будет добавлена к результирующему выходному файлу как URI данных. `'hidden'` работает так же, как `true`, за исключением того, что соответствующие комментарии исходной карты в связанных файлах подавляются.

## build.rollupOptions

- **Тип:** [`RollupOptions`](https://rollupjs.org/configuration-options/)

Непосредственно настройте базовый пакет Rollup. Это то же самое, что параметры, которые можно экспортировать из файла конфигурации накопительного пакета и будут объединены с внутренними параметрами накопительного пакета Vite. Дополнительную информацию см. в [Документации по Rollup](https://rollupjs.org/configuration-options/).

## build.commonjsOptions

- **Тип:** [`RollupCommonJSOptions`](https://github.com/rollup/plugins/tree/master/packages/commonjs#options)

Параметры для перехода к [@rollup/plugin-commonjs](https://github.com/rollup/plugins/tree/master/packages/commonjs).

## build.dynamicImportVarsOptions

- **Тип:** [`RollupDynamicImportVarsOptions`](https://github.com/rollup/plugins/tree/master/packages/dynamic-import-vars#options)
- **Связанный:** [Dynamic Import](/guide/features#dynamic-import)

Параметры для перехода к [@rollup/plugin-dynamic-import-vars](https://github.com/rollup/plugins/tree/master/packages/dynamic-import-vars).

## build.lib

- **Тип:** `{ entry: string | string[] | { [entryAlias: string]: string }, name?: string, formats?: ('es' | 'cjs' | 'umd' | 'iife')[], fileName?: string | ((format: ModuleFormat, entryName: string) => string), cssFileName?: string }`
- **Связанный:** [Режим библиотеки](/guide/build#library-mode)

Сборка как библиотеки. `entry` требуется, так как библиотека не может использовать HTML в качестве записи. `name` является открытой глобальной переменной и требуется, когда `formats` включает `'umd'` или `'iife'`. По умолчанию `formats` являются `['es', 'umd']` или `['es', 'cjs']`, если используется несколько записей.

`fileName` — это имя выходного файла пакета, которое по умолчанию равно `"name"` в `package.json`. Его также можно определить как функцию, принимающую `format` и `entryName` в качестве аргументов и возвращающую имя файла.

Если ваш пакет импортирует CSS, `cssFileName` можно использовать для указания имени выходного файла CSS. По умолчанию оно равно тому же значению, что и `fileName`, если оно задано как строка, в противном случае оно также возвращается к `"name"` в `package.json`.

```js twoslash [vite.config.js]
import { defineConfig } from 'vite'

export default defineConfig({
  build: {
    lib: {
      entry: ['src/main.js'],
      fileName: (format, entryName) => `my-lib-${entryName}.${format}.js`,
      cssFileName: 'my-lib-style',
    },
  },
})
```

## build.manifest

- **Тип:** `boolean | string | string[] | { [entryAlias: string]: string }`
- **По умолчанию:** `false`
- **Связанный:** [Backend Integration](/guide/backend-integration)

Если установлено значение `true`, сборка также создаст файл `.vite/manifest.json`, содержащий сопоставление имен файлов нехешированных ресурсов с их хешированными версиями, которые затем могут использоваться серверной платформой для отображения правильных ссылки на активы. Если значение представляет собой строку, оно будет использоваться в качестве имени файла манифеста.

## build.ssrManifest

- **Тип:** `boolean | string`
- **По умолчанию:** `false`
- **Связанный:** [Server-Side Rendering](/guide/ssr)

Если установлено значение `true`, сборка также будет генерировать манифест SSR для определения ссылок на стили и директив предварительной загрузки ресурсов в рабочей среде. Если значение представляет собой строку, оно будет использоваться в качестве имени файла манифеста.

Если установлено значение `true`, сборка также будет генерировать манифест SSR для определения ссылок на стили и директив предварительной загрузки ресурсов в рабочей среде. Если значение представляет собой строку, оно будет использоваться в качестве имени файла манифеста.

## build.ssr

- **Тип:** `boolean | string`
- **По умолчанию:** `false`
- **Связанный:** [Server-Side Rendering](/guide/ssr)

Создание сборки, ориентированной на SSR. Значение может быть строкой для прямого указания записи SSR или `true`, что требует указания записи SSR через `rollupOptions.input`.

## build.emitAssets

- **Type:** `boolean`
- **Default:** `false`

During non-client builds, static assets aren't emitted as it is assumed they would be emitted as part of the client build. This option allows frameworks to force emitting them in other environments build. It is responsibility of the framework to merge the assets with a post build step.

## build.ssrEmitAssets

- **Тип:** `boolean`
- **По умолчанию:** `false`

Во время сборки SSR статические ресурсы не выдаются, поскольку предполагается, что они будут выдаваться как часть сборки клиента. Эта опция позволяет фреймворкам принудительно выдавать их как в сборке клиента, так и в сборке SSR. Фреймворк отвечает за объединение ресурсов с шагом после сборки. Эта опция будет заменена на `build.emitAssets`, как только API среды станет стабильным.

## build.minify

- **Тип:** `boolean | 'terser' | 'esbuild'`
- **По умолчанию:** `'esbuild'` для клиентской сборки, `false` для SSR-сборки

Установите значение `false`, чтобы отключить минимизацию, или укажите минимизатор для использования. По умолчанию используется [esbuild](https://github.com/evanw/esbuild), который в 20–40 раз быстрее, чем краткий, и только на 1–2% хуже сжатие. [Тесты](https://github.com/privatenumber/minification-benchmarks)

Обратите внимание, что опция `build.minify` не уменьшает пробелы при использовании формата `'es'` в режиме библиотеки, поскольку она удаляет чистые аннотации и предотвращает tree-shaking.

Обратите внимание, что опция `build.minify` не минимизирует пробелы при использовании формата `'es'` в режиме lib, поскольку она удаляет чистые аннотации и прерывает tree-shaking.

Terser должен быть установлен, если для него задано значение `'terser'`.

```sh
npm add -D terser
```

## build.terserOptions

- **Тип:** `TerserOptions`

Дополнительные [опции минимизации](https://terser.org/docs/api-reference#minify-options) для передачи в Terser.

Кроме того, вы также можете передать параметр `maxWorkers: number`, чтобы указать максимальное количество создаваемых рабочих. По умолчанию количество процессоров минус 1.

## build.write

- **Тип:** `boolean`
- **По умолчанию:** `true`

Установите значение `false`, чтобы отключить запись пакета на диск. В основном это используется в [программных вызовах `build()`](/guide/api-javascript#build), где перед записью на диск требуется дополнительная постобработка пакета.

## build.emptyOutDir

- **Тип:** `boolean`
- **По умолчанию:** `true`, если `outDir` находится внутри `root`

По умолчанию Vite очищает `outDir` при сборке, если он находится внутри корня проекта. Он выдаст предупреждение, если `outDir` находится за пределами root, чтобы избежать случайного удаления важных файлов. Вы можете явно установить этот параметр, чтобы скрыть предупреждение. Это также доступно через командную строку как `--emptyOutDir`.

## build.copyPublicDir

- **Тип:** `boolean`
- **По умолчанию:** `true`

По умолчанию Vite копирует файлы из `publicDir` в `outDir` при сборке. Установите `false`, чтобы отключить это.

## build.reportCompressedSize

- **Тип:** `boolean`
- **По умолчанию:** `true`

Включить/отключить отчеты о размерах, сжатых gzip. Сжатие больших выходных файлов может быть медленным, поэтому его отключение может повысить производительность сборки для больших проектов.

## build.chunkSizeWarningLimit

- **Тип:** `number`
- **По умолчанию:** `500`

Ограничение для предупреждений о размере фрагмента (в КБ). Он сравнивается с размером несжатого фрагмента, поскольку [размер JavaScript сам по себе связан со временем выполнения](https://v8.dev/blog/cost-of-javascript-2019).

## build.watch

- **Тип:** [`WatcherOptions`](https://rollupjs.org/configuration-options/#watch)`| null`
- **По умолчанию:** `null`

Установите `{}`, чтобы включить наблюдатель сводки. Это в основном используется в случаях, когда речь идет о плагинах только для сборки или процессах интеграции.

::: warning Использование Vite в подсистеме Windows для Linux (WSL) 2

Бывают случаи, когда просмотр файловой системы не работает с WSL2.
Подробнее смотрите [`server.watch`](./server-options.md#server-watch).

:::
