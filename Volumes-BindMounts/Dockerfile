# Use an official Node.js image
FROM node:16

# Set the working directory
WORKDIR /usr/src/app

# Copy application files
COPY package*.json ./
COPY . .

# Install dependencies
RUN npm install

# Expose the app port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
