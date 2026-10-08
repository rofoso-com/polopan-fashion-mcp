---
name: polopan-fashion
description: >-
  Search PoloPan fashion catalog, deconstruct outfit images with AI bounding boxes, discover 100% in-stock occasion looks, display product specifications tables, check live SKU sizes/stock & return/shipping policies, and generate 1-click checkout purchase links.
---

# PoloPan Fashion & Products Skill

This skill guides AI assistants on how to query the **PoloPan Products MCP Server** to find apparel, deconstruct outfit photos into individual pieces, check real-time size stock, find budget alternatives, discover occasion-tailored looks, render structured product specifications tables, and generate direct 1-click checkout links with verified `s.polopan.com` permalinks.

---

## 🛠 Available MCP Tools

| Tool | Purpose | Key Arguments |
| :--- | :--- | :--- |
| `search_products_text` | Search products via natural language & filters with tabular `product_details` and policy SLAs | `query`, `gender`, `price_min`, `price_max`, `size`, `vendor`, `sort_by`, `sort_order`, `page`, `page_size` |
| `search_products_image` | Find visually similar items using an image URL | `image_url`, `gender`, `price_min`, `price_max`, `size`, `vendor` |
| `search_products_image_upload` | Find items by uploading local image / base64 | `file_path` plus search filters |
| `detect_fashion_pieces` | Deconstruct outfit photo into pieces (Tops, Bottoms, Shoes, Bags, Accessories) | `image_url`, `threshold` |
| `get_looks_by_occasion` | Discover complete curated looks for events (Wedding, Party, Date Night, Casual, etc.). Automatically excludes out-of-stock looks. | `occasion`, `gender` (`women`/`men`), `age`, `page`, `page_size`, `vendor` |
| `get_product_by_handle` | Retrieve full product document, `product_details`, & variants | `handle` (e.g. `shopify_11206`) |
| `get_size_guide` | Get full garment measurement matrix (Chest, Waist, Hip, Length), metric/imperial unit conversion (`in`/`cm`), and automatic best-fit size recommendation based on body dimensions | `handle`, `unit` (`in`/`cm`), `desired_size`, `user_chest`, `user_waist`, `user_hip`, `user_unit` |
| `check_variant_availability` | Verify real-time stock, `product_details` table, `size_chart`, in-stock sizes, live pricing, and shipping/return SLAs | `handle`, `desired_size` (optional), `size_index` (optional) |
| `get_direct_checkout_url` | Generate 1-click direct checkout permalink with confirmed size index & coupon (`https://s.polopan.com/p/{handle}/{size_index}`) | `handle`, `size`, `size_index`, `quantity`, `coupon` |
| `search_alternatives_in_budget` | Find visually similar alternatives in a target price bracket | `handle`, `budget_range` (`0-1500`, `1501-3000`, `3001-5000`, `5000+`), `limit` |
| `get_recommended_outfits` | Get complementary matching outfits by handle or occasion (100% in-stock verified) | `handle` OR `occasion`, `gender`, `page`, `page_size` |

---

## 🚨 MANDATORY OPERATIONAL RULES FOR ALL AI AGENTS

### Rule 0: Available Sizes & Policy Display
When presenting any look or product recommendation:
1. **Available In-Stock Sizes**: Always tell the user the currently available sizes for every item in that look (e.g., `Available Sizes: XS, S, M, L, XL` or `Available Sizes: 38, 40, 42`).
2. **Shipping & Return SLA**: Always display the shipping dispatch SLA and return policy for each item.
3. **Negative Return Days = Exchange Only**:
   * If `return_days < 0` (or `return_policy_text` starts with Exchange), display: **`Exchange only |X| days`** (e.g., `-7` days $\rightarrow$ `Exchange only 7 days`, `-5` days $\rightarrow$ `Exchange only 5 days`).
   * If `return_allowed` is `false` or `0` days: display `No returns (Final sale)`.
   * If positive: display `X days easy returns`.

---

### Rule 1: Strict In-Stock Look Shortlisting (Never Return OOS Looks)
* **Never recommend or shortlist a look that contains any out-of-stock item.**
* Every single item in the outfit (garments, footwear, bag, accessories) must be currently in stock.
* If any component in a look is out of stock (`is_in_stock: false` or `available_sizes` is empty), **immediately discard that look and move to the next look**. Do not shortlist or present out-of-stock looks to the user.

---

### Rule 2: Structured Specifications Table for Shortlisted Looks
When a look is shortlisted and presented to the user, render a structured specifications table for the garments/items from `product_details`:

```markdown
| Specification | Details |
| :--- | :--- |
| **Material / Fabric** | Pure Cotton / Silk Blend |
| **Pattern / Work** | Embroidered / Woven Floral |
| **Collar / Neck** | Round Neck / Mandarin Collar |
| **Sleeve Length** | 3/4 Sleeves / Long Sleeves |
| **Fit / Shape** | Straight / Regular Fit |
| **Bottom Type** | Trousers / Pyjama with Drawstring |
| **Care Instructions** | Machine wash / Dry clean |
| **Shipping** | Dispatched within 2 days |
| **Return / Exchange** | Exchange only 7 days |
```
*(Do not mention internal terms like "quick view" to the user—simply display the table cleanly).*

---

### Rule 3: Direct `/{size_index}` Checkout Links ONLY When Sizes Are Finalized
* **During initial browsing, shortlisting, or exploration**: Return **ONLY** the clean base link without a size index:
  `https://s.polopan.com/p/{handle}`
* **If there is any confusion or ambiguity regarding size**: Return **ONLY** the clean base link:
  `https://s.polopan.com/p/{handle}`
* **Prompt the user to select/confirm their size**:
  *"Please let me know your preferred size from the available options (e.g., M, L, XL) so I can prepare your direct 1-click checkout link."*
* **ONLY and ONLY when the user's size is confirmed/finalized**: Provide the complete direct 1-click checkout permalink with the resolved `{size_index}`:
  `https://s.polopan.com/p/{handle}/{size_index}`

---

### Rule 4: Size Guide, Unit Conversion & Fit Recommendations
When the user inquires about garment fit, dimensions, size charts, or what size to buy:
1. **Call `get_size_guide`**: Retrieve the garment's measurement matrix (`Chest`, `Waist`, `Hip`, `Length`), unit converter (`in` / `cm`), and how-to-measure tips.
2. **Display Formatted Size Chart**: Present the measurement table cleanly in Markdown:
   ```markdown
   | Size | Chest (in) | Waist (in) | Length (in) |
   | :--- | :--- | :--- | :--- |
   | **S** | 38 | 30 | 28 |
   | **M** | 40 | 32 | 29 |
   | **L** | 42 | 34 | 30 |
   ```
3. **Automated Fit Match**: When the user provides their body measurements (e.g., *"My chest is 40 inches"*), provide the tailored recommendation from `fit_recommendation`, explain whether it's an exact or relaxed fit, and include the 1-click checkout link for the recommended size.

---

## 🎯 Example Flow: Recommending a Wedding Look

1. User asks: *"Give me an outfit for a wedding"*
2. AI calls `get_looks_by_occasion(occasion="Wedding & Reception", gender="women")`.
3. AI inspects returned looks (which are verified 100% in stock).
4. AI shortlists the top look and presents:
   - Complete look title and hero image.
   - Component items breakdown with **Available Sizes** for each item.
   - Specifications Table (`Material`, `Work`, `Neck`, `Fit`, `Care`, `Shipping SLA`, `Return/Exchange Policy`).
   - Clean base product links: `https://s.polopan.com/p/{handle}`.
   - AI asks user: *"Which size would you like for the Kurta set and footwear?"*
5. User replies: *"I need size M for the kurta and size 38 for the juttis."*
6. AI responds with the finalized 1-click direct checkout permalinks:
   - Kurta Set (Size M): `https://s.polopan.com/p/{kurta_handle}/{m_index}`
   - Juttis (Size 38): `https://s.polopan.com/p/{jutti_handle}/{38_index}`

---

## ⚙️ MCP Server Configuration

### Hosted HTTP (Cursor / Claude Desktop / Remote Agents)
```json
{
  "mcpServers": {
    "polopan-products": {
      "type": "http",
      "url": "https://mcp-server.polopan.com/mcp"
    }
  }
}
```

### Stdio / Local CLI (Node.js 18+)
```bash
npx -y polopan-products-mcp
```
