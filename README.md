# postcss-exclude-files [![Build Status](https://travis-ci.org/anyley/postcss-exclude-files.svg?branch=master)](https://travis-ci.org/anyley/postcss-exclude-files)

postcss plugin for exclude files from processing

## Install

```bash
npm i postcss-exclude-files
```

or

```bash
yarn add postcss-exclude-files
```

## Usage

**webpack.config.babel.js**
```javascript
// postcss plugins
import autoprefixer from 'autoprefixer'
import postcssFlexbugsFixes from 'postcss-flexbugs-fixes'
import colorguard from 'colorguard'
import stylelint from 'stylelint'
import postcssReporter from 'postcss-reporter'
import rucksackCss from 'rucksack-css'
import excludeFiles from './app/lib/postcss-exclude-files'

export default () => {
  // ...
  module: {
    rules: [
      {
        test: /\.s?css$/,
        use:  [
          'style-loader',
          'css-loader',
          'sass-loader',
          {
            loader:  'postcss-loader',
            options: {
              plugins: [
                excludeFiles({
                  filter:  '**/node_modules/**',
                  plugins: [
                    rucksackCss,
                    stylelint,
                    colorguard
                  ]
                }),
                postcssFlexbugsFixes,
                autoprefixer({ grid: true }),
                postcssReporter({ clearReportedMessages: true }),
              ]
            }
          },
        ]
      }
    ]
  }
  // ...
}
```

**Using in Laravel vite**
```javascript
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";
import excludeFiles from "postcss-exclude-files";
import rtlcss from "rtlcss";
import autoprefixer from "autoprefixer";

export default defineConfig({
    plugins: [
        laravel({
            input: [
                "resources/sass/app.ltr.scss",
                "resources/sass/app.rtl.scss",
                "resources/js/app.js",
            ],
            refresh: true,
        }),
    ],
    css: {
        postcss: {
            plugins: [
                excludeFiles({
                    filter: "**/*.ltr.*", // exclude ltr form rtlcss
                    plugins: [rtlcss],
                }),
                autoprefixer,
            ],
        },
    },
});

```
