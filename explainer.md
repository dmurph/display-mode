# Display Mode Override Proposal

## Background
Display modes in the manifest currently 
XXX: current state, hard to have a fallback that is not browser, new display modes don't have good fallbacks, back button inconsistancy

## Objective

Create new display mode API that balances the following objectives:
 * Acceptable fallback behavior - Fallback behavior is acceptable to web developers, and doesn't force them back into 'browser'
 * Developer control - Allow developers to sufficiently control the webapp as they want
 * Simplicity - not overly complicated

## Proposal
tl;dr:
 * Add a new field, sequence of strings `"display-override"`, which the user agent considers before the `"display"` field. These are considered in-order, and the first supported display mode is applied. If none are suppored, the user agent falls back to `"display"`.
 * Add a media queries for browser controls. Back button is the only one currently proposed.

Example:
```json
{
  "display": "standalone",
  "display-override": ["customized", "minimal-ui"],
}
```
In this example, the developer prefers `"customized"`, and if not supported then `"minimal-ui"`. If neither of those are supported they want to fall back to a `"standalone"` window.

New modes would only be allowed to be specified in the "display-override" field.

### Pros

#### Predictable Fallback
This proposal allows developers to request the specific display modes they want to support, and in the fallback order they want to display.

#### User Agent Support Simplicity
This lets the user agent support a fixed list of chosen display options. A proposal like display-modifiers creates a cross product of possible configuration which is very difficult for a UI team to support well, especially for every new feature.

#### Avoids most 'feature' detection
When a user agent supports a mode, that is predictable and fully supported. If it does not, it appropriately falls back.

#### Backwards compatible
A user-agent that doesn't support "display-override" would fall back to the "display" field, which is fine and is predictable.

### Cons

#### Possible large number of modes
If tabbed & title bar customization are allowed, then this could be the list of modes:

1. minimal
1. standalone
1. customized
1. tabbed_minimal [potentially remove this]
1. tabbed
1. tabbed_customized
1. fullscreen
1. browser

Not supported
* ~~customized_minimal~~
* ~tabbed_customized_minimal~

Even though this seems long, this is how user agents would have to implement the UI combinations anyways from a UX design perspective. These all need to be enumerated to be properly tested as well. So having them listed separately (and supported separately) seems ok.
