# 📑 Шаблон: SEO-метаданные и Schema.org (SEO & Metadata Template)

#templates #seo #metadata #opengraph #schema #json-ld

> **Назначение:** Полный шаблон поисковой оптимизации, социальных превью (OpenGraph/Twitter) и структурированных данных Schema.org JSON-LD.

---

## 1. Функция `generateMetadata` для Next.js App Router

```typescript
import type { Metadata } from 'next'
import { getSiteUrl } from '@/lib/siteUrl'

export async function generateMetadata({ params }): Promise<Metadata> {
  const { slug } = await params
  const siteUrl = getSiteUrl()
  const canonicalUrl = `${siteUrl}/services/${slug}`

  return {
    title: 'Service Title | Erythro.ai',
    description: 'Clear, high-converting meta description up to 160 chars.',
    alternates: {
      canonical: canonicalUrl,
      languages: {
        'en-US': `${canonicalUrl}?lang=en`,
        'ru-RU': `${canonicalUrl}?lang=ru`,
        'he-IL': `${canonicalUrl}?lang=he`,
      },
    },
    openGraph: {
      title: 'Service Title | Erythro.ai',
      description: 'High-performance AI engineering and web development.',
      url: canonicalUrl,
      siteName: 'Erythro.ai',
      locale: 'en_US',
      type: 'website',
      images: [
        {
          url: `${siteUrl}/images/og-default.png`,
          width: 1200,
          height: 630,
          alt: 'Erythro.ai',
        },
      ],
    },
    twitter: {
      card: 'summary_large_image',
      title: 'Service Title | Erythro.ai',
      description: 'High-performance AI engineering and web development.',
      images: [`${siteUrl}/images/og-default.png`],
    },
  }
}
```

---

## 2. Структурированные данные Schema.org (JSON-LD)

Вставляются в корень страницы через `<script type="application/ld+json">`:

```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://erythro.ai/#organization",
      "name": "Erythro.ai",
      "url": "https://erythro.ai",
      "logo": "https://erythro.ai/logo.png",
      "sameAs": [
        "https://facebook.com/erythro.ai",
        "https://t.me/erythroai"
      ],
      "contactPoint": {
        "@type": "ContactPoint",
        "telephone": "+972505308305",
        "contactType": "customer service",
        "email": "order@erythro.ai",
        "areaServed": ["IL", "EU", "US"],
        "availableLanguage": ["English", "Russian", "Hebrew"]
      }
    },
    {
      "@type": "WebSite",
      "@id": "https://erythro.ai/#website",
      "url": "https://erythro.ai",
      "name": "Erythro.ai",
      "publisher": {
        "@id": "https://erythro.ai/#organization"
      }
    }
  ]
}
```
