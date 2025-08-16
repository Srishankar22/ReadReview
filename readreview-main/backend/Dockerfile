FROM python:3.11-slim

# If you want local LanguageTool remove the next 2 lines and see Option B below
# (Public API needs no Java and is simpler)
# RUN apt-get update && apt-get install -y --no-install-recommends default-jre-headless \
#     && rm -rf /var/lib/apt/lists/*

# Build tools for some wheels
RUN apt-get update && apt-get install -y --no-install-recommends build-essential gcc g++ \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY requirements.txt /app/requirements.txt

# Install deps (+ CPU Torch wheels)
RUN pip install --upgrade pip && \
    pip install --no-cache-dir \
      --extra-index-url https://download.pytorch.org/whl/cpu \
      -r requirements.txt

COPY . /app

# Optional: warm up the ST model so first request is fast
RUN python - <<'PY'
from sentence_transformers import SentenceTransformer
SentenceTransformer("all-MiniLM-L6-v2")
PY

ENV PYTHONUNBUFFERED=1 PORT=8000
EXPOSE 8000
CMD ["uvicorn","main:app","--host","0.0.0.0","--port","8000"]
