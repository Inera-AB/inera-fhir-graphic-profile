# inera-fhir-graphic-profile

IG Publisher template with Inera's graphic profile for Inera FHIR Implementation Guides. It builds on HL7's `fhir2.base.template` and adds only styling: no scripts.

An IG uses it in `ig.ini`:

```ini
[IG]
ig = fsh-generated/resources/ImplementationGuide-<id>.json
template = https://github.com/Inera-AB/inera-fhir-graphic-profile/tree/v1
```

IG Publisher downloads the template from GitHub on every build, locally and in CI. A change released here reaches every IG on `v1` at its next build. Published versions of an IG are never rebuilt, so they keep the look they were published with.

IGs created from [IneraFHIRTemplate](https://github.com/Inera-AB/IneraFHIRTemplate) use it from the start.

## Contents

| Path | What it is |
|---|---|
| `package/package.json` | Template id `inera.fhir.template`, type `fhir.template`, base `fhir2.base.template#current` |
| `includes/fragment-css.html` | Loads `inera.css` and the favicon. Fills the base template's placeholder for child template CSS |
| `includes/fragment-header.html` | The Inera logo above the IG's menu. Fills the base template's header placeholder |
| `content/assets/css/inera.css` | Colours, fonts and layout |
| `content/assets/images/` | Logo and favicon |

An IG should not have its own `input/includes/fragment-css.html` or `fragment-header.html`: they replace the template's files, and the IG loses the Inera styling. The IG's own `input/includes/menu.xml` is unaffected.

## Versions

IGs refer to a major version: `tree/v1`. `v1` is a moving tag that points to the latest `v1.x.y` release, as in [fhir-ig-workflows](https://github.com/Inera-AB/fhir-ig-workflows).

- A fix or a backwards-compatible change: set `version` in `package/package.json`, tag `v1.x.y` and move `v1` to it.
- A change that needs IGs to change (renamed files, a different base template): release as `v2`. IGs move to `tree/v2` when they are ready.

Before moving `v1`, test the change on an IG branch with `template = https://github.com/Inera-AB/inera-fhir-graphic-profile/tree/<branch>`, or with a local clone: `template = ../inera-fhir-graphic-profile`. The branch name must not contain `/`: IG Publisher splits the URL on slashes.

## Restrictions

IG Publisher treats a template as active content if it has ant scripts or files outside a fixed list of types (for example `.js`). Active content only runs for templates on HL7's approved list when an IG is built on HL7's auto-build. This template has neither, and Inera's IGs build in GitHub Actions, so the restriction does not apply. Keep it that way: styling, HTML fragments and images only.
