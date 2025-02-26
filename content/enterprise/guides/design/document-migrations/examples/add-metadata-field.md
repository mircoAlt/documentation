---
title: "Example: Add a Metadata Field"
linkTitle: Add a Metadata Field
tags: [guides, maintenance]
menus:
  guides:
    parent: Livingdocs Data Migrations
---

Adding a new metadata field is not a breaking change but it's a common use case that you want to update your old documents with the new metadata entry. Additions of metadata have nothing to do with the design. The design can only define an automated field extractor for metadata, but not the metadata itself. The definition of the metadata is done in the server.

In this example we will introduce a new metadata field "title" and prefill it in all documents with the value of the title field of the first header found in the document (if any).

Designs for the example
- [test@1.0.0](../designs/test_design_1.0.0.js)

```js
const _ = require('lodash')
const liSDK = require('@livingdocs/node-sdk')
const testDesignV1 = require('../designs/test_design_1.0.0.js')

module.exports = {
  async migrateAsync ({serializedLivingdoc, metadata, systemdata} = {}) {
    // You can work directly on the JSON or create a livingdoc instance from
    // it and use the livingdoc API. We do the latter in this example.
    const livingdoc = liSDK.document.create({
      content: serializedLivingdoc.content,
      design: testDesignV1
    })

    // We get the first header component in the document
    const headers = livingdoc.componentTree.find('header')
    const firstHeader = _.first(headers)
    if (firstHeader) {
      // And from this first header component we get the title directive and assign
      // its value to the metadata field "title"
      const titleDirective = firstHeader.directives.get('title')
      metadata.title = titleDirective.getContent()
    }

    // return the modified serializedLivingdoc and metadata
    // those will overwrite the ones you got as an input and the migration is
    // done.
    return {serializedLivingdoc: livingdoc.serialize(), metadata}
  }
}
```
