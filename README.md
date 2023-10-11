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
    - dvc remote modify origin --local access_key_id {6e3ac8f03201e07f4c0faee9317fc2fd57b6943c}
    - dvc remote modify origin --local secret_access_key 6e3ac8f03201e07f4c0faee9317fc2fd57b6943c
    - git add .
    - git commit -m "Initialize dvc"
    - git push
    - dvc push -r orgin

4. Create stage for: 
    - Data extraction: 
        - dvc stage add -n extract -d data/Posts.xml.zip -o data/Posts.xml unzip data/Posts.xml.zip -d data (Linux, MacOS)
        - dvc stage add -n extract -d data/Posts.xml.zip -o data/Posts.xml tar -xf data/Posts.xml.zip -C data (Windows)
    - Data transformation (XML to TSV):
        - dvc stage add -n transform -d code/xml_to_tsv.py -d data/Posts.xml -o data/Posts.tsv python code/xml_to_tsv.py data/Posts.xml data/Posts.tsv
    - Dataset splitting:
        - dvc stage add -n split -d code/split_train_test.py -d data/Posts.tsv -o data/Posts-train.tsv -o data/Posts-test.tsv python code/split_train_test.py 0.2 1234
    - Feature Extraction:
        - dvc stage add -n featurize -d code/featurization.py -d data/Posts-train.tsv -d data/Posts-test.tsv -o data/matrix-train.pkl -o data/matrix-test.pkl python code/featurization.py
    - Training:
        - dvc stage add -n train -d code/train_model.py -d data/matrix-train.pkl -o data/model.pkl python code/train_model.py 1234 
    - Evaluation:
        - dvc stage add -n evaluate -d code/evaluate.py -d data/model.pkl -d data/matrix-test.pkl -M metrics/auc.metric python code/evaluate.py
