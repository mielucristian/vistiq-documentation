# Vistiq

A marketing, e-commerce, and services website built in [Framer](https://www.framer.com).

Vistiq combines a content site (home, about, services, blog), a shop with cart and checkout, and service packages — all assembled from a shared component library and design system so the look stays consistent across pages.

---

## Pages

| Path | Description |
|------|-------------|
| `/` | Home |
| `/about` | About the company / team |
| `/services` | Services overview |
| `/packages` | Service packages |
| `/shop` | Product listing |
| `/shop/:slug` | Individual product (CMS) |
| `/blog` | Blog listing |
| `/blog/:slug` | Individual blog post (CMS) |
| `/contact` | Contact form & info |
| `/legal/:slug` | Legal/policy pages (CMS) |
| `/404` | Not found |

Routes with `:slug` are powered by Framer CMS collections.

---

## Design system

**Typography** — Playfair Display (serif) for headings paired with Kumbh Sans for body and UI text. Inter is used for the smaller heading sizes. A full scale of text styles is defined (headings, body, links, buttons, prices, quotes, statistics).

**Color** — a neutral light-to-dark scale (`Light 100` → `Dark 2`) plus accent colors including a green, reds, and a brand "Headphone Petrol Black" tone.

These are saved as Framer color and text styles, so updating a style updates everything that uses it.

## Component library

The site is built from ~55 reusable components, including:

- **Navigation & layout:** Navbar, Menu, Footer, Logo
- **Commerce:** Shop Card, Cart 2.0, Add To Cart / Buy / Checkout buttons, Product Tabs, Recommended Product
- **Services:** Services Category cards, Services Slider, Services package, Program Line
- **Social proof:** Quotes, Testimonials ticker, 5-stars, Statistic cards, Happy Clients
- **Forms:** Contact Form, Newsletter, Search
- **Content:** Accordions, Team Slider, Team Member cards, Tags

Because components are shared across pages, editing one updates every page it appears on.

---

## Editing this project

This project is connected to the **Framer MCP server**, which lets AI agents (the Framer agent, Claude, and other MCP clients) read and modify the project directly.

If you're using an AI agent, see **[AGENTS.md](./AGENTS.md)** for the operating rules — how to read project structure, edit safely, use the design system, and avoid breaking shared components.

You can also edit everything by hand in the Framer editor as usual.

### A note on credentials

The MCP connection URL includes a private `secret=` token. **Keep it out of this repository and any shared file** — anyone with that URL can edit the project. If it has been shared, rotate it from your Framer MCP settings.

---

## Tech

- **Built with:** Framer (visual editor + CMS + hosting)
- **Agent interface:** Framer MCP server (Model Context Protocol)
