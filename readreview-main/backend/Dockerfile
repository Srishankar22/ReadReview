FROM python:3.11-slim

# Java + LanguageTool + build tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    default-jre-headless languagetool build-essential gcc g++ \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY requirements.txt /app/requirements.txt

RUN pip install --upgrade pip && \
    pip install --no-cache-dir \
      --extra-index-url https://download.pytorch.org/whl/cpu \
      -r requirements.txt

COPY . /app

# (optional) warm the ST model
RUN python - <<'PY'
from sentence_transformers import SentenceTransformer
SentenceTransformer("all-MiniLM-L6-v2")
PY

# (optional) keep Java memory reasonable on small plans
ENV JAVA_TOOL_OPTIONS="-Xms128m -Xmx512m"

ENV PYTHONUNBUFFERED=1
EXPOSE 8000
CMD ["sh","-c","uvicorn main:app --host 0.0.0.0 --port ${PORT:-8000}"]
