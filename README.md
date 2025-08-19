# Prompt Architect (with Gemini)

A lightweight, single-file web app to help you craft high‑quality prompts. Enter a rough request, pick a target user persona, and the app uses Google's Gemini API to propose a structured prompt with clear sections you can refine and copy.

### Features
- **User input → structured prompt**: Role, Task, Context, Reasoning, Output format, Stop condition
- **Persona-aware**: Adjusts tone and depth for different user levels
- **One-click copy**: Copy the full prompt in a clean, Markdown-ready format
- **Zero build**: Just a single `app.html` with Tailwind via CDN

---

## Quick start

### 1) Prerequisites
- A modern browser
- A Google API key from **Google AI Studio**: [Get an API key](https://aistudio.google.com). See also the **Gemini API docs**: [Gemini API documentation](https://ai.google.dev/gemini-api/docs)

### 2) Configure your API key
Open `app.html` and set your key at the line where the API key is defined:

```html
const apiKey = "YOUR_GOOGLE_API_KEY";
```

Note: This app calls Gemini directly from the browser for simplicity. For production use, see “Security notes” below.

### 3) Run locally
You can open `app.html` directly in your browser, or serve it with a simple static server to avoid any local file restrictions.

- Python
```bash
cd /home/jason12/Documents/hyper-prompt-creator
python3 -m http.server 8000
```
Open `http://localhost:8000/app.html`.

- Node (serve)
```bash
npx serve /home/jason12/Documents/hyper-prompt-creator --listen 8000
```
Open `http://localhost:8000/app.html`.

---

## How to use
1. **Nhập yêu cầu ban đầu** vào ô "Nhập yêu cầu ban đầu của bạn".
2. **Chọn đối tượng người dùng** (Newbie / Regular / Advanced / Expert).
3. Nhấn **“Tạo Prompt chi tiết”** để AI tạo cấu trúc prompt.
4. Xem, chỉnh sửa từng phần: **Role, Task, Context, Reasoning, Output Format, Stop Condition**.
5. Nhấn **“Sao chép toàn bộ Prompt”** để copy nội dung đã tinh chỉnh.

---

## Customization
- **Model**: Update the model in `app.html` at the API URL if you want a different Gemini model.
- **Instruction**: Tweak the `instruction` string in `app.html` to steer how the structure is generated.
- **Styling**: Tailwind is loaded via CDN; you can adjust classes right in the HTML.

---

## Security notes (production)
Placing API keys in client-side code exposes them to users. For production, route requests through your backend or a serverless function and keep the key in server-side environment variables.

Minimal Express proxy example:
```js
import express from "express";
import fetch from "node-fetch";

const app = express();
app.use(express.json());

app.post("/api/generate", async (req, res) => {
  const url = "https://generativelanguage.googleapis.com/v1beta/models/"
            + "gemini-2.5-flash-preview-05-20:generateContent?key="
            + process.env.GOOGLE_API_KEY;

  const upstream = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(req.body),
  });

  const data = await upstream.json();
  res.status(upstream.status).json(data);
});

app.listen(3000, () => console.log("Proxy listening on http://localhost:3000"));
```
Then change the frontend to call your proxy (`/api/generate`) instead of Google directly and remove `const apiKey = "..."` from the client.

---

## Troubleshooting
- **401 Unauthorized**: Check the API key value and project permissions; verify the model is available for your key.
- **CORS or network errors**: Prefer running via a local server (`http.server`, `serve`) rather than opening the file directly.
- **Empty or malformed response**: Ensure the model name and payload match current Gemini API expectations.

---

## Tech stack
- HTML + Tailwind CSS (CDN)
- Vanilla JavaScript `fetch` to Gemini API

---

## License
Specify your license here (e.g., MIT). 