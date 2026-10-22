# Shopify Apron Store Redesign: New Theme, $10 Personalisation and AI Banners

> **Case study.** This repository documents the approach and results only. The theme code and store data are private and are not included. The client's store is not named.

## Overview

A UK store that sells leather and canvas work aprons (chef, blacksmith, canvas, bulk and wholesale) wanted a new look. The new theme reuses the design of one of my fashion stores, adapted for aprons, with:

- the store's own 38 products and collections, unchanged
- a personalisation option that adds **$10** to the order
- new banners, made with AI image generation
- new information pages, rewritten in fresh wording rather than copied

The whole build ran in an **unpublished draft theme** on the live store. The live theme, products, collections, existing pages and menus were never touched. The owner previews the draft and decides when to publish.

## What was built

### 1. Personalisation that adds $10
- A "Add your personalisation (+$10)" box on every apron product page, with engraving text and a logo upload.
- On Add to Cart, a small script adds a hidden $10 "Personalisation" item next to the apron. It matches the quantity and pack size, so 3 aprons = 3 × $10.
- The engraving text and logo file travel with the order line, so the workshop sees exactly what to make.
- "Buy it now" is hidden while personalisation is ticked, so the extra charge can't be skipped.
- Tested: $46.99 apron + $10 personalisation = $56.99 in the cart, with the engraving on the order line.

### 2. Collection pages with their own banners
- A new collection banner section: photo strip, title, short intro and product count.
- Each collection (Chef, Blacksmith, Canvas, Bulk/Wholesale, Single, All) gets its own image and intro, chosen inside the theme. The store's collection data stays as it is.
- Faces are kept in frame at every screen size, with a different aspect ratio per device: 16:9 on phones, 21:8 on tablets and 3:1 on desktop and TV.

### 3. AI-generated banners
- The home hero (desktop and phone versions), collection banners and the craft and workshop images were all generated with GPT Image, then upscaled to 2K.
- Each image is exported at 900px and 2000px and served with `srcset`, so phones download the small file.

### 4. Honest product pages
I removed sales tricks copied from the template that weren't true for this store:
- fake countdowns ("Sale ends tonight")
- "X people viewing" counters
- "only 10 left" messages
- discount codes that didn't exist
- copied reviews

Delivery times, the returns window and shipping text now match the store's real policies.

### 5. Information pages in new wording
- About, FAQs, Delivery Information and Returns & Exchanges were rewritten from scratch, with readable tables and an 820px reading width.
- New footer menus point to these pages and to Shopify's built-in policies.
- I found contradictions between the old pages (shipping area, delivery time, damage-report window, address) and flagged them to the owner instead of guessing.

### 6. Rounds of client feedback
- Offer bar recoloured, footer switched to black, and soft pink/red discount badges.
- Only one badge per product, in this order: Sold out, then discount, then New.
- A smaller logo, plain trust lines (delivery, exchanges, secure checkout) in the style the client liked, and a menu with a red hover underline.
- Fixed a cart layout bug where a page-width rule was squeezing the cart table.

## How it was done

- **Shopify Admin GraphQL API** through an MCP connector:
  - theme creation and renaming
  - theme file uploads, via `stagedUploadsCreate` → upload → `themeFilesUpsert`, needed for 200 KB section files
  - pages and menus
- **Liquid + JSON templates:** new sections and snippets, and template settings edited in code.
- **Gotchas found and solved:**
  - Shopify silently rejects a theme file if one setting breaks its schema rules (for example a value off the `step`) or if a template points to a missing section. I uploaded small files as plain text to surface the errors.
  - Headless test browsers get rate-limited (429) on Shopify previews, so the visual QA used a real browser instead.
- **QA at 375, 768, 1440 and 1920px:** home, collections, product, info pages and cart. I checked for sideways scrolling, broken images, banner crops, the personalisation price and cart totals.

## Tech

Shopify (Online Store 2.0, Liquid, JSON templates) · Admin GraphQL API · MCP connector · JavaScript (fetch wrapper for the add-on) · CSS · AI image generation and upscaling · sharp for image resizing
