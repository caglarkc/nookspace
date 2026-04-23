# i18n Guide

This project uses `react-i18next` for localization in the renderer layer.

## Files

- `src/renderer/i18n/config.ts`: i18n initialization
- `src/renderer/i18n/locales/en.json`: English locale
- `src/renderer/i18n/locales/zh.json`: Chinese locale

## Basic Usage

```tsx
import { useTranslation } from 'react-i18next';

export function Example() {
  const { t } = useTranslation();
  return <h1>{t('welcome.title')}</h1>;
}
```

## Adding New Strings

1. Add a key to `en.json`
2. Add the same key path to `zh.json`
3. Use `t('path.to.key')` in UI

Keep locale object structures synchronized across languages.

## Best Practices

- Use semantic keys (for example, `settings.apiKey.label`)
- Avoid hardcoded user-facing text in components
- Group translation keys by feature domain
