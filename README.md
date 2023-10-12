# dagshub-demo

1. install the requirements.txt
2. dvc init
3. configure remote storage with DVC
    - mkdir metrics
    - mkdir data
    - echo "## Data will be uploaded to this folder" >> data/README.md
    - dvc get https://github.com/iterative/dataset-registry tutorials/nlp/Posts.xml.zip -o data/Posts.xml.zip
    - dvc add data/Posts.xml.zip
    - dvc remote add origin s3://dvc
    - dvc remote modify origin endpointurl https://dagshub.com/andreabasile97/dagshub-demo.s3
    - dvc remote modify origin --local access_key_id {6e3ac8f03201e07f4c0faee9317fc2fd57b6****}
    - dvc remote modify origin --local secret_access_key {6e3ac8f03201e07f4c0faee9317fc2fd57b6****}
    - git add .
    - git commit -m "Initialize dvc"
    - git push
    - dvc push -r orgin
