# Migrate-to-gcr.io-repositories-hosted-on-Artifact-Registry

## It will migrate the whole GCP GCR repo named gcr.io to GCP Artifacts having same name(it works moving same repo from GCR to Artifacts named gcr.io)
  
  **gcloud artifacts docker upgrade migrate --projects=PROJECTS**

  you need to have owner role on your gcp account for successfully completing this. Because it requires some permission on service account as well as gcp account. For having owner role on gcp account you can easily complete this task..

## Faced organizatoin level issue while working on rajat project.

  **gcloud artifacts docker upgrade migrate --projects=PROJECTS**               ----> again it needs and **owner permission** for successfully migrating repos, images and also send IAM policy to send traffic to **artifact registry**.

  I was using the same command on rajat project but it was gaving some kind of organization level restriction. I held meeting with rajat on this issue. rajat used the same command. he was also facing same issue, he just use the option (3).. details is available below.. (Do not change permissions for this repo (users may lose access to gcr.io/rajat-demo-354311)) and everything works fine. it create repos, copy images on repos and set policy to send new image traffic to artifact-registry.   summery we can use 3 options as well when facing any organziation level restriction or other restriction.

    This IAM policy will grant users the ability to perform all actions in Artifact Registry that they can currently perform in Container 
    Registry. This policy may allow access that was previously prevented by deny policies or IAM conditions.

    Warning: Generated bindings for rajat-demo-354311/gcr.io may be insufficient because you do not have access to analyze IAM for 
    the following resources: ['organizations/1076394572697']
    See https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies#required-permissions
    
     [1] Apply above policy to the rajat-demo-354311/gcr.io Artifact Registry repository
     [2] Edit policy
     [3] Do not change permissions for this repo (users may lose access to gcr.io/rajat-demo-354311)
     [4] Skip permission updates for all remaining repos (users may lose access to all remaining repos)
     [5] Exit


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

You could see the thing like...

    gcloud artifacts docker upgrade migrate --projects=world-learning-400909           --------> applying migration command
    Please enter the correct Google Cloud Project ID: world-learning-400909
    Updated property [core/project].
    Repository report for world-learning-400909:
    
    Container Registry Host  Location  Artifact Registry Repository                   ----------> starting migration
    gcr.io                   us        projects/world-learning-400909/locations/us/repositories/gcr.io
    us.gcr.io                us        projects/world-learning-400909/locations/us/repositories/us.gcr.io
    asia.gcr.io              asia      projects/world-learning-400909/locations/asia/repositories/asia.gcr.io
    eu.gcr.io                europe    projects/world-learning-400909/locations/europe/repositories/eu.gcr.io
    
    OK: All Container Registry repositories have equivalent Artifact Registry repostories.
    
    
    Copying initial images (additional images will be copied later)...
    
    Copying images for world-learning-400909... done.                                                               ----------> starting copying images                             
    
    world-learning-400909: Successfully copied 0 additional tags and 0 additional manifests. There were 85 failures.
    
    Example images that failed to copy:                                                                             -----------> fail missing images
    aretec-search@sha256:f78aeb19d4facb0f4f6f2ff9485c93184d456d61d4c26a368f74016dcf223183
    custom-neo4j@sha256:b87c335d00240dc5e5e582508eb214798033faf08867de43828c4acb0ad87693
    disearch-bison@sha256:bb715f80e3ecae5b85111d746eccbc90075b46ba3b2df5a4e5e664f640946280
    disearch-bison@sha256:bf8c227c7ee0156b6904dc3aba9c14adf64a45c082b45113339bc725dd9e2a63
    answer-proposal-prod@sha256:4a83010b0cdea657809df1f395044c53cf7e478527f99855ece3006db95d8e6e
    answer-proposal-prod@sha256:c3432d72c631317deb01113a2d9a92745531313c225ca43e5589323839311d15
    disearch-mmr@sha256:5922593ae8c9caa21fee71b9697c5b07a4bbdf4c0075dfed8fe7314424933280
    disearch-mmr@sha256:396e304f9a1ac3af3ecc52147098f422b3c120aa6fe3b1889218f902323b8a25
    disearch-mmr@sha256:1a44e8d59e8c20d02da7fac1c784d05464e860f19bef77b91d777e484dea5a84
    doc_chat@sha256:ee4ffbcfcd2f6c9501f29a1670aec3ab65ea44e0d09d9de91ba197bda12f0f88
    
    Among those failures, there are 85 image copy failures due to parts of the image missing from GCR. You may try pulling the images directly from GCR to confirm. Because the images are already currupted in GCR, there's no action required for these images.
    
    Example images that failed to copy due to missing data in GCR:
    aretec-search@sha256:f78aeb19d4facb0f4f6f2ff9485c93184d456d61d4c26a368f74016dcf223183
    custom-neo4j@sha256:b87c335d00240dc5e5e582508eb214798033faf08867de43828c4acb0ad87693
    disearch-bison@sha256:bb715f80e3ecae5b85111d746eccbc90075b46ba3b2df5a4e5e664f640946280
    disearch-bison@sha256:bf8c227c7ee0156b6904dc3aba9c14adf64a45c082b45113339bc725dd9e2a63
    answer-proposal-prod@sha256:4a83010b0cdea657809df1f395044c53cf7e478527f99855ece3006db95d8e6e
    answer-proposal-prod@sha256:c3432d72c631317deb01113a2d9a92745531313c225ca43e5589323839311d15
    disearch-mmr@sha256:5922593ae8c9caa21fee71b9697c5b07a4bbdf4c0075dfed8fe7314424933280
    disearch-mmr@sha256:396e304f9a1ac3af3ecc52147098f422b3c120aa6fe3b1889218f902323b8a25
    disearch-mmr@sha256:1a44e8d59e8c20d02da7fac1c784d05464e860f19bef77b91d777e484dea5a84
    doc_chat@sha256:ee4ffbcfcd2f6c9501f29a1670aec3ab65ea44e0d09d9de91ba197bda12f0f88
    
    Warning: Unable to confirm auth bindings for world-learning-400909/gcr.io are sufficient because you do not have access to analyze IAM for the following resources: ['organizations/607380686659']
    See https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies#required-permissions
    
    Warning: Unable to confirm auth bindings for world-learning-400909/us.gcr.io are sufficient because you do not have access to analyze IAM for the following resources: ['organizations/607380686659']
    See https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies#required-permissions
    
    Warning: Unable to confirm auth bindings for world-learning-400909/asia.gcr.io are sufficient because you do not have access to analyze IAM for the following resources: ['organizations/607380686659']
    See https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies#required-permissions
    
    Potential IAM change for eu.gcr.io repository in project world-learning-400909:
    
    bindings:
    - members:
      - serviceAccount:service-22927148231@gcp-sa-datamigration.iam.gserviceaccount.com
      role: roles/artifactregistry.reader
    - members:
      - serviceAccount:gke-sa@world-learning-400909.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@dataflow-service-producer-prod.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@firebase-rules.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gae-api-prod.google.com.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gcp-sa-aiplatform.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gcp-sa-datapipelines.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gcp-sa-discoveryengine.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gcp-sa-firestore.iam.gserviceaccount.com
      - serviceAccount:service-22927148231@gcp-sa-prod-dai-core.iam.gserviceaccount.com
      role: roles/artifactregistry.repoAdmin
    - members:
      - serviceAccount:service-22927148231@compute-system.iam.gserviceaccount.com
      role: roles/artifactregistry.writer
    
    This IAM policy will grant users the ability to perform all actions in Artifact Registry that they can currently perform in Container 
    Registry. This policy may allow access that was previously prevented by deny policies or IAM conditions.
    
    Warning: Generated bindings for world-learning-400909/eu.gcr.io may be insufficient because you do not have access to analyze 
    IAM for the following resources: ['organizations/607380686659']
    See https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies#required-permissions
    
     [1] Apply above policy to the world-learning-400909/eu.gcr.io Artifact Registry repository
     [2] Edit policy
     [3] Do not change permissions for this repo (users may lose access to eu.gcr.io/world-learning-400909)
     [4] Skip permission updates for all remaining repos (users may lose access to all remaining repos)
     [5] Exit
    Please enter your numeric choice (2):  1                                                                --------> allowing to apply policy on eu.gcr.io
    
    Applying policy to repository world-learning-400909/eu.gcr.io
    
    The next step will redirect all *gcr.io traffic to Artifact Registry. Remaining Container Registry images will be copied. During migration, Artifact Registry will serve *gcr.io requests for images it doesn't have yet by copying them from Container Registry at request time. Deleting images from *gcr.io repos in the middle of migration might not be effective.
    
    IMPORTANT: Make sure to update any relevant VPC-SC policies before migrating. Once *gcr.io is redirected to Artifact Registry, the artifactregistry.googleapis.com service will be checked for VPC-SC instead of containerregistry.googleapis.com.
    
    Projects to redirect: ['world-learning-400909']
    
    Do you want to continue (Y/n)?  Y                                                                  ----------------> allowing  for redirecting all gcr.io container registry traffic to artifact registry 
    
    *gcr.io traffic is now being served by Artifact Registry for world-learning-400909. Missing images are being copied from Container Registry
    To send traffic back to Container Registry, run:
      gcloud artifacts settings disable-upgrade-redirection --project=world-learning-400909
    
    
    Copying remaining images...
    
    Copying images for world-learning-400909... done.                                                                                            
    
    world-learning-400909: Successfully copied 0 additional tags and 0 additional manifests. There were 85 failures.
    
    Example images that failed to copy:
    aretec-search@sha256:f78aeb19d4facb0f4f6f2ff9485c93184d456d61d4c26a368f74016dcf223183
    custom-neo4j@sha256:b87c335d00240dc5e5e582508eb214798033faf08867de43828c4acb0ad87693
    answer-proposal-prod@sha256:c3432d72c631317deb01113a2d9a92745531313c225ca43e5589323839311d15
    answer-proposal-prod@sha256:4a83010b0cdea657809df1f395044c53cf7e478527f99855ece3006db95d8e6e
    disearch-bison@sha256:bf8c227c7ee0156b6904dc3aba9c14adf64a45c082b45113339bc725dd9e2a63
    disearch-bison@sha256:bb715f80e3ecae5b85111d746eccbc90075b46ba3b2df5a4e5e664f640946280
    disearch-mmr@sha256:396e304f9a1ac3af3ecc52147098f422b3c120aa6fe3b1889218f902323b8a25
    disearch-mmr@sha256:5922593ae8c9caa21fee71b9697c5b07a4bbdf4c0075dfed8fe7314424933280
    disearch-mmr@sha256:1a44e8d59e8c20d02da7fac1c784d05464e860f19bef77b91d777e484dea5a84
    doc_chat@sha256:158d24fcd02fa84dfa07684c556226fd20fd70a19f5aeb1964392fdc7b7ef92a
    
    Among those failures, there are 85 image copy failures due to parts of the image missing from GCR. You may try pulling the images directly from GCR to confirm. Because the images are already currupted in GCR, there's no action required for these images.
    
    Example images that failed to copy due to missing data in GCR:
    aretec-search@sha256:f78aeb19d4facb0f4f6f2ff9485c93184d456d61d4c26a368f74016dcf223183
    custom-neo4j@sha256:b87c335d00240dc5e5e582508eb214798033faf08867de43828c4acb0ad87693
    answer-proposal-prod@sha256:c3432d72c631317deb01113a2d9a92745531313c225ca43e5589323839311d15
    answer-proposal-prod@sha256:4a83010b0cdea657809df1f395044c53cf7e478527f99855ece3006db95d8e6e
    disearch-bison@sha256:bf8c227c7ee0156b6904dc3aba9c14adf64a45c082b45113339bc725dd9e2a63
    disearch-bison@sha256:bb715f80e3ecae5b85111d746eccbc90075b46ba3b2df5a4e5e664f640946280
    disearch-mmr@sha256:396e304f9a1ac3af3ecc52147098f422b3c120aa6fe3b1889218f902323b8a25
    disearch-mmr@sha256:5922593ae8c9caa21fee71b9697c5b07a4bbdf4c0075dfed8fe7314424933280
    disearch-mmr@sha256:1a44e8d59e8c20d02da7fac1c784d05464e860f19bef77b91d777e484dea5a84
    doc_chat@sha256:158d24fcd02fa84dfa07684c556226fd20fd70a19f5aeb1964392fdc7b7ef92a
    
    All projects had image copy failures. Continuing will disable further copying and images will be missing.
    
    Continue anyway? (y/N)?  Y                                                               ----------> it needs continous permission, asking because of some failure images 
    
    
    ***gcr.io traffic is now being fully served by Artifact Registry for world-learning-400909. Images will no longer be copied from Container Registry for this project.***
    
    The following projects are fully migrated: ['world-learning-400909']
   

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
    
