# CI/CD Workflow Implementation + Deployment

## DockerHub Steps

<a href="https://hub.docker.com/repository/docker/liku45/assignment-2-agile-avengers/general"> Public Repository </a>

- Created a public repository to store images for the client and server builds.
- Tag Management:
  - `client-commit_hash`: Client Image for a specific commit. Where commit hash ensures uniqueness of the tag, through a unique ID.
  - `server-commit_hash`: Server Image for a specific commit. Where commit hash ensures uniqueness of the tag, through a unique ID.

## GitHub Secrets Used

- DockerHub:
  - `GROUP3_DOCKERHUB_USERNAME`: DockerHub Username.
  - `GROUP3_DOCKERHUB_ACCESS_TOKEN`: DockerHub Personal Access Token.
  - `ENV_FILE_CLIENT`: Environment File for Client, is updated to point to the google cloud server instance.
  - `ENV_FILE_SERVER`: Environment File for Server.

### Building & Pushing Images

When a change is pushed to GitHub, a GitHub Action is triggered to build the new docker images. The new client and server images are then pushed to the DockerHub registry upon successful build.

- Steps taken by `docker-image.yml` in the GitHub Actions Workflow:
    1. The GitHub Action copies the `package-lock.json` and `package.json` files from the `root` folder to the `client` and `server` folders.
    2. The GitHub Action sets up the `.env` files for the `client` and `server` folders using GitHub Secrets.
    3. The GitHub Action builds the `client` and `server` images using the `Dockerfile` in the respective folders.
    4. The GitHub Action tags the images with the `client-commit_hash` and `server-commit_hash` tags.
    5. The gitHub Action uses the `docker login` command to log in to DockerHub using the `GROUP3_DOCKERHUB_USERNAME` and `GROUP3_DOCKERHUB_ACCESS_TOKEN` GitHub Secrets.
    6. The GitHub Action pushes the images to DockerHub using the `docker push` command.
<br> </br>
- Steps taken by `Dockerfile` in the `client` folder:
    1. `node:18-alpine` is installed and used as the base image.
    2. The `WORKDIR` is set to `/client/app`.
    3. The `package-lock.json` and `package.json` files are copied to the `WORKDIR`.
    4. The `npm i` command is run to install the dependencies.
    5. The `COPY` command is used to copy the rest of the files to the `WORKDIR`.
    6. The `npm run build` command is run to build the client.
    7. The port `3000` is exposed for the client.
    8. The image is run using the `npm run prod` command.
<br> </br>
- Steps taken by `Dockerfile` in the `server` folder:
    1. `node:18-alpine` is installed and used as the base image.
    2. The `WORKDIR` is set to `/server/src`.
    3. The `package-lock.json` and `package.json` files are copied to the `WORKDIR`.
    4. The `npm i` command is run to install the dependencies.
    5. The `COPY` command is used to copy the rest of the files to the `WORKDIR`.
    6. The `npm run build` command is run to build the server.
    7. The port `4000` is exposed for the server.
    8. The image is run using the `npm run dev` command.

---

## Workflow Implementation

1. Developer makes changes to the code and pushes to the `group-3` branch or they create a pull request to merge their changes into the `group-3` branch.
2. GitHub Action is triggered and follows the aforementioned steps to create Docker images for the `client` and `server` and then pushes them to DockerHub.
3. When an image is deployed, we use Google Cloud Run to deploy the image to a cloud environment.
4. After the image is deployed, we run manual tests to ensure that the image is working as expected. We are currently unable to automate this process since we are unable to run new workflows using GitHub Actions because of the following error message: `The job was not started because recent account payments have failed or your spending limit needs to be increased. Please check the 'Billing & plans' section in your settings`.

### Deploying to Google Cloud

1. The `client` and `server` images are pulled from DockerHub using the `docker pull` command.
2. The `client` and `server` images are tagged using the `docker tag` command.
3. The proper authentication is set up to ensure that the images can be deployed to Google Cloud Run.
4. The images are then deployed to Google Cloud Run using the `docker push` command.

### Run Manual Tests

1. The `client` and `server` services are opened using the Google Cloud Run URL.
2. The `client` and `server` services are tested manually using existing `POSTMAN` tests  to ensure that they are working as expected.
