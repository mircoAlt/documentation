---
title: Deliveries Config
menus:
  reference-docs:
    parent: Project Config
---
{{< added-in release-2020-12 >}}

You can configure `deliveries` to let Livingdocs know something about the delivery/deliveries you operate.

As of December 2020 this is used to enable the `Internal Document Links` feature.

An example:
```js
// deliveries is configured at projectConfig.deliveries
deliveries: [
  {
    handle: 'web',
    label: 'Website',
    isPrimary: true,
    icon: 'book-open',
    url: {
      origin: 'https://example.com',
      pathPattern: '/article/:id'
    }
  },
  {
    handle: 'app',
    label: 'App',
    icon: 'rocket',
    url: {
      origin: 'https://example.com',
      pathPattern: '/app/article/:id'
    }
  }
]
```

## pathPattern
The `Internal Document Links` feature will construct a URL to a document based on these placeholders:

- `:id` (document.id, always available)
- `:projectId`, (document.projectId, always available)
- `:slug` (document.metadata.slug, available if you have a metadata property with the handle slug)

This URL is then inserted into to `a` tags `href` attribute within the document.
Additionally a `data-li-document-ref` attribute is written containing the `document.id`.

### Caveats
Since the `Internal Document Links` is specifically handy to link to not yet published documents you want to validate these links somewhere in your delivery to remove the ones pointing to not yet published documents.
As this feature works on unpublished documents, Livingdocs cannot resolve the `routing` information from the [Routing System]({{< ref "/enterprise/guides/routing-system.md" >}}) since this stores the information on publish which makes it unavailable before a document is published.

To have the `Internal Document Links` feature be able to construct working `href` attributes, you need a routing system in your delivery that is able to redirect a user do a valid document URL based on the `document.id` and `document.projectId` only.
