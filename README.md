## Deployment: 3 ways:


### 1) Deploy directly
The most direct way is by just using cloud run directly:

`gcloud run deploy <AppName>`

We are going to be asked about allowing unauthenticated and the region.
The building process ends up updating an image to **Artifact Registry**, in a repository called **cloud-run-source-deploy** (not sure if this will change).

### 2) Using container registry

These are the instructions in the YouTube tutorial:
 
First build and storing in container registry: 
- `gcloud builds submit --tag gcr.io/<Project-Name>/<AppName>  --project=<Project-Name>`
- Example: 
    - `gcloud builds submit --tag gcr.io/woven-respect-353403/continuos-deployment  --project=woven-respect-353403`

And then deploying: 
- `gcloud run deploy <AppName> --image gcr.io/<Project-Name>/<AppName> --platform managed  --project=<Project-Name> --allow-unauthenticated --region us-central1`

### 3) Manually building into Artifact registry (recommended)

As a prerequisite we need to create an artifact repository:

- `gcloud artifacts repositories create <Repository-Name> --repository-format=docker    --location=us-central1 --description="Docker repository"`

Then we can build explicitly in this repo (this applies if we build from source)

- `gcloud builds submit --region=us-west2 --tag us-central1-docker.pkg.dev/<Project-id>/<Repository-Name>/<Image-Name>:tag1`

Finally we can deploy:

- `gcloud run deploy <App-Name> --image us-central1-docker.pkg.dev/<Project-id>/<Repository-Name>/<Image-Name>:tag1`

See:
- [Transitioning from Container Registry](https://cloud.google.com/artifact-registry/docs/transition/transition-from-gcr?_ga=2.44742244.-191396548.1655265501)
- [Build and push a Docker image with Cloud Build](https://cloud.google.com/build/docs/build-push-docker-image)
- [Changes for building and deploying in Google Cloud](https://cloud.google.com/artifact-registry/docs/transition/changes-gcp?hl=en)




gcloud iam service-accounts list --project=<Project-Name>

gcloud iam service-accounts keys create ./keys.json --iam-account email@address

gcloud auth activate-service-account --key-file=keys.json

# Useful links
* Paul Craig [blog](https://dev.to/pcraig3/quickstart-continuous-deployment-to-google-cloud-run-using-github-actions-fna)
* GitHub action [deploy-cloudrun](https://github.com/google-github-actions/deploy-cloudrun)
* Cloud Run simple Flask application [tutorial](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/python)
* [Installing Google Cloud SDK](https://cloud.google.com/sdk/docs/install)