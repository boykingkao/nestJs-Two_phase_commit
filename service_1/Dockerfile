# Stage 1: Build the application
FROM node:15.14.0-alpine3.13 AS build

WORKDIR /app

COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Create a smaller production image
FROM node:15.14.0-alpine3.13

WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker's layer caching
COPY --from=build /app/package*.json ./

# Copy the entire 'node_modules' folder from the build stage
COPY --from=build /app/node_modules ./node_modules

# Copy the compiled application
COPY --from=build /app/dist ./dist

EXPOSE 3000

CMD ["npm", "run", "start:prod"]
