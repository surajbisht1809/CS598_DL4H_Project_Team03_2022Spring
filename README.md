# UIUC CS598 DL4H Final Project 2022

## Team 03
Shubhendu Bhaskar - sb59@illinois.edu

Suraj Bisht - surajb2@illinois.edu

Documentation: Final report and communication with original author is available in ``/documentation`` folder

Video Presentation: https://youtu.be/Q7Fltwmcm-k

## Reimplementation for paper *"Improving Clinical Outcome Predictions Using Convolution over Medical Entities with Multimodal Learning"*
Original paper reference: https://arxiv.org/pdf/2011.12349.pdf

Original code repo:  https://github.com/tanlab/ConvolutionMedicalNer

## Data
Publicly available MIMIC-III dataset used for all the experiments: https://physionet.org/content/mimiciii/1.4/ 


## Computational Requirements
All experiements performed using Colab Pro with TPU/GPU and High-RAM configuration.

Total duration requried for entire code run and results using pre-processed MIMIC-III data: 40 hours

Total duration with all the experments and ablations: 150 hours

## Local setup requriments and dependencies
Jupyter notebook

Python 3.7

Tensorflow v1

## Usage

1. Clone the code to local.   
```
https://github.com/surajbisht1809/CS598_DL4H_Project_Team03_2022Spring.git
cd CS598_DL4H_Project_Team03_2022Spring
```
2. Run MIMIC-Extract Pipeline as explained in https://github.com/MLforHealth/MIMIC_Extract.   
   Optionally download all_hourly_data.h5 from GCP at https://console.cloud.google.com/storage/browser/mimic_extract
   
      Pre-requisite: physionet access and GCP account

3. Copy the output file of MIMIC-Extract Pipeline named `all_hourly_data.h5` to `data` folder.

4. Run `01-Extract-Timseries-Features_LoS.ipnyb` to extract first 24 hours timeseries features from MIMIC-Extract raw data.
   
      Execution time: 10 mins
   
      Added los > 5 data points
   
   experimented data extraction for first 48 hours

5. Copy the `ADMISSIONS.csv`, `NOTEEVENTS.csv`, `ICUSTAYS.csv` files into `data` folder. 

6. Run `02-Select-SubClinicalNotes_LoS.ipynb` to select subnotes based on criteria from all MIMIC-III Notes.
   
   Execution time: 5 mins
   
   Output: sub_notes

7. Run `03-Prprocess-Clinical-Notes_LoS.ipnyb` to prepocessing notes.

   Execution time: 15 mins
   
   Output:  preprocessed_notes

8. Run `04-Apply-med7-on-Clinical-Notes_LoS.ipynb` to extract medical entities.

   Execution time: 16-20 hrs - Execution is based on for loop hence most time-consuming
   
   Output:  ner_df

9. Download pretrained embeddings into `embeddings` folder via link in given References section.

10. Run `05-Represent-Entities-With-Different-Embeddings_LoS.ipynb` to convert medical entities into word representations.

      Execution time: 30 mins
   
      Output:  six dictionary files for Wrod2Vec, Fasttext and Combined

11. Run `06-Create-Timeseries-Data_LoS.ipynb` to prepare the timeseries data to fed through GRU / LSTM.

      Execution time: 30 mins
   
      Output:  Training, validation and test data and ids

12. Run `07-Timeseries-Baseline_LoS.ipynb` to run timeseries baseline model to predict 4 different clinical tasks.

    Timeseries base models used: LSTM and GRU
    
    Execution time: 4 hours
    
    Hyperparameters: hidden unites:128 and 256 epochs:10, model patience:5, Iteration:10

13. Run `08-Multimodal-Baseline_LoS.ipynb` to run multimodal baseline to predict 4 different clinical tasks.

    Timeseries base models used: GRU with average multimodal using wordVec, fasttext and concat
    
    Execution time: 5 hours
    
    Hyperparameters: hidden unites:128 and 256 epochs:10, model patience:5, Iteration:10
    
14. Run `09-Proposed-Model.ipynb_LoS` to run proposed model to predict 4 different clinical tasks.

    Proposed model: GRU with three layers of 1D convolution for wordVec, fasttext and concat embeddings 
    
    Execution time: 8 hours
    
    Hyperparameters: hidden unites:128 and 256 epochs:10, model patience:5, Iteration:10

## Results
Results files are available in ``/results`` folder, following are summarized result tables

**Baseline vs Baseline with MultiModal**

![Alt text](/results/images/Baseline.png "Baseline Models")

**Best Baseline vs Proposed Model**

![Alt text](/results/images/Proposed.png "Proposed Model")

## References

Original Paper reference: https://arxiv.org/pdf/2011.12349.pdf

Original code repo:  https://github.com/tanlab/ConvolutionMedicalNer

Download the MIMIC-III dataset via https://mimic.physionet.org/

MIMIC-Extract implementation: https://github.com/MLforHealth/MIMIC_Extract

med7 implementation: https://github.com/kormilitzin/med7

Download Pre-trained Word2Vec & FastText embeddings: https://github.com/kexinhuang12345/clinicalBERT

Preprocessing Script: https://github.com/kaggarwal/ClinicalNotesICU
