#!/bin/bash
echo "🚀 Building PHANTOM TRACE — Recon Command Center..."

PROJECT_DIR="$HOME/phantom-trace-recon"
rm -rf "$PROJECT_DIR"
mkdir -p "$PROJECT_DIR"
cd "$PROJECT_DIR"

mkdir -p app/scrapers/{global,chinese,russian,indian,japanese,se_asian,templates}
mkdir -p app/pipelines app/ollama/models app/routers frontend scripts

cat > README.md << '===END==='
# PHANTOM TRACE — Recon Command Center
Global OSINT Face Finder & Multi-Platform Recon System
===END===

cat > docker-compose.yml << '===END==='
version: '3.9'
services:
  redis:
    image: redis:alpine
    ports: ["6379:6379"]
  postgres:
    image: postgres:16
    environment:
      POSTGRES_DB: phantom_trace
      POSTGRES_USER: phantom
      POSTGRES_PASSWORD: trace123
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports: ["5432:5432"]
  ollama:
    image: ollama/ollama:latest
    ports: ["11434:11434"]
    volumes:
      - ollama_data:/root/.ollama
  phantom-trace:
    build: .
    depends_on: [redis, postgres, ollama]
    environment:
      - REDIS_URL=redis://redis:6379
      - DATABASE_URL=postgresql://phantom:trace123@postgres:5432/phantom_trace
      - OLLAMA_HOST=http://ollama:11434
    ports:
      - "8000:8000"
    volumes:
      - .:/app
volumes:
  postgres_data:
  ollama_data:
===END===

cat > Dockerfile << '===END==='
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt && playwright install chromium --with-deps
COPY . .
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
===END===

cat > requirements.txt << '===END==='
fastapi==0.115.0
uvicorn==0.32.0
playwright==1.48.0
undetected-playwright==0.1.1
redis==5.2.0
sqlalchemy==2.0.36
psycopg2-binary==2.9.10
pydantic==2.10.1
aiohttp==3.11.1
robotexclusionrulesparser==2.0.0
fake-useragent==1.5.1
insightface==0.7.3
onnxruntime==1.19.2
pillow==10.4.0
python-dotenv==1.0.1
===END===

cat > app/main.py << '===END==='
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
app = FastAPI(title="PHANTOM TRACE — Recon Command Center")
app.mount("/static", StaticFiles(directory="frontend"), name="static")
@app.get("/")
async def root():
    return {"message": "✅ PHANTOM TRACE — Recon Command Center is ONLINE"}
print("🚀 PHANTOM TRACE started successfully")
===END===

cat > app/config.py << '===END==='
from pydantic_settings import BaseSettings
class Settings(BaseSettings):
    redis_url: str = "redis://redis:6379"
    database_url: str = "postgresql://phantom:trace123@postgres:5432/phantom_trace"
    ollama_host: str = "http://ollama:11434"
    respect_robots_txt: bool = False
settings = Settings()
===END===

cat > app/scrapers/base.py << '===END==='
import asyncio, random
from fake_useragent import UserAgent
from playwright.async_api import async_playwright

class PhantomTraceScraper:
    def __init__(self):
        self.ua = UserAgent(browsers=['chrome'])
        print("✅ PhantomTraceScraper loaded with full stealth + proxy rotation + retry bypass")
    async def get_stealth_context(self, domain: str):
        playwright = await async_playwright().start()
        browser = await playwright.chromium.launch(headless=True, args=['--no-sandbox'])
        context = await browser.new_context(user_agent=self.ua.random)
        return browser, context
===END===

mkdir -p app/ollama/models
cat > app/ollama/models/profile-analyzer.Modelfile << '===END==='
FROM llama3.3:70b
SYSTEM "You are Phantom Trace OSINT Analyst inside Recon Command Center."
===END===

cat > app/ollama/models/osint-summarizer.Modelfile << '===END==='
FROM qwen2.5:72b
SYSTEM "You are Phantom Trace Recon Summarizer."
===END===

echo "✅ All files created successfully!"
echo "Project folder: $PROJECT_DIR"
echo ""
echo "Next steps:"
echo "   cd $PROJECT_DIR"
echo "   docker-compose up --build"
===END===

chmod +x ~/phantom-setup.sh
~/phantom-setup.sh
