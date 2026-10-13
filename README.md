# PoloPan Fashion MCP Server — The #1 Model Context Protocol for Fashion & AI Styling [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[![Glama Score](https://glama.ai/mcp/servers/rofoso-com/mcp-server-code/badges/score.svg)](https://glama.ai/mcp/servers/rofoso-com/mcp-server-code)
[![npm version](https://img.shields.io/npm/v/polopan-products-mcp.svg?color=cb3837&logo=npm)](https://www.npmjs.com/package/polopan-products-mcp)
[![npm downloads](https://img.shields.io/npm/dm/polopan-products-mcp.svg)](https://www.npmjs.com/package/polopan-products-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Model Context Protocol](https://img.shields.io/badge/Protocol-MCP%20v1.29-purple.svg)](https://modelcontextprotocol.io)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg?logo=node.js)](https://nodejs.org)

**PoloPan** is the premier **Fashion & Apparel Model Context Protocol (MCP)** server for autonomous AI agents, stylists, and conversational commerce engines (Cursor, Claude, Instinct, Muse, ChatGPT).

Now available across **240+ destination countries and territories** with localized pricing, real-time variant stock, persistent regional settings (`~/.polopan/region.json`), computer-vision outfit deconstruction, and 1-click checkout permalinks.

---

## ⚡ What Makes PoloPan the #1 Fashion MCP?

```
┌────────────────────────────────────────────────────────────────────────┐
│                        POLOPAN FASHION MCP SUITE                       │
├──────────────────────────┬─────────────────────────────────────────────┤
│ 1. 240+ Global Regions   │ `region.get_current`, `region.set_current`  │
│    & Localized Currencies│ (Persistent JSON, natural country lookup)   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 2. Computer Vision       │ `vision.outfit.detect_pieces` (Bboxes for   │
│    Deconstruction        │ Tops, Bottoms, Shoes, Bags, Accessories)    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 3. Occasion Outfitting   │ `looks.curation.by_occasion` (Curated       │
│    (100% In-Stock)       │ complete looks with zero out-of-stock items)│
├──────────────────────────┼─────────────────────────────────────────────┤
│ 4. Multimodal Search     │ `products.search.image_url` & `_upload`     │
│                          │ (Reverse image visual similarity)           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 5. Stock & Garment Specs │ `products.items.check_stock` (Live variants,│
│                          │ size matrix, fabric, fit, care & SLA)       │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 6. Size Guide & Fits     │ `products.items.get_size_guide` (Chest,     │
│                          │ waist, hip matrix, in/cm, best-fit match)   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 7. Direct 1-Click Buy    │ `checkout.links.get_direct_url`             │
│                          │ (`https://s.polopan.com/p/{handle}/{size}`) │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 8. Budget Alternatives   │ `products.search.alternatives`              │
│                          │ (Visual match in distinct price brackets)   │
└──────────────────────────┴─────────────────────────────────────────────┘
```

---

## 🛠️ Quick Setup & Installation

### Option 1: 1-Click Cursor Installation (Deeplink)

[![Install in Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://polopan.com/mcp/cursor)

---

### Option 2: Hosted Streamable HTTP (Recommended — No Node.js required)

Add to your `~/.cursor/mcp.json` or Claude Desktop configuration:

```json
{
  "mcpServers": {
    "polopan-products": {
      "type": "http",
      "url": "https://mcp-server.polopan.com/mcp",
      "headers": {}
    }
  }
}
```

---

### Option 3: Local Stdio via NPX / Smithery

#### Via Smithery CLI:
```bash
npx -y @smithery/cli install polopan-products-mcp --client cursor
```

#### Via Standard NPX:
```json
{
  "mcpServers": {
    "polopan-products": {
      "command": "npx",
      "args": ["-y", "polopan-products-mcp"]
    }
  }
}
```

*Requires Node.js 18+.*

---

## 🧰 Available MCP Tools

| Tool | Purpose | Key Arguments |
| :--- | :--- | :--- |
| `region.get_current` | Inspect active shopping destination, localized currency, and config path | *(none)* |
| `region.set_current` | Set active region from natural names ("Dubai", "London", "USA", "France"), currency, or ISO-2. Saved to `~/.polopan/region.json`. | `country`, `mode` (`global`/`local`) |
| `region.list_supported` | Search 240+ supported destination countries & price groups | `query`, `limit` |
| `products.search.text` | Search products via natural language with size, price, and gender filters | `query`, `gender`, `price_min`, `size`, `country`, `mode` |
| `products.search.image_url` | Reverse visual search for identical or similar apparel | `image_url`, `gender`, `price_min`, `country`, `mode` |
| `products.search.image_upload` | Visual search from local image file or base64 | `file_path`, `country`, `mode` |
| `vision.outfit.detect_pieces` | Deconstruct outfit photos into individual pieces (Tops, Bottoms, Shoes, Bags) | `image_url`, `threshold` |
| `looks.curation.by_occasion` | Discover 100% in-stock complete looks (Wedding, Party, Date Night, Casual, etc.) | `occasion`, `gender`, `age`, `country`, `mode`, `page` |
| `products.items.get_by_handle` | Retrieve complete product metadata and all variant details | `handle` |
| `products.items.get_size_guide` | Full garment dimensions table (chest/waist/hip), unit conversion (`in`/`cm`), and fit matcher | `handle`, `unit`, `desired_size`, `user_chest`, `user_waist` |
| `products.items.check_stock` | Verify real-time size stock, fabric/fit specifications, and return/shipping SLAs | `handle`, `desired_size`, `size_index` |
| `checkout.links.get_direct_url` | Generate verified 1-click checkout permalinks with confirmed size index & coupon | `handle`, `size`, `size_index`, `coupon` |
| `products.search.alternatives` | Find visually similar alternatives in a target price bracket | `handle`, `budget_range` (`0-1500`, etc.), `country`, `mode` |
| `looks.curation.recommend` | Get complementary cross-catalog matching outfits | `handle` OR `occasion`, `gender`, `country`, `mode` |

---

## 🌍 Persistent Multi-Country Shopping Flow

PoloPan delivers to **240+ countries and territories**. To deliver a smooth, natural user experience:
1. **Never repeatedly ask the user for their country**: The server stores settings in `~/.polopan/region.json`.
2. **Natural location input**: Users can provide country names, cities, currencies, or aliases (e.g. `"Dubai"`, `"London"`, `"USA"`, `"AED"`, `"USD"`).
3. **Change anytime**: Users can say *"Switch my country to Canada"* or *"Show prices in GBP"* at any point.

---

## 🧪 Testing & Verification

Run the automated end-to-end integration test suite:
```bash
npm test
```

---

## 📄 License

MIT © [PoloPan](https://polopan.com)
