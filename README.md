# PoloPan Fashion MCP Server — The #1 Model Context Protocol for Fashion & AI Styling

[![Glama Score](https://glama.ai/mcp/servers/rofoso-com/mcp-server-code/badges/score.svg)](https://glama.ai/mcp/servers/rofoso-com/mcp-server-code)
[![npm version](https://img.shields.io/npm/v/polopan-products-mcp.svg?color=cb3837&logo=npm)](https://www.npmjs.com/package/polopan-products-mcp)
[![npm downloads](https://img.shields.io/npm/dm/polopan-products-mcp.svg)](https://www.npmjs.com/package/polopan-products-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Model Context Protocol](https://img.shields.io/badge/Protocol-MCP%20v1.29-purple.svg)](https://modelcontextprotocol.io)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg?logo=node.js)](https://nodejs.org)

**PoloPan** is the premier **Fashion & Apparel Model Context Protocol (MCP)** server for autonomous AI agents, stylists, and conversational commerce engines (Cursor, Claude, Instinct, Muse, ChatGPT).

It empowers AI assistants to **deconstruct outfit photos with computer-vision bounding boxes**, find **100% in-stock occasion looks**, verify **real-time size availability & fabric specs**, and generate **1-click direct checkout permalinks**.

---

## ⚡ What Makes PoloPan the #1 Fashion MCP?

```
┌────────────────────────────────────────────────────────────────────────┐
│                        POLOPAN FASHION MCP SUITE                       │
├──────────────────────────┬─────────────────────────────────────────────┤
│ 1. Computer Vision       │ `detect_fashion_pieces` (Bounding boxes for │
│    Deconstruction        │ Tops, Bottoms, Shoes, Bags, Accessories)    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 2. Occasion Outfitting   │ `get_looks_by_occasion` (Curated complete   │
│    (100% In-Stock)       │ looks with zero out-of-stock items)         │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 3. Multimodal Search     │ `search_products_image` & `_upload`         │
│                          │ (Reverse image visual similarity)           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 4. Stock & Garment Specs │ `check_variant_availability` (Live variants,│
│                          │ size matrix, fabric, fit, care & SLA)       │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 5. Direct 1-Click Buy    │ `get_direct_checkout_url`                   │
│                          │ (`https://s.polopan.com/p/{handle}/{size}`) │
├──────────────────────────┼─────────────────────────────────────────────┤
│ 6. Budget Alternatives   │ `search_alternatives_in_budget`             │
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
| `detect_fashion_pieces` | Deconstruct outfit photos into individual pieces (Tops, Bottoms, Shoes, Bags) | `image_url`, `image_path`, `threshold` |
| `get_looks_by_occasion` | Discover 100% in-stock complete looks (Wedding, Party, Date Night, Casual, etc.) | `occasion`, `gender`, `age`, `page` |
| `search_products_text` | Search products via natural language with size, price, and gender filters | `query`, `gender`, `price_min`, `size` |
| `search_products_image` | Reverse visual search for identical or similar apparel | `image_url`, `gender`, `price_min` |
| `search_products_image_upload` | Visual search from local image file or base64 | `image_path`, `image_base64` |
| `check_variant_availability` | Verify real-time size stock, fabric/fit specifications, and return/shipping SLAs | `handle`, `desired_size`, `size_index` |
| `get_product_by_handle` | Retrieve complete product metadata and all variant details | `handle` |
| `get_direct_checkout_url` | Generate verified 1-click checkout permalinks with confirmed size index & coupon | `handle`, `size`, `size_index`, `coupon` |
| `search_alternatives_in_budget` | Find visually similar alternatives in a target price bracket | `handle`, `budget_range` (`0-1500`, etc.) |
| `get_recommended_outfits` | Get complementary cross-catalog matching outfits | `handle` OR `occasion`, `gender` |

---

## 💡 Copy-Paste Agent Prompts

### 1. Shop the Full Look from an Instagram Photo
```text
Use PoloPan MCP to deconstruct this outfit photo into individual pieces (top, bottom, footwear, accessories).
Find the closest match on PoloPan for each piece in size M, check live stock, and give me direct 1-click checkout links with total outfit price.
```

### 2. Wedding Guest Outfit Discovery
```text
I need outfit ideas for an evening wedding reception.
Give me 5 complete looks for women with jewellery, shoes, and bag.
Check available in-stock sizes and give me the direct 1-click checkout links with shipping timelines.
```

### 3. Budget Alternative Finder
```text
I want a printed resort shirt in the 1501-3000 budget range for a beach vacation.
Then use PoloPan MCP to give me the best matching trousers and footwear to complete the set.
```

---

## 🧪 Testing & Verification

Run the automated end-to-end integration test suite:
```bash
npm test
```

---

## 📄 License

MIT © [PoloPan](https://polopan.com)
