FROM base

WORKDIR /app

# Install Docker CLI and Docker Compose (plugin-based)
RUN apt-get update && \
    apt-get install -y ca-certificates curl gnupg lsb-release && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce-cli docker-compose-plugin && \
    docker --version && \
    docker compose version

COPY --from=build /prod/papra-server ./
COPY --from=build /app/apps/papra-client/dist ./public

EXPOSE 1221

ENV NODE_ENV=production
ENV SERVER_SERVE_PUBLIC_DIR=true
ENV DATABASE_URL=file:./app-data/db/db.sqlite
ENV DOCUMENT_STORAGE_FILESYSTEM_ROOT=./app-data/documents
ENV PAPRA_CONFIG_DIR=./app-data
ENV EMAILS_DRY_RUN=true
ENV CLIENT_BASE_URL=http://localhost:1221

RUN mkdir -p ./app-data/db ./app-data/documents ./ingestion

CMD ["pnpm", "start:with-migrations"]
