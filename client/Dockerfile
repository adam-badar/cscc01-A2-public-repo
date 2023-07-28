# Step 1: Base image for each service
FROM node:18-alpine AS base

# Step 2: Set the working directory
WORKDIR /client/app

COPY ../package*.json .

COPY ../../package-lock*.json .


#Step 4: Install dependencies
RUN npm i

#Step 5: Copy everything
COPY . .

#Step 6: Compile app
RUN npm run build

# Step7: expose port
EXPOSE 3000

#Run image
CMD ["npm", "run", "prod"]