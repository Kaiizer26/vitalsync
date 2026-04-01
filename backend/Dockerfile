# Multi-stage build : le premier stage installe les dépendances et exécute les tests
# dans un environnement isolé. Le second copie uniquement le nécessaire dans une image
# alpine légère, sans les outils de dev ni les résultats de tests.
# Intérêt sécurité : l'image finale ne contient pas Jest, supertest ni les sources de test.
# Intérêt taille : node:20-alpine pèse ~50MB contre ~350MB pour node:20. Les images alpines sont bcp plus légères.

# Stage 1: build et tests
FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm test

# Stage 2: production
FROM node:20-alpine
WORKDIR /app
COPY package.json .
#omit=dev pour ne pas intaller les dépendances de dev (jest, supertest,...)
RUN npm install --omit=dev
COPY server.js .
EXPOSE 3000
CMD ["node", "server.js"]
