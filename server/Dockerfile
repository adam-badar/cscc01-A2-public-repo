# Step 1: Base image for each service
FROM node:18-alpine AS base

# Step 2: Set the working directory
WORKDIR /server/src

COPY package-lock.json .


COPY package.json .

#Step 4: Install dependencies
RUN npm i

#Step 5: Copy everything
COPY . .

#Step 6: Compile app
RUN npm run build

# Step7: expose port
EXPOSE 4000

#Run image
CMD ["npm", "run", "dev"]