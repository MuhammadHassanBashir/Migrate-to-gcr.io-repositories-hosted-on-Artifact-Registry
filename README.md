# Migrate-to-gcr.io-repositories-hosted-on-Artifact-Registry

## It will migrate the whole GCP GCR repo named gcr.io to GCP Artifacts having same name(it works moving same repo from GCR to Artifacts)
  
  gcloud artifacts docker upgrade migrate \
        --projects=PROJECTS

  you need to have owner role on your gcp account for successfully completing this. Because it requires some permission on service account as well as gcp account. For having owner role on gcp account you can easily complete this task..


## IAM role require if you donot have owner role 

  Required IAM Roles
  Ensure you have the following roles granted:
  
  For the Artifact Registry service account to copy images:
  
  Storage Object Viewer (roles/storage.objectViewer) on the Container Registry project.
  For IAM recommendations:
  
  Cloud Asset Viewer (roles/cloudasset.viewer) on the Container Registry project.
  Role Viewer (roles/iam.roleViewer) if analyzing policies with custom IAM roles.
  Service Usage Consumer (roles/serviceusage.serviceUsageConsumer) to analyze policies using the Google Cloud CLI.
  To create Artifact Registry repositories:
  
  Artifact Registry Administrator (roles/artifactregistry.admin) on the Container Registry project.
  Storage Admin (roles/storage.admin) on the Container Registry project.

**Note: it works on moving same repo from GCR name gcr.io to artifact name gcr.io**   

## Migrate to standard Artifact Registry repositories (It works for differnet repo name like from GCR gcr.io to Artifact gcr-io.. both are differnt)

   It will first create the given repo to the GCP artifacts, like in my case i need to migrate gcr.io repo from GCP GCR to GCP Artifact, so i have given gcr.io in the below command, so it creates the repo first in the GCP artifact and then copy the all images from GCP GCR gcr.io repo to newly created GCP Artifacts gcr.io.

    gcloud artifacts docker upgrade migrate \
    --from-gcr=GCR_HOSTNAME/GCR_PROJECT \         ---> GCP GCR repo name that need to migrate(in my case repo name is gcr.io)/project id
    --to-pkg-dev=AR_PROJECT/AR_REPOSITORY         ---> artifact project id where repo need to migrate/artifact repo name(in my case is gcr-io)
    
    Replace the following:
    
    GCR_HOSTNAME with the Container Registry hostname. The hostname depends on where your container images are stored:
    
    gcr.io hosts the images in the United States.
    us.gcr.io hosts the images in the United States, in a separate storage bucket from images hosted by gcr.io.
    eu.gcr.io hosts the images within member states of the European Union.
    asia.gcr.io hosts the images in Asia.
    GCR_PROJECT with your Container Registry Google Cloud project ID. If your project ID contains a colon (:), see Domain-scoped projects.
    
    AR_PROJECT with the Google Cloud project ID where you enabled the Artifact Registry API.
    
    AR_REPOSITORY with the name for your Artifact Registry repository.
    
    The migration tool completes the following steps:
    
    Creates the Artifact Registry repository if the repository doesn't already exist.
    Suggests an IAM policy for the repository, and applies the policy or skips application depending on user preference.
    Copies images in the specified Container Registry region and project to your Artifact Registry repository.
    If you only want to copy images pulled from Container Registry in the last 30 to 150 days, you can include the --recent-images=DAYS flag. Replace DAYS with the number of days, between 30 and 150, that the tool should check for pulls within.
    
    If you encounter errors or timeouts, you can safely re-run the command, and completed steps are skipped.

    Workable command:
    ----------------

     gcloud artifacts docker upgrade migrate \
     --from-gcr=gcr.io/world-learning-400909 \
     --to-pkg-dev=world-learning-400909/gcr-io
    
