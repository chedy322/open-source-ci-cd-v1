# Uuse multi-layers
FROM node:20-alpine AS build

WORKDIR /usr/src/app

COPY package.json package-lock.json ./

# We store the node_module in locker inside the hard drive
RUN --mount=type=cache,target=/root/.npm \
    npm ci

COPY ./server.js ./


FROM node:20-alpine AS final
WORKDIR /app
# Change to non root user for secuirty vulnaribilies
RUN addgroup -S myGroup && adduser -S nonrootuser  -G myGroup

COPY package*.json .
RUN npm ci --only=production
COPY --from=build --chown=nonrootuser:myGroup /usr/src/app/server.js ./server.js
USER nonrootuser

CMD ["node","server.js"]


