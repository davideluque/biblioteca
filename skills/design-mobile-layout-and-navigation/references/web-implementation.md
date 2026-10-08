# Web implementation notes

How the decisions in the skill translate to HTML and CSS for mobile web layouts.

## Viewport and safe areas

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

- **`viewport-fit=cover`** lets the page extend under the notch and home indicator. Once you use it, you must pad content back inside the safe areas yourself.
- **Never add `user-scalable=no` or a low `maximum-scale`.** They block people who need to zoom, and modern iOS Safari ignores them anyway (MDN).

Pad fixed and sticky bars with the safe-area insets. Give a fallback, because the insets are 0 on rectangular screens:

```css
.bottom-bar {
  position: sticky;
  bottom: 0;
  padding-bottom: calc(0.75rem + env(safe-area-inset-bottom, 0px));
}

.top-bar {
  padding-top: env(safe-area-inset-top, 0px);
}
```

## On-screen keyboard

The viewport meta tag's `interactive-widget` key controls what the on-screen keyboard resizes (MDN):

| Value | Effect |
| --- | --- |
| `resizes-visual` (default) | Layout stays the same; only the visible area shrinks |
| `resizes-content` | The layout viewport and viewport units shrink, so bottom bars move above the keyboard |
| `overlays-content` | Nothing resizes; the keyboard covers content |

Browser support differs, so test on real devices. Whatever the setting, the focused field and its submit button must stay visible while typing.

## Sticky bars and focus

A sticky header or footer must not fully cover the element that has keyboard focus (WCAG 2.4.11). Reserve its height with `scroll-padding`, so the browser scrolls focused elements clear of the bar. Also add the same amount of real padding at the end of the page. Without it, the last controls can't scroll far enough to clear a fixed bottom bar (WCAG technique C43):

```css
html {
  scroll-padding-top: var(--header-height);
  scroll-padding-bottom: var(--bottom-bar-height);
}

body {
  padding-bottom: var(--bottom-bar-height);
}
```

On very small or very zoomed viewports, let bars scroll away so content has room (WCAG 1.4.10 advisory technique):

```css
@media (max-height: 480px) {
  .top-bar, .bottom-bar { position: static; }
}
```

Cookie banners and other overlays that don't take focus should either be modal or close when focus moves past them.

## Navigation markup

Site navigation is a list of links inside `<nav>`, not an ARIA `menu`. That role is for application menus with arrow-key behaviour (Heydon Pickering, Inclusive Components).

```html
<nav aria-label="Main">
  <button type="button" aria-expanded="false" aria-controls="main-links">Menu</button>
  <ul id="main-links">
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/orders">Orders</a></li>
    <li><a href="/account">Account</a></li>
  </ul>
</nav>
```

- Collapse the list from script on page load (set `hidden` on it), not in the HTML, so the links stay reachable if the script fails.
- Keep the toggle button inside the `<nav>`. When it opens or closes the menu, change `aria-expanded` and the list's `hidden` attribute together, so the announced state always matches what is visible:

  ```js
  button.addEventListener("click", () => {
    const open = button.getAttribute("aria-expanded") === "true";
    button.setAttribute("aria-expanded", String(!open));
    list.hidden = open;
  });
  ```
- Mark the current page with `aria-current="page"`, and show it with more than colour alone.
- At wide widths, hide the toggle and remove `hidden` from the list, so the links are always visible.

## Target size

Size the tappable element, not just the icon inside it:

```css
.icon-button {
  min-inline-size: 44px;
  min-block-size: 44px;
  display: inline-grid;
  place-items: center;
}
```
