# Copilot Instructions for my-portfolio

## Project Overview
Personal portfolio for Yuvaraj Ramasamy, showcasing projects and experience. It's a responsive, single-page application with **bilingual support (English/Hindi)** and **dual theme support (dark/light)** using vanilla JavaScript, CSS, and Tailwind CSS.

**Key Files:**
- [index.html](../index.html) - Main HTML structure with semantic sections (#home, #about, #projects, #contact, etc.)
- [js/script.js](../js/script.js) - Core app logic (~965 lines); single `AppState` object manages all state
- [css/style.css](../css/style.css) - Primary stylesheet (~2,669 lines) with design system via CSS variables

---

## Architecture & State Management

### AppState Pattern
The app uses a centralized state object in [js/script.js](../js/script.js#L1-L6):
```javascript
const AppState = {
    currentLang: 'en',
    currentTheme: 'dark',
    currentSection: 'home',
    isMenuOpen: false,
    isLoaded: false
};
```
**Lifecycle:** `DOMContentLoaded` → `initializeApp()` → sets AppState → all features initialized  
**Persistence:** Theme & language saved to `localStorage` with keys `portfolio-theme` and `portfolio-lang`

### Key Initialization Flow
1. `loadPreferences()` - Hydrates AppState from localStorage
2. Language and theme systems initialized separately
3. Navigation, form handlers, mobile menu attached
4. Scroll effects and IntersectionObserver for fade-in animations

---

## Design System & Styling

### CSS Variables Architecture
All colors/spacing defined in `:root` ([css/style.css](../css/style.css#L1-L36)):
- **Colors:** `--primary`, `--secondary`, `--accent`, `--cyan`, `--purple`, `--green`
- **Backgrounds:** `--bg-dark`, `--bg-darker`, `--bg-card`, `--bg-hover`
- **Typography:** `--font-primary` (Tajawal), `--font-code` (Fira Code)
- **Gradients:** `--gradient-1`, `--gradient-2`, `--gradient-3`
- **Transitions:** `--transition`, `--transition-fast`, `--transition-smooth`, `--transition-bounce`

### Theme System
- **Dark theme (default):** High contrast with dark backgrounds (#0f172a, #020617)
- **Light theme:** Applied via `[data-theme="light"]` selector with white/gray palette
- Toggle theme: Add `data-theme="light"` to `<body>` → CSS variables auto-switch

### Component Patterns
- **Project Cards** (`.project-card`): Gradient borders, glow shadows, hover scale effects
- **Contact Items** (`.contact-item`): Animated gradient borders, shimmer on hover
- **Contact Form** (`.contact-form`): Radial gradient bg, gradient input borders, enhanced focus states
- **Alert Boxes** (`.alert`): `.alert-success`, `.alert-warning`, `.alert-danger`, `.alert-info` with icon support
- **Fade-in Elements** (`.fade-in`): Animated on scroll via IntersectionObserver

---

## Internationalization (i18n)

### Pattern: Data Attributes
All translatable text uses dual attributes on HTML elements:
```html
<span data-text-en="Home" data-text-hi="होम">Home</span>
<input data-placeholder-en="Name" data-placeholder-hi="नाम" />
```

### Function: `updateLanguageUI()`
Iterates all `[data-text-en], [data-text-hi]` and `[data-placeholder-en], [data-placeholder-hi]` elements, replacing text/attributes based on `AppState.currentLang`.

### Language Toggle
- Button ID: `#langToggle` with `.lang-text` child showing "EN" or "HI"
- `toggleLanguage()` switches between 'en'/'hi', persists to localStorage
- Updates `<html lang>` and `<body data-lang>` attributes

---

## Navigation & Scroll Behavior

### Navigation Links
- `.nav-link` elements with `data-section` attribute match section IDs (#home, #about, etc.)
- Click handler: smooth scroll to target section accounting for fixed header height
- Auto-close mobile menu after navigation
- Active state updates via `updateActiveNavLink()` as user scrolls past sections

### Scroll Progress Indicator
Dynamically created in `initScrollEffects()`:
- CSS class: `.scroll-progress` (fixed top bar with linear gradient)
- Width updates on scroll as percentage of total page height

### Mobile Menu
- Toggle via `#menuToggle` button (hamburger icon)
- `AppState.isMenuOpen` tracks state
- `#navMenu` slides in/out with CSS transitions

---

## Form Handling & Validation

### Contact Form
- ID: `#contactForm`
- Real-time validation via input/blur listeners on `.form-input` elements
- `validateFormInput()` function checks field validity
- `handleFormSubmit()` processes submission (implementation context in [js/script.js](../js/script.js#L300+))

### Validation Pattern
- Attach listeners to `.form-input` elements
- Update UI with `.valid`/`.invalid` CSS classes
- Show errors/success states inline

---

## Responsive Design

### Breakpoints & Approach
Mobile-first approach via [css/responsive.css](../css/responsive.css) overrides:
- Base styles in [css/style.css](../css/style.css) apply to all screens
- Responsive CSS provides tablet/desktop adjustments
- Flexbox/Grid layouts adapt from single-column to multi-column

### Key Files
- [RESPONSIVE_GUIDE.md](../RESPONSIVE_GUIDE.md) - Detailed breakpoint documentation
- [css/responsive-mobile-first.css](../css/responsive-mobile-first.css) - Alternative mobile-first architecture

---

## Development Conventions

### Component Conventions
- **Naming:** BEM-style classes (`.project-card`, `.project-card__title`, `.project-card--featured`)
- **State:** Prefix with data attributes (`data-theme`, `data-lang`, `data-section`)
- **CSS Classes:** Reuse existing classes; avoid inline styles

### JavaScript Patterns
- **Selectors:** Use `querySelector()` / `querySelectorAll()` consistently
- **Error Handling:** Wrap sensitive operations in try-catch (e.g., `toggleLanguage()`)
- **Event Delegation:** Attach listeners to static parents when possible
- **Comments:** Use when "why" is non-obvious; code style is self-documenting

### CSS Maintenance
- Variables first: Always use CSS custom properties for colors, spacing, transitions
- Gradients: Reference `--gradient-*` variables, not inline gradients
- Media queries: Group by breakpoint, not by element
- Avoid:** !important flags, deep nesting (max 3 levels)

---

## Documentation Files

- [QUICK_START.md](../QUICK_START.md) - Usage examples for alert boxes, cards, form components
- [STYLE_ENHANCEMENTS.md](../STYLE_ENHANCEMENTS.md) - Detailed changelog of style upgrades
- [ENHANCEMENT_SUMMARY.md](../ENHANCEMENT_SUMMARY.md) - Overview of recent enhancements
- [RESPONSIVE_GUIDE.md](../RESPONSIVE_GUIDE.md) - Responsive design patterns and breakpoints
- [VISUAL_GUIDE.md](../VISUAL_GUIDE.md) - Visual component reference
- [STYLE_PREVIEW.html](../STYLE_PREVIEW.html) - Interactive preview of all component styles

---

## Common Tasks

### Add a New Section
1. Add `<section id="new-section">` to [index.html](../index.html)
2. Add navigation link: `<a href="#new-section" class="nav-link" data-section="new-section">`
3. Include bilingual text: `<span data-text-en="Label" data-text-hi="लेबल">Label</span>`
4. Scroll handler in [js/script.js](../js/script.js#L177-L192) auto-activates nav link on scroll

### Modify Theme Colors
Edit CSS variables in `:root` ([css/style.css](../css/style.css#L1-L36)) and `[data-theme="light"]` ([css/style.css](../css/style.css#L38-L46))  
All components automatically update due to CSS variable cascade.

### Add i18n Text
1. Add `data-text-en="..."` and `data-text-hi="..."` attributes to element
2. `updateLanguageUI()` picks up changes on next language toggle
3. No JS changes needed—purely HTML attribute-driven

### Style New Components
1. Define in [css/style.css](../css/style.css) using existing CSS variables
2. Reference [STYLE_ENHANCEMENTS.md](../STYLE_ENHANCEMENTS.md) for gradient/shadow patterns
3. Add media queries to [css/responsive.css](../css/responsive.css) for mobile adjustments

---

## External Dependencies

- **Tailwind CSS** (CDN): `https://cdn.tailwindcss.com` - Utility classes for layout
- **Font Awesome** (CDN): `https://cdnjs.cloudflare.com/.../all.min.css` - Icon library
- **Google Fonts:** Tajawal (primary), Fira Code (monospace)

---

## Testing & Validation

- **Responsiveness:** Test at 320px (mobile), 768px (tablet), 1200px+ (desktop)
- **Themes:** Toggle dark/light via `[data-theme]` attribute and verify color contrast
- **Languages:** Switch via language toggle and verify all `data-text-*` attributes render
- **Accessibility:** Check focus states, color contrast ratios, semantic HTML
- **Performance:** Verify loader screen, smooth scrolling, no layout shifts

---

## Key Decision Rationale

- **No Framework:** Vanilla JS for simplicity and portfolio demonstration; no build step needed
- **CSS Variables:** Enables dynamic theming without CSS-in-JS complexity
- **Data Attributes:** i18n via HTML attributes keeps translation logic decoupled from JS
- **IntersectionObserver:** Efficient scroll animations vs. constant scroll event listeners
- **CDN Dependencies:** Speeds up initial load; Tailwind for rapid prototyping
