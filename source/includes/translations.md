# Translations

Some endpoints in V3 of the API have translations available under the `translations` key, which is an array of all the translations available for that resource.

| Attribute | Description |
| --------- | ----------- |
| `locale` | The locale code for the set of translations. This is generally an [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) code, or where translations are available for the same language across multiple countries or regions, a [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag) code is used |
| *`<attribute>`* | Each translated attribute has a key - for example, if the `name` attribute of a resource has translations available, there will be a `name` attribute in each translation object |

Note that GPS' native locale is British English (`en-GB`) and will not be present in any `translations` array.

Not all locales may be completed for all resources, but generally all attributes within a locale should be complete.

The following resources have translations available:

| Resource | Attributes |
| -------- | ---------- |
| [Category](#categories-list-categories-v3) | `name` |

An example payload for a fictitious resource is given on the right (below on mobile).

```json
{
  "name": "name in British English",
  "description": "description in British English",
  "translations": [
    {
      "locale": "es",
      "name": "name in Spanish",
      "description": "description in Spanish"
    },
    {
      "locale": "fr",
      "name": "name in French",
      "description": "description in French"
    }
    ...
  ]
}
```
