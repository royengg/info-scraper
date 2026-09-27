# Web Scraper Utility (Reddit, Twitter/X, Generic Sites)

A **TypeScript-based web scraping utility** that can extract content from:
- **Twitter/X** (via Nitter + Playwright fallback)
- **Reddit posts & comments** (using Reddit’s JSON API)
- **Generic websites** (via Axios + Cheerio parsing)
- **Exa API** (as a fallback for scraping structured page content)

It uses a **layered fallback strategy**:
`Axios → Exa API → Playwright`  
ensuring robust scraping across multiple sources.

---

##  Features
-  **Reddit Scraper**: Fetch posts and nested comments recursively.  
-  **Twitter/X Scraper**: Uses Nitter + Playwright for reliability.  
-  **Generic Website Scraper**: Extracts text with Axios + Cheerio.  
-  **Fallback System**: If one method fails, another is tried.  
-  **Environment Config**: Secure API key management with dotenv.  
-  **TypeScript Strongly Typed**: Safer development with clear interfaces.  

---

##  Folder Structure
```
.
├── dist/                # Compiled JavaScript output
├── node_modules/        # Dependencies
├── src/                 # Source code
│   ├── lib/
│   │   ├── constants.ts # Browser headers, shared constants
│   │   ├── utils.ts     # Utility functions (e.g., text cleaning, delay)
│   ├── types/
│   │   └── reddit.ts    # TypeScript types for Reddit scraping
│   └── scraper.ts       # Main scraper implementation
├── .env.example         # Example environment variables
├── .gitignore           # Ignored files/folders (node_modules, dist, etc.)
├── package.json         # Project metadata & scripts
├── tsconfig.json        # TypeScript config
├── tsconfig.tsbuildinfo # Build cache
```

---

##  Installation

1. **Clone the repo**
   ```bash
   git clone https://github.com/YOUR_USERNAME/web-scraper-utility.git
   cd web-scraper-utility
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment variables**
   - Copy `.env.example` → `.env`
   - Add your [Exa API Key](https://exa.ai/)
   ```env
   EXA_API_KEY=your_exa_api_key_here
   HEADLESS=true   # (optional) set false to see Playwright browser
   ```

---

##  Usage

### 1. Build the project
```bash
npm run build
```

### 2. Run scraper
```bash
node dist/src/scraper.js
```

### 3. Example: Scraping Reddit
```ts
import { scrapeReddit } from "./src/scraper";

async function main() {
  const data = await scrapeReddit({ subreddit: "webdev", postsCount: 10 });
  console.log(JSON.stringify(data, null, 2));
}

main();
```

### 4. Example: Scraping a Generic URL
```ts
import { scrape } from "./src/scraper";

async function main() {
  const result = await scrape("https://example.com");
  console.log(result);
}

main();
```

---

##  Scripts

```json
"scripts": {
  "dev": "ts-node src/scraper.ts",
  "build": "tsc",
  "start": "node dist/src/scraper.js"
}
```

- `npm run dev` → Run in development with ts-node  
- `npm run build` → Compile TypeScript to `dist/`  
- `npm start` → Run compiled code  

---

##  Environment Variables

| Variable      | Required | Description                          |
|---------------|----------|--------------------------------------|
| `EXA_API_KEY` | ✅       | API key for Exa API (fallback scraper) |
| `HEADLESS`    | ❌       | If `false`, Playwright will show UI   |

---

## 📊 Example Output

### Reddit scrape (`subreddit: "webdev", postsCount: 2`)
```json
{
  "posts": [
    {
      "post": "How do I get started with React?",
      "postId": "abc123",
      "posterId": "user123",
      "urlToPost": "https://reddit.com/r/webdev/comments/abc123",
      "comments": [
        {
          "commenterId": "cmt456",
          "commentText": "Check the official docs at react.dev",
          "commenterName": "dev_guru",
          "urlToComment": "https://reddit.com/r/webdev/comments/abc123/cmt456",
          "children": []
        }
      ]
    }
  ]
}
```

---

##  Contributing
Contributions are welcome!  
1. Fork the repo  
2. Create a feature branch  
3. Submit a PR   

---

##  License
This project is licensed under the [MIT License](./LICENSE.md).
