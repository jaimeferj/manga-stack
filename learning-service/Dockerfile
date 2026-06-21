FROM python:3.12-slim AS builder

RUN pip install --no-cache-dir uv==0.7.13

WORKDIR /app

COPY pyproject.toml uv.lock ./

ENV UV_LINK_MODE=copy
ENV UV_PROJECT_ENVIRONMENT=/app/.venv

RUN uv sync --frozen --no-dev --no-install-project

COPY src ./src

RUN uv sync --frozen --no-dev

FROM python:3.12-slim

WORKDIR /app

COPY --from=builder /app/.venv .venv
COPY --from=builder /app/src ./src

ENV PATH="/app/.venv/bin:$PATH"
ENV PYTHONUNBUFFERED=1

EXPOSE 8765

CMD ["python", "-m", "manga_learning_service"]
