# Local patches to the Solar System drop

These files come from the rider's generator as `planet-distance-live*.zip`
in ~/Downloads, and every new drop OVERWRITES all three. Anything below has
to be re-applied by hand after copying a new `dist/`.

Check with:

    grep -c "LAZYMANLABS LOCAL PATCHES" styles.css   # expect 1

## 1. Detail panel scrollbar (styles.css, appended block)

**Measured 2026-09-03 at 1280x720:** the panel sits at y=196, the timebar
starts at y=642, so the panel gets 443px and already fills it — the gap
below it is 1px, there is no space to reclaim. Its content is 620px once
the MASS row and the COMPARE ITS SCALE TO box are present, so ~177px must
scroll. It did scroll; macOS overlay scrollbars are invisible until moved,
so the box read as truncated.

The patch gives `.detail` an always-visible scrollbar and
`overscroll-behavior:contain` so a wheel over the panel cannot fall through
to the map's zoom once the panel bottoms out.

**Chrome/Safari vs Firefox:** setting any standard scrollbar property
(`scrollbar-width`, `scrollbar-color`) makes Chrome ignore the
`::-webkit-scrollbar` rules completely. Measured on this page: 11px styled
bar with webkit rules alone, 2px unstyled bar as soon as either standard
property is added. So the standard properties live inside a Firefox-only
`@supports (-moz-orient:inline)` guard (verified not to match Chrome).
Do not lift them out of it.

**Do not "fix" this by shrinking max-height or moving the panel up** — the
top edge would collide with the SCALE / 3D · PRECESSING readout.
