#Deploy Kubernetes Challenge lab

Task 1:-

gcloud auth list
gsutil cat gs://cloud-training/gsp318/marking/setup_marking_v2.sh | bash
gcloud source repos clone valkyrie-app
cd valkyrie-app
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF
docker build -t <Docker Image>:<Tag Name> .
cd ..
cd marking
./step1_v2.sh




Task 2:-
cd ..
cd valkyrie-app
docker run -p 8080:8080 <Docker Image>:<Tag Name> &
cd ..
cd marking
./step2_v2.sh
bash ~/marking/step2_v2.sh


Task 3:-

cd ..
cd valkyrie-app

gcloud artifacts repositories create Change_repo \
    --repository-format=docker \
    --location=us-central1 \
    --description="subcribe to quciklab" \
    --async 

gcloud auth configure-docker us-central1-docker.pkg.dev

docker images

docker tag Image_ID us-central1-docker.pkg.dev/PROJECT-ID/REPOSITORY/<Docker Image>:<Tag Name>

docker push us-central1-docker.pkg.dev/PROJECT-ID/REPOSITORY/<Docker Image>:<Tag Name>



Task 4:-

sed -i s#IMAGE_HERE#us-central1-docker.pkg.dev/PROJECT-ID/REPOSITORY/<Docker Image>:<Tag Name>#g k8s/deployment.yaml

gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml


#Perform Foundational Data, ML, and AI Tasks in Google Cloud 

Task 1 ::=>>

gsutil cp gs://cloud-training/gsp323/lab.csv .

cat lab.csv

gsutil cp gs://cloud-training/gsp323/lab.schema .

cat lab.schema

---------------------------------------------------------------------------------------------------------------------------------------------------------------

Task 4 :::=>

gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------


gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com
  
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------
 
 wget https://raw.githubusercontent.com/guys-in-the-cloud/cloud-skill-boosts/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud%3A%20Challenge%20Lab/speech-request.json

---------------------------------------------------------------------------------------------------------------------------------------------------------------

curl -s -X POST -H "Content-Type: application/json" --data-binary @speech-request.json \ 
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > speech.json

---------------------------------------------------------------------------------------------------------------------------------------------------------------

gsutil cp speech.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>
---------------------------------------------------------------------------------------------------------------------------------------------------------------

gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > language.json
---------------------------------------------------------------------------------------------------------------------------------------------------------------

gsutil cp language.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>

---------------------------------------------------------------------------------------------------------------------------------------------------------------

wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/blob/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud:%20Challenge%20Lab/video-intelligence-request.json
---------------------------------------------------------------------------------------------------------------------------------------------------------------


curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @video-intelligence-request.json  > video.json
    
---------------------------------------------------------------------------------------------------------------------------------------------------------------

    
 gsutil cp video.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>

