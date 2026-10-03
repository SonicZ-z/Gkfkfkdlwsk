# OneHub Documentation

A simple guide for building a clean, modern, and scroll-friendly "OneHub" website or dashboard. This README includes useful functions, layout ideas, and ready-to-use sample code you can copy into your project.

## What is OneHub?

OneHub is a concept for a single-page website or landing page that brings together:

- Hero section
- Feature highlights
- About / overview
- Product or service cards
- Testimonial or feedback
- FAQ section
- Contact or CTA section
- Smooth scrolling navigation

The goal is to make the page feel professional, easy to navigate, and visually appealing.

---

## Core Functions You Can Use

These are the most useful functions for building a good OneHub page.

### 1. Smooth scroll to sections
Use this to make the page move smoothly between sections.

```css
html {
  scroll-behavior: smooth;
}
```

### 2. Navigation menu setup
This lets users click a menu item and jump to different parts of the page.

```html
<nav class="navbar">
  <a href="#home">Home</a>
  <a href="#features">Features</a>
  <a href="#about">About</a>
  <a href="#contact">Contact</a>
</nav>
```

### 3. Highlight active section while scrolling
This helps users know where they are on the page.

```js
const sections = document.querySelectorAll('section');
const navLinks = document.querySelectorAll('.navbar a');

const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      navLinks.forEach((link) => {
        const href = link.getAttribute('href');
        link.classList.toggle('active', href === '#' + entry.target.id);
      });
    }
  });
}, { threshold: 0.5 });

sections.forEach((section) => observer.observe(section));
```

### 4. Reveal content on scroll
This gives a modern animated effect when sections come into view.

```js
const revealItems = document.querySelectorAll('.reveal');

const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    }
  });
}, { threshold: 0.2 });

revealItems.forEach((item) => revealObserver.observe(item));
```

```css
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

### 5. Sticky navigation bar
Useful for giving the site a professional layout.

```css
.navbar {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: rgba(15, 23, 42, 0.9);
  backdrop-filter: blur(10px);
}
```

### 6. CTA button function
A button that calls someone to action. Good for signup, contact, download, or booking.

```js
function scrollToContact() {
  document.getElementById('contact').scrollIntoView({ behavior: 'smooth' });
}
```

### 7. Scroll to top button
Useful when a page is long.

```js
const topBtn = document.getElementById('scrollTop');

window.addEventListener('scroll', () => {
  if (window.scrollY > 300) {
    topBtn.style.display = 'block';
  } else {
    topBtn.style.display = 'none';
  }
});

topBtn.addEventListener('click', () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
});
```

### 8. Feature card generator
Use this function to generate cards dynamically.

```js
const features = [
  { title: 'Fast Setup', text: 'Launch quickly and start building right away.' },
  { title: 'Modern Design', text: 'Create a clean and professional interface.' },
  { title: 'Flexible Layout', text: 'Adjust content and sections for your brand.' }
];

function renderFeatures() {
  const container = document.getElementById('features');

  features.forEach((feature) => {
    const card = document.createElement('div');
    card.className = 'feature-card reveal';
    card.innerHTML = `
      <h3>${feature.title}</h3>
      <p>${feature.text}</p>
    `;
    container.appendChild(card);
  });
}

renderFeatures();
```

### 9. Menu toggle for mobile devices
This is useful for small screens.

```html
<button class="menu-toggle">☰</button>
```

```js
const toggle = document.querySelector('.menu-toggle');
const nav = document.querySelector('.navbar');

toggle.addEventListener('click', () => {
  nav.classList.toggle('open');
});
```

```css
.navbar.open {
  display: block;
}

@media (max-width: 768px) {
  .navbar {
    display: none;
  }
}
```

---

## Sample OneHub Page

Below is a complete example you can use as a starting point.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>OneHub</title>
    <style>
      * { box-sizing: border-box; }
      html { scroll-behavior: smooth; }

      body {
        margin: 0;
        font-family: Arial, sans-serif;
        background: #0f172a;
        color: #e2e8f0;
      }

      .navbar {
        position: sticky;
        top: 0;
        display: flex;
        justify-content: center;
        gap: 30px;
        background: rgba(15, 23, 42, 0.9);
        padding: 18px;
        border-bottom: 1px solid rgba(255,255,255,0.1);
      }

      .navbar a {
        color: #e2e8f0;
        text-decoration: none;
        font-weight: bold;
      }

      .navbar a.active {
        color: #38bdf8;
      }

      section {
        padding: 80px 20px;
        max-width: 1100px;
        margin: 0 auto;
      }

      .hero {
        min-height: 80vh;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
      }

      .hero h1 {
        font-size: 3rem;
        margin-bottom: 20px;
      }

      .hero p {
        font-size: 1.2rem;
        max-width: 700px;
        margin: 0 auto 30px;
      }

      .btn {
        background: #38bdf8;
        color: #082f49;
        border: none;
        padding: 14px 24px;
        border-radius: 12px;
        font-weight: bold;
        cursor: pointer;
      }

      .feature-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
        gap: 20px;
      }

      .feature-card {
        background: #111827;
        padding: 20px;
        border-radius: 16px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.2);
      }

      .reveal {
        opacity: 0;
        transform: translateY(30px);
        transition: all 0.8s ease;
      }

      .reveal.visible {
        opacity: 1;
        transform: translateY(0);
      }

      .scroll-top {
        position: fixed;
        right: 20px;
        bottom: 20px;
        display: none;
        background: #38bdf8;
        color: #082f49;
        border: none;
        padding: 12px 16px;
        border-radius: 50%;
        cursor: pointer;
      }
    </style>
  </head>
  <body>
    <nav class="navbar">
      <a href="#home" class="active">Home</a>
      <a href="#features">Features</a>
      <a href="#about">About</a>
      <a href="#contact">Contact</a>
    </nav>

    <section id="home" class="hero">
      <div>
        <h1>Welcome to OneHub</h1>
        <p>Build a modern one-page experience with smooth scrolling, stylish features, and clear messaging.</p>
        <button class="btn" onclick="document.getElementById('contact').scrollIntoView({behavior:'smooth'})">Get Started</button>
      </div>
    </section>

    <section id="features">
      <h2>Features</h2>
      <div class="feature-grid" id="featureList"></div>
    </section>

    <section id="about" class="reveal">
      <h2>About</h2>
      <p>
        OneHub is a simple structure for product pages, portfolios, agencies, or startup landing pages.
        It gives users a smooth experience and clean visual presentation.
      </p>
    </section>

    <section id="contact" class="reveal">
      <h2>Contact</h2>
      <p>Need help building your website? Contact us today and let’s create something amazing.</p>
      <button class="btn">Email Us</button>
    </section>

    <button class="scroll-top" id="scrollTop">↑</button>

    <script>
      const features = [
        { title: 'Fast Setup', text: 'Easy to build and launch.' },
        { title: 'Scrolling Layout', text: 'Smooth sections and modern navigation.' },
        { title: 'Responsive Design', text: 'Looks good on desktop and mobile.' }
      ];

      const featureList = document.getElementById('featureList');
      features.forEach((feature) => {
        const card = document.createElement('div');
        card.className = 'feature-card reveal';
        card.innerHTML = `<h3>${feature.title}</h3><p>${feature.text}</p>`;
        featureList.appendChild(card);
      });

      const sections = document.querySelectorAll('section');
      const navLinks = document.querySelectorAll('.navbar a');

      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            navLinks.forEach((link) => {
              const href = link.getAttribute('href');
              link.classList.toggle('active', href === '#' + entry.target.id);
            });
          }
        });
      }, { threshold: 0.5 });

      sections.forEach((section) => observer.observe(section));

      const revealItems = document.querySelectorAll('.reveal');
      const revealObserver = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
          }
        });
      }, { threshold: 0.2 });

      revealItems.forEach((item) => revealObserver.observe(item));

      const topBtn = document.getElementById('scrollTop');
      window.addEventListener('scroll', () => {
        topBtn.style.display = window.scrollY > 300 ? 'block' : 'none';
      });

      topBtn.addEventListener('click', () => {
        window.scrollTo({ top: 0, behavior: 'smooth' });
      });
    </script>
  </body>
</html>
```

---

## Best Practices for a Good OneHub Page

- Keep the design clean and uncluttered
- Use a big hero section with a clear message
- Add smooth scrolling to every section
- Use cards for features and benefits
- Keep CTA buttons obvious and visible
- Add animations gently so the page feels modern
- Make it mobile responsive
- Keep the text short and readable

---

## Suggested File Structure

```text
project/
├── index.html
├── style.css
├── script.js
└── README.md
```

---

## Quick Start

1. Create an `index.html` file
2. Add the sections you want: home, features, about, contact
3. Add CSS for layout and design
4. Add JS for scroll effects and animations
5. Test it in the browser

---

## Final Tip

Use a smooth scrolling layout with clear section anchors, attractive cards, an eye-catching hero, and a simple color theme. That combination makes a OneHub page feel professional and easy to use.

If you want, I can also create a more advanced version of this README for a specific stack such as:

- HTML + CSS + JavaScript
- React
- Next.js
- Tailwind CSS
- Bootstrap


