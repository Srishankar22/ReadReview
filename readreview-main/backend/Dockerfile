FROM python:3.11-slim

# Java (for local LanguageTool) + build tools for native wheels
RUN apt-get update && apt-get install -y --no-install-recommends \
    default-jre-headless build-essential gcc g++ \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY requirements.txt /app/requirements.txt

RUN pip install --upgrade pip && \
    pip install --no-cache-dir \
      --extra-index-url https://download.pytorch.org/whl/cpu \
      -r requirements.txt

COPY . /app

# Optional warm-ups
RUN python - <<'PY'
from sentence_transformers import SentenceTransformer
SentenceTransformer("all-MiniLM-L6-v2")
PY

# (optional) warm LanguageTool
# RUN python - <<'PY'
# import language_tool_python
# tool = language_tool_python.LanguageTool('en-US'); tool.close()
# PY

ENV PYTHONUNBUFFERED=1
EXPOSE 8000
CMD ["sh", "-c", "uvicorn main:app --host 0.0.0.0 --port ${PORT:-8000}"]
