# 11. UI Design Specification (Revised)

A **niche, category-reactive music player** — the whole UI tints itself to match the
genre of the currently playing track. Not another generic "Spotify-dark" clone: the
app has a point of view, and the color of that point of view shifts with the music.

## 11.1 Design Language

| Element         | Specification |
|-----------------|---------------|
| Aesthetic       | Editorial / zine-inspired. Sharp typography, generous whitespace, asymmetric grids. |
| Base canvas     | Near-black `#0B0B10` with a soft radial glow that takes the current category's accent color. |
| Surface layers  | Frosted glass panels (`backdrop-blur-xl`, 8–12% white tint) over the tinted canvas. |
| Typography      | Display: **Space Grotesk** or **Clash Display** (headings, now-playing track title). Body: **Inter**. Numerics: tabular-nums for time codes. |
| Grid            | 12-col desktop, 4-col mobile. Song rows use a wide asymmetric grid — cover art is oversized relative to metadata to feel gallery-like. |
| Album art       | Rounded 12px, 1px inner border that inherits the accent color at 30% opacity. Fallback is a generative gradient seeded from the song's `id`. |
| Motion          | 220ms ease-out on accent transitions. Player bar background cross-fades (not instant-swaps) when the category changes. |
| Hover           | Song rows scale 1.01x and reveal a thin accent-colored left border (3px). Play buttons use an accent-to-white radial on hover. |

## 11.2 Category-Reactive Theming

The accent color, glow, waveform tint, progress bar, and hover highlights all derive
from a single `--accent` CSS variable that is updated whenever `currentSong.genre`
changes.

### Palette (per category)

| Genre / Category   | Accent (primary) | Glow (secondary) | Mood        |
|--------------------|------------------|------------------|-------------|
| Electronic / EDM   | `#06B6D4` cyan   | `#8B5CF6` violet | neon, cold  |
| Hip-Hop / Rap      | `#F59E0B` amber  | `#DC2626` crimson| warm, punchy|
| Rock / Metal       | `#EF4444` red    | `#1F1F29` graphite| raw, loud  |
| Pop                | `#EC4899` pink   | `#FBBF24` yellow | bright, hi-key |
| Jazz / Blues       | `#B45309` bronze | `#1E3A8A` deep blue | smoky, late-night |
| Classical          | `#C7A15B` gold   | `#0F172A` midnight ink | stately |
| Indie / Folk       | `#65A30D` moss   | `#F5F1E8` parchment | earthy, acoustic |
| Lo-fi / Ambient    | `#A78BFA` lavender| `#155E75` teal  | soft, hazy  |
| R&B / Soul         | `#7C3AED` purple | `#F472B6` rose   | velvety     |
| Unknown / Default  | `#E5E7EB` off-white | `#1F2937` slate | neutral  |

### Implementation sketch

```js
// store/useThemeStore.js
const GENRE_THEMES = {
  electronic: { accent: '#06B6D4', glow: '#8B5CF6' },
  hiphop:     { accent: '#F59E0B', glow: '#DC2626' },
  // ...
  default:    { accent: '#E5E7EB', glow: '#1F2937' },
};

function applyTheme(genre) {
  const t = GENRE_THEMES[normalize(genre)] ?? GENRE_THEMES.default;
  document.documentElement.style.setProperty('--accent', t.accent);
  document.documentElement.style.setProperty('--glow',   t.glow);
}
```

```jsx
// subscribe in the player
useEffect(() => applyTheme(currentSong?.genre), [currentSong?.id]);
```

Tailwind reads the vars via `theme.extend.colors.accent = 'var(--accent)'`, so
`bg-accent`, `text-accent`, `ring-accent` all react automatically — no component-
level theming code.

## 11.3 Layout

Three-panel, but opinionated:

- **Left rail (220px)** — Playlists. Collapses to a 64px icon rail at <1024px, then off-canvas drawer at <640px.
- **Center stage** — Scrollable. Switches between `SongList` and a `NowPlaying` hero view. The hero shows oversized album art with the accent glow spilling behind it.
- **Bottom player (fixed, 88px)** — Full-width, `backdrop-blur-xl`, tinted with `--accent` at 8% opacity. Waveform scrubber (not a plain progress bar) rendered in the accent color.

## 11.4 Niche Touches

- **Generative fallback covers** — when `cover_art_url` is null, render a WebGL/Canvas gradient seeded by `song.id`, tinted by the genre palette. Every song always has art.
- **Ambient glow** — a large, low-opacity radial-gradient blob follows the cursor on the Now Playing view, colored by `--glow`. Disabled via `prefers-reduced-motion`.
- **Genre pill** — small capsule next to the track title, filled with `--accent` at 20% and outlined at 100%. Doubles as a filter button.
- **Transition rule** — when the genre changes between tracks, the accent crossfades over 220ms; within the same genre, it snaps instantly.
- **Accessibility** — every accent is paired with a guaranteed ≥4.5:1 foreground. Never rely on accent alone to convey state (pair with icon or text).
