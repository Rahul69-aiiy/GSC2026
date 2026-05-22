**Render deployment guide**

Steps to deploy the backend to Render using your MongoDB Atlas URI.

Required secrets (set in Render and GitHub):

- `MONGODB_URI` (Atlas connection string)
- `RENDER_API_KEY` (GitHub secret to trigger deploys)
- `RENDER_SERVICE_ID` (GitHub secret: Render service id)
- Optional: `GEMINI_API_KEY`, `SECRET_KEY`, `TELEGRAM_API_ID`, `TELEGRAM_API_HASH`

1. Add the repo to Render and create a new service:
   - Connect your GitHub repo in the Render dashboard.
   - Create a Web Service and choose "Docker" environment.
   - Set the Dockerfile path to `backend/Dockerfile`.
   - Set the branch to `main`.

2. Add environment variables / secrets in Render (Settings → Environment):
   - `MONGODB_URI` = your Atlas connection string
   - `GEMINI_API_KEY` = (if using Gemini features)
   - `SECRET_KEY` = (JWT secret)
   - `TELEGRAM_API_ID`, `TELEGRAM_API_HASH` = (if using Telegram monitor)

3. In GitHub, add repository secrets:
   - `RENDER_API_KEY` = (from Render → Account → API Keys)
   - `RENDER_SERVICE_ID` = (Render service id: found in service URL or dashboard)

4. Push to `main` — the GitHub Actions workflow will call Render's API to trigger a deploy.

Local test (build Docker image locally):

```bash
docker build -t sportguard-backend -f backend/Dockerfile .
docker run -e MONGODB_URI='<your-atlas-uri>' -p 8000:8000 sportguard-backend
```

If you want, I can:

- create a `render.yaml` for full infra-as-code (added in this repo as `render.yaml`)
- or set up a GitHub Action that builds the image and pushes to a registry before deploying
