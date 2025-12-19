# EarCare Landing Page - AI Coding Instructions

## Project Overview
Single-page ayurvedic product landing page (EarCare Capsule - Vs5 Swelling) with Hindi/English bilingual content, fixed mobile-first design (max-width: 450px), and conversion-focused UI. No backend—form submissions store data in localStorage and redirect to a thank-you page.

## Architecture & Key Files

### Core Files
- **[index.html](../index.html)**: Main landing page (2318 lines) - hero, problems, product showcase, benefits, testimonials, FAQ, contact form
- **[ThankYou.html](../ThankYou.html)**: Post-submission confirmation page (235 lines) - displays customer name from localStorage
- **Image/**: Product images (Hero.avif, ProductShowcase.avif), customer testimonials (customer1.avif, customer2.avif)

### Design System
All styles are inline in `<head>` using CSS custom properties and Tailwind Browser CDN:
```css
:root {
    --brand-primary: #14B8A6;    /* Teal - main CTAs */
    --brand-secondary: #0D9488;   /* Darker teal - gradients */
    --brand-light: #CCFBF1;       /* Light teal - backgrounds */
    --brand-muted: #99F6E4;       /* Muted teal - borders */
    --brand-accent: #134E4A;      /* Dark teal - text */
}
```

**Fonts**: Google Fonts via CDN
- `font-arya`: UI elements, buttons, CTAs
- `font-inknut`: Headlines, hero text
- `font-yantra`: Body text, descriptions

## Critical Development Patterns

### 1. Mobile-First Container Pattern
**ALL content must be wrapped in**:
```html
<div class="main-container">
    <!-- Content here -->
</div>
```
- Desktop: `max-width: 450px` (centered)
- Mobile: `max-width: 100%`
- Never exceed container width for proper mobile display

### 2. Form Submission Flow
Form in [index.html](../index.html#L1991):
```javascript
<form id="contactForm" onsubmit="return showThankYou(event)">
```
- Validates name and phone (10-15 digits)
- Stores data in `localStorage.setItem('customerOrder', JSON.stringify(formData))`
- Redirects to `ThankYou.html` (NOT server submission)
- **No backend/API integration** - client-side only

ThankYou page retrieves data:
```javascript
const customerData = localStorage.getItem('customerOrder');
const parsedData = JSON.parse(customerData);
document.getElementById('customerName').textContent = parsedData.customerName;
```

### 3. Meta Pixel Tracking
[ThankYou.html](../ThankYou.html#L6-L27) includes Facebook Pixel:
- `fbq('init', '2474768816312806')`
- Fires events: `PageView`, `SubmitLead`, `Lead`, `Purchase` (₹1999.00 INR)
- **Important**: Do not remove or modify pixel code

### 4. Fixed CTA Bar
Bottom-fixed CTA in [index.html](../index.html#L2248-L2293):
- Always visible with countdown timer (resets at midnight IST)
- Width matches main container (`450px`)
- Includes: promotional text, timer, "Order Now" button
- `onclick="scrollToContact()"` scrolls to `#contact` section

### 5. Interactive Components

**FAQ Accordions** ([index.html](../index.html#L2134-L2200)):
```javascript
function toggleFAQ(element) {
    // Closes other FAQs, toggles clicked one
    // Adds .active class to question and answer
    // Rotates chevron icon
}
```

**Countdown Timer**:
```javascript
function updateCountdown() {
    // Calculates time until midnight
    // Updates #offer-countdown and #cta-countdown
    // Runs every 1000ms
}
```

### 6. CSS Animation Patterns
Hover effects disabled on touch devices:
```css
@media (hover: hover) and (pointer: fine) {
    .creative-logo:hover {
        transform: scale(1.1) rotate(12deg);
    }
}
```

Common animations:
- `.fade-in-up`: Section entry animation
- `.slide-in-left/right`: Testimonial cards
- `@keyframes pulse`: Money-back badge
- `.wave`: Sound wave animation in logo

## Common Modifications

### Adding New Sections
1. Wrap in `<section class="px-4 py-2">`
2. Add decorative divider after:
```html
<div class="my-4 h-2 max-[450px]:my-3" 
     style="background: linear-gradient(to right, transparent, var(--brand-primary), transparent);">
</div>
```
3. Use responsive classes: `text-3xl max-[450px]:text-2xl`

### Responsive Text Pattern
Always include mobile breakpoint:
```html
<h2 class="text-3xl max-[450px]:text-2xl">...</h2>
<p class="text-lg max-[450px]:text-base">...</p>
```

### Button Styles
Primary CTA button pattern:
```html
<button class="bg-brand-primary text-white font-bold py-4 px-10 rounded-xl text-xl 
               font-arya cursor-pointer transition-all duration-300 
               hover:bg-brand-secondary hover:scale-105 shadow-lg 
               max-[450px]:py-3 max-[450px]:px-8 max-[450px]:text-lg">
    ✨ Button Text ✨
</button>
```

## Bilingual Content Strategy
- Primary language: Hindi (Devanagari script)
- English technical terms in parentheses: "संक्रमण (infection)"
- Emoji usage: Heavy for visual appeal and breaking language barriers
- Voice: Empathetic, problem-solution focused, direct address ("आप")

## No Build Process
- Pure HTML/CSS/JS - open `index.html` directly in browser
- Tailwind via CDN (`@tailwindcss/browser@4`)
- No npm, no compilation, no deployment steps
- Test changes by refreshing browser

## Testing Checklist
- [ ] Mobile view (max 450px width) - primary target
- [ ] Desktop centered container
- [ ] Form validation (10-15 digit phone)
- [ ] localStorage → ThankYou page data flow
- [ ] Fixed CTA bar visibility
- [ ] Countdown timer accuracy
- [ ] FAQ toggle functionality
- [ ] Smooth scroll to #contact section

## Project Context
**Version**: Vs5 (Swelling) - focuses on ear inflammation/swelling
Product: Ayurvedic ear care capsules for pain, infection, hearing issues
Target audience: Hindi-speaking users with ear problems seeking natural solutions
