# AGENTS.md

Operating instructions for AI agents (Framer agent, Claude, or any MCP client) working on this Framer project.

This file tells an agent **how to make changes safely**. Read it before editing anything.

---

## Project at a glance

**Vistiq** — a marketing + e-commerce + services website built in Framer.

It is a multi-page published site backed by a large library of reusable components, color styles, and text styles. Most pages are assembled from these shared components, so changes to a component propagate everywhere it is used.

### Published pages

| Path | Purpose |
|------|---------|
| `/` | Home |
| `/about` | About |
| `/services` | Services overview |
| `/packages` | Service packages |
| `/shop` | Shop / product listing |
| `/shop/:slug` | Product detail (CMS) |
| `/blog` | Blog listing |
| `/blog/:slug` | Blog post (CMS) |
| `/contact` | Contact |
| `/legal/:slug` | Legal pages (CMS) |
| `/404` | Not found |

`:slug` routes are CMS-driven collection pages — do not hardcode content into them; they render collection items.

### Design pages
- `Design` — canvas for explorations and prototyping. Not published.

---

## Golden rules

1. **Read before you write.** Always call `getProjectXml` at the start of a session, then `getNodeXml` on the specific page or component before editing it. The project structure changes between sessions.
2. **Components are shared.** Editing a component (e.g. `Navbar`, `Footer`, `Button`) changes every page that uses it. If a change should be local, detach the instance (`?detached=true`) instead.
3. **Make small, incremental edits.** Call `updateXmlForNode` with only the nodes/attributes you are changing, multiple times — not one giant XML blob. This lets the user watch changes land in real time on the canvas.
4. **Never invent node IDs.** Only reference `nodeId` / `componentId` values returned by `getProjectXml`, `getNodeXml`, or a previous `updateXmlForNode` call.
5. **Use the design system.** Reuse existing color styles and text styles (below) instead of hardcoding hex values or fonts. This keeps the site consistent and theme-able.
6. **Preserve responsiveness.** Insert page sections under the root `Desktop` breakpoint node, not inside other components, so breakpoints are preserved.
7. **Don't commit secrets.** The MCP connection URL contains a `secret=` token. Never write it into any file, component, or code file.

---

## Design system

Agents should style using these tokens rather than raw values.

### Color styles (reference as `backgroundColor="/Light 100"` etc.)
Neutrals run from `/Light 100` (white) down through `/Light 98`, `/Light 90`, `/Light 70` to the dark scale `/Dark 40`, `/Dark 20`, `/Dark 12`, `/Dark 10`, `/Dark 5`, `/Dark 2`. Accents: `/Green 25`, `/Red 50`, `/Red 98`, and brand `/Headphone Petrol Black 25`.

> Note: several near-duplicate `/White 100` entries exist. Prefer `/Light 100` for white surfaces for consistency.

### Text styles (reference as `inlineTextStyle="/Heading 1"` etc.)
- **Headings:** `Playfair Display` serif — `/Heading 1` (96px) → `/Heading 1 Small`, `/Heading 2`, `/Heading 2 Small`, `/Heading 3`, `/Heading 3 Small`. Smaller headings (`/Heading 4`–`/Heading 6`) switch to `Inter`.
- **Body & UI:** `Kumbh Sans` — `/Body Left`, `/Body Center`, `/Body Right`, `/Body White`, `/Body Big`, `/Subheadline`.
- **Links:** `/Link`, `/Link Big`, `/Menu Link Desktop` (Playfair 48px), `/Menu Link Mobile`.
- **Buttons:** `/Button`, `/Button Small`, `/Button Light`.
- **Commerce/data:** `/Price`, `/Price  Big`, `/Product Name`, `/Statistic Number Big`, `/Statistic Number Small`, `/Quotes Big`, `/Quotes Small`, `/Name`, `/Role`, `/Span`.

**Type pairing:** Playfair Display (display/headings) + Kumbh Sans (body/UI). Keep new text within this pairing.

**Important constraints:**
- A text node uses **either** `inlineTextStyle` **or** a custom `font` — never both.
- To change a text node's **color**, you must use a text style (create or reuse one); color cannot be set directly on a text node.

---

## Component map

Key shared components (full list via `getProjectXml`):

- **Layout / nav:** `Navbar`, `Menu`, `Menu Link`, `Footer`, `Logo`, `Link`, `Link Collection`, `Copyright`
- **Buttons:** `Button`, `Button Form`, `Get Template Button`, `Get template Button`, `View More - With Link`, `View More - No Link`
- **Commerce:** `Shop Card`, `Recommended Product`, `Product Tabs`, `Cart 2.0`, `Cart Layer`, `Cart List Item 2`, `Add To Cart Button`, `Buy Button`, `Checkout button`
- **Services:** `Services Category Card`, `Services Category Content`, `Services package`, `Services Slider`, `Program Line`, `Vistiq Experience Line`
- **Social proof:** `Quotes`, `Quote Big`, `Quote Small`, `Ticker - Quotes`, `5-stars`, `Span with Stars`, `Happy Clients`, `Statistic Card`, `Statistic Card Small`
- **Forms:** `Contact Form`, `Newsletter Section`, `Newsletter Form`, `Search Placeholder`
- **Content:** `Accordion Card`, `Philosophy accordion - Desktop`, `Team Slider`, `Team Member Card`, `Profile Photo`, `Tag`, `Arrow`, `Plus-Minus`, `Bentos - Why Choose Vistiq`

---

## Workflow for common tasks

**Add a section to a page**
1. `getNodeXml` on the page → find the root `Desktop` breakpoint node ID.
2. Prefer a pre-built section component via `insertUrl` with `?detached=true` (hero, pricing, footer, testimonials, CTA, etc.).
3. After inserting a detached section, `getNodeXml` again on the page to read the generated internal structure, then customize text/images.

**Edit shared UI (nav, footer, buttons)**
- Edit the component itself only if the change is global. Otherwise detach the instance on the specific page.

**Write or edit Framer code files**
- First read the MCP resource `mcp://mcp.unframer.co/prompts/how-to-write-framer-code-files.md`. Do not freehand code components.

**Delete nodes**
- Use `deleteNode`. Omitting a node from an `updateXmlForNode` payload does **not** delete it.

---

## Don'ts

- Don't hardcode colors/fonts when a token exists.
- Don't paste large XML trees into a single `updateXmlForNode` call.
- Don't edit CMS slug pages with static content.
- Don't reference IDs you haven't seen returned by a tool.
- Don't store the MCP `secret=` anywhere.
