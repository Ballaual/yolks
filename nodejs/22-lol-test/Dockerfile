FROM --platform=$TARGETOS/$TARGETARCH node:22-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Systemabhängigkeiten + Chromium
RUN apt update && apt -y install \
    ffmpeg \
    iproute2 \
    git \
    sqlite3 \
    libsqlite3-dev \
    python3 \
    python3-dev \
    ca-certificates \
    dnsutils \
    tzdata \
    zip \
    tar \
    curl \
    build-essential \
    libtool \
    iputils-ping \
    chromium \
    dbus \
    libnss3 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libcups2 \
    libgdk-pixbuf2.0-0 \
    libnspr4 \
    libx11-xcb1 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libxss1 \
    libxtst6 \
    libgbm1 \
    libasound2 \
    libpangocairo-1.0-0 \
    libpango-1.0-0 \
    libpangoft2-1.0-0 \
    libgtk-3-0 \
    libdrm2 \
    libxkbcommon0 \
    libxshmfence1 \
    fonts-freefont-ttf \
    fonts-ipafont-gothic \
    && dbus-uuidgen > /var/lib/dbus/machine-id \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Node.js-Tools
RUN npm install npm@latest typescript ts-node --location=global

# pnpm einrichten
RUN npm install -g corepack \
    && corepack enable \
    && corepack prepare pnpm@latest --activate

# Benutzerkonfiguration
RUN useradd -m -d /home/container container
USER container
ENV USER=container \
    HOME=/home/container \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium \
    NODE_ENV=production

# Arbeitsverzeichnis und Entrypoint
WORKDIR /home/container
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
STOPSIGNAL SIGINT
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
