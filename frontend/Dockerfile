FROM node:20

WORKDIR /app

# Install SvelteKit
RUN npx sv create frontend-manage

WORKDIR /app/frontend-manage

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the SvelteKit app
RUN npm run build

# Start the SvelteKit app
CMD ["npm", "run", "preview"]