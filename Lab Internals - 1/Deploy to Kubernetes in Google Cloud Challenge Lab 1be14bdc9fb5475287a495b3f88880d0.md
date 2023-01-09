# Deploy to Kubernetes in Google Cloud: Challenge Lab

## Task - 1

```jsx
source <(gsutil cat gs://cloud-training/gsp318/marking/setup_marking.sh)

# -> REPLACE "YOUR_PROJECT" with your Project ID

gcloud source repos clone valkyrie-app --project=YOUR_PROJECT

cd valkyrie-app

cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF

# -> Replace the Docker_image and Tag_name with the ones given on left side in challenge
docker build -t <DOCKER_IMAGE>:<TAG_NAME>

cd ..
cd marking
./step1_v2.sh
```

## Task - 2

```jsx
cd ..
cd valkyrie-app

# -> Replace the Docker_image and Tag_name with the ones given on left side in challenge docker build -t <DOCKER_IMAGE>:<TAG_NAME>

docker run -p 8080:8080 <DOCKER_IMAGE>:<TAG_NAME>

cd ..
cd marking
./step2_v2.sh
```

## Task - 3

```jsx
cd ..
cd valkyrie-app

# -> Replace docker_image and tag with the one given in statement.
docker tag <docker_image><docker_tag> <the_tag_given_in_statement>
docker push <the_tag_given_in_statement>
```

## Task - 4

```jsx
cd k8s
gcloud container clusters get-credentials valkyrie-dev --region us-east1-d

# -> change the place_holder image here with tag name given in above task
nano deployment.yaml

kubectl create -f deployment.yaml
kubectl create -f service.yaml

```

## Task - 5

```jsx
# -> Increase it to the number of replicas that is needed.
nano deployement.yaml

git merge origin/kurt-dev

cd ..
cd valkyrie-app

# -> Change project_name to your project ID
docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2 .
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2

# -> Change the version from 1 to 2 in the image in the first part and also at the end of the document
kubectl edit deployment valkyrie-dev

```

## Task - 6

```jsx
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo

docker ps
docker kill <container_id>

export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
```

Open Jenkins and do these

- Open web-preview and login as admin with password from last command
- Go to click credentials -> Jenkins -> Global Credentials
- Go to Click add credentials
- Select Google Service Account from metadata
- Click ok

### **Creating the Jenkins job on top left**

- Click new item
- Name valkyrie-app to Multibranch Pipeline option and click OK.
- Now go to the next page, in the Branch Sources section, click **Add Source** and select **git**.
- Paste the HTTPS clone URL of your sample-app repo in Cloud Source Repositories `https://source.developers.google.com/p/YOUR_PROJECT/r/valkyrie-app` into the Project Repository field.
- Set credentials to qwiklabs and Click on ok
- We can change color by following Command

→ Now back to the command shell

```jsx
sed -i "s/green/orange/g" source/html.go

# -> Replace YOUR_PROJECT with your Project ID
sed -i "s/YOUR_PROJECT/$GOOGLE_CLOUD_PROJECT/g" Jenkinsfile
git config --global user.email "you@example.com"
git config --global user.name "student"
git add .
git push origin master

```