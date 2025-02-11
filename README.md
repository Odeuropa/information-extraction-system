# Creation of folds for train/dev/test 

This script `create_folds.py` takes into input a folder with Odeuropa INCEpTION annotations exports (webanno format) and returns the data organized in 10 folds in BERT and Machamp (with labels divided in multiple tasks) formats.

To run the script you need to set the following paramethers:

`--folder`: The input folder containing INCEpTION exports

`--output`: Folder where the folds are saved

Other paramethers are:

`--tasktype`: Type of the output. It can be `BERT`(default) or `MULTITASK`

`--tags`: List of the frame elements to keep in the folds separated by `,`


Usage example:
```
python3 create_folds.py --folder en-webanno/ --output OutputDir --tasktype BERT --tags Smell\\_Word,Smell\\_Source,Quality
```



# Convert English plain texts to Bert format for the classification

Run the script `books_converter.py` on the folder containing the documents you want to use to extract frame elements and convert them in a format readable by the classifier.


`--folder`: The input folder containing the books/document (plain txt, no metadata or tags)

`--output`: The output folder for the converted documents

`--label`: A short label used to assign an ID to the documents (so that later they can be matched with the metadata)

`--books` The script allows to merge multible books into a single file, setting the value to 1 create a file for each book


The script creates a `-meta` file outside the output folder to map the document ID with the original books.


Usage example:
```
python3 books_converter.py  --folder books_folder --output output_folder --label abc --books 100
```


# Odeuropa Smell Classifier

The models are available at this link:

| https://drive.google.com/drive/u/1/folders/1IfRbjNc5nAveRaAwraLpYsZJmqS3fsSM |

After the download unzip the model file and update run_classifier.sh with the model path.

To install the required packages:

```
pip install -r requirements.txt
```    

Update run_classifier.sh with the right paths of the input and the output folders, the language, and the model path.

Example change for run_classifier.sh:

```
python smell_classifier.py --model_path "bert-base-italian-uncased-odeuropa" --input_path "input_folder" \
                           --output_path "output_folder" --lang "italian"
```

The input files found in 'input_folder' must be in CONLL format with the columns: 'Document', 'Sentence-Token Num', 'Characters', 'Word', 'Label'. No header should be included to the files. The classifier updates the 'Label' column of each file and saves it in 'output_folder'. 

To run the classifier:

```
bash run_classifier.sh
```

Note: Models are chosen for the distribution regarding their overall F1 score on the labels. 


# Extract Frame Elements from Bert prediction

The script `extract_annotations.py` takes as input the folder with the predictions from the classifier and return a tsv with the frame elements, the sentences from which they are extracted and the associated books.

`--folder`: The input folder containing the predictions of the classifier

`--output`: File to save the annotations

`--stopwords`: file containing stopwords to be ignored during the extraction (optional)

`--smellwordtag`: Label used for the smell word (usualle `Smell_Word` or `Smell\_Word`)

`--tags`: List of the frame elements to extract separated by `,`

Usage example:
```
python3 extract_annotations.py --folder [predictions_folder] --smellwordtag Smell_Word --tags Smell_Source,Quality --stopwords stopwords.txt --output out.tsv
```



## Funding acknowledgement

<img src="https://github.com/Odeuropa/.github/raw/main/profile/eu-logo.png" width="80" height="54" align="left" alt="EU logo" />

This work has been realised in the context of [Odeuropa](https://odeuropa.eu/), a research project that has received funding from the European Union’s Horizon 2020 research and innovation programme under grant agreement No. 101004469.

