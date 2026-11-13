# inera-fhir-graphic-profile

Inera's graphic profile for FHIR Implementation Guides: an IG Publisher template that adds Inera's colours, fonts and logo on top of HL7's `fhir2.base.template`. It holds styling only, no scripts.

## How it works

- New IGs created from [IneraFHIRTemplate](https://github.com/Inera-AB/IneraFHIRTemplate) get the Inera styling from this repo automatically.
- To change the styling for all IGs: change this repo, tag `v1.x.y` and move `v1`. Every IG picks it up on its next build, locally and in CI.
- Versions already published keep their look. They are never rebuilt.

## Use it in an IG

An IG points to this repo in `ig.ini`:

```ini
[IG]
ig = fsh-generated/resources/ImplementationGuide-<id>.json
template = https://github.com/Inera-AB/inera-fhir-graphic-profile/tree/v1
```

IG Publisher downloads the template from GitHub on every build. IGs created from IneraFHIRTemplate already have this line.

An IG created before this repo existed has its own copies of the Inera files. To switch it over:

1. In `ig.ini`, set `template = https://github.com/Inera-AB/inera-fhir-graphic-profile/tree/v1`.
2. Delete `input/includes/fragment-css.html`, `input/includes/fragment-header.html` and `input/images/inera*`.
3. If a page links to a logo, change the link to `assets/images/<file>`, for example `assets/images/inera-logo.svg`.
4. Build and check that the header logo and colours are there.

An IG must not have its own `input/includes/fragment-css.html` or `fragment-header.html`. They replace this template's files, and the Inera styling disappears. The IG's `input/includes/menu.xml` is its own and is unaffected.

## Change the styling

1. Make the change on a branch here, in `content/assets/css/inera.css` or another file under [Contents](#contents). The branch name must not contain `/`, because IG Publisher splits the URL on slashes.
2. Test it from an IG: on a branch of the IG, set `template = https://github.com/Inera-AB/inera-fhir-graphic-profile/tree/<your-branch>` in `ig.ini` and build. For a local build you can also use a clone: `template = ../inera-fhir-graphic-profile`. Don't merge that `ig.ini` change in the IG.
3. Raise `version` in `package/package.json`, for example from `1.0.0` to `1.0.1`, and merge the branch to `main`.
4. Tag the release and move `v1` to it:

   ```
   git fetch origin
   git tag -a v1.0.1 -m "v1.0.1" origin/main
   git tag -f v1 origin/main
   git push origin v1.0.1
   git push -f origin v1
   ```

Every IG on `tree/v1` uses the change from its next build.

A change that needs the IGs to change too, such as renamed or moved files or a different base template, is released as `v2` instead: tag `v2.0.0` and `v2`, and move each IG to `tree/v2` when it is ready.

## Contents

| Path | What it is |
|---|---|
| `package/package.json` | Template id `inera.fhir.template`, its version, and the base template `fhir2.base.template#current` |
| `includes/fragment-css.html` | Loads `inera.css` and the favicon |
| `includes/fragment-header.html` | The Inera logo above the IG's menu |
| `content/assets/css/inera.css` | Colours, fonts and layout |
| `content/assets/images/` | Logos and favicon. The header uses `inera-logo-header.svg`. IG pages can use the others as `assets/images/<file>`: `inera-logo.svg`, `inera-logo-official.svg`, `inera-logo-negative.svg` and `inera-logo.png` |

## Keep it to styling

IG Publisher treats a template as active content if it contains ant scripts or file types outside a fixed list, `.js` among them. On HL7's auto-build such templates only run if they are on HL7's approved list. Inera's IGs build in GitHub Actions, where the rule doesn't apply. Even so, keep this repo to CSS, HTML fragments and images.
