<div style="display: flex; justify-content: space-between; align-items: center;">
  <img src="readme_images/logo_unipi.png" alt="Unipi Logo" width="110" />
  <img src="readme_images/logo-composto-CNR_ISTI.png" alt="ISTI Logo" width="400" />
  <img src="readme_images/logo_unitn.png" alt="Unitn Logo" width="140" />
  <img src="readme_images/CogNoscoLab_Logo.png" alt="CogNoscoLab Logo.png" width="100" />
</div>

------

# LLM World Of Words (LWOW)
_Katherine Abramski_, _Riccardo Improta_, _Giulio Rossetti_, _Massimo Stella_

<p align="center">
  <img src="readme_images/LWOW Logo.jpg" alt="Logo" width="300" />
</p>

The "LLM World of Words" (LWOW) is a collection of datasets of English free association norms generated by various large language models (LLMs). Currently, the collection consists of datasets generated by Mistral, LLaMA3, and Claude Haiku. The datasets are modeled after the [Small World of Words (SWOW)](https://smallworldofwords.org/en/project/) [1] English free association norms, generated by humans, consisting of over 12,000 cue words and over 3 million responses. The purpose of the LWOW datasets is to provide a way to investigate various aspects of the semantic memory of LLMs using an approach that has been applied extensively for investigating the semantic memory of humans. These datasets, together with the SWOW dataset, can be used to gain insights about similarities and differences in the language structures possessed by humans and LLMs.

-------

### What are free associations?
Free associations are implicit mental connections between words or concepts. They are typically accessed by presenting humans (or AI agents) with a cue word and then asking them to respond with the first words that come to mind. The responses represent implicit associations that connect different concepts in the mind, reflecting the semantic representations that underly patterns of thought, memory, and language. For example, given the cue word "woman", a common free association response might be "man", reflecting the associative mental relation between these two concepts.

<p align="center">
  <img src="readme_images/prompt.png" alt="validation" width="300"/>

### How can they be used?
Free associations have been extensively used in cognitive psychology and linguistics as a tool for studying language and cognitive information processing. They provide a way for researchers to understand how conceptual knowledge is organized and accessed in the mind. Free associations are often used to built network models of semantic memory by connecting cue words to their responses, as shown in the figure below. When thousands of cues and responses are connected in this way, the result is a complex network model that represents the complex organization of semantic knowledge. Such models enable the investigation of complex cognitive processes that take place within semantic memory, and can be used to study a variety of cognitive phenomena such as language learning, creativity, personality traits, and cognitive biases.

<p align="center">
  <img src="readme_images/semantic_networks.02.png" alt="validation" width="700"/>

### Validation of the datasets with semantic priming

The LWOW datasets were validated using data from the [Semantic Priming Project](https://www.montana.edu/attmemlab/spp.html) [2], which implements a lexical decision task (LDT) to study semantic priming. The semantic priming effect is the cognitive phenomenon that a target word (e.g. nurse) is more easily recognized when it is prompted by a related prime word (e.g. doctor) compared to an unrelated prime word (e.g. doctrine). We simulated the semantic priming effect within network models of semantic memory built from both the LWOW and the SWOW free association norms by implementing spreading activation processes within the networks [3]. We found that the final activation levels of prime-target pairs correlated significantly with reaction time data for the same prime-target pairs from the LDT. Specifically, the activation of a target node (e.g. nurse) is higher when a related prime node (e.g. doctor) is activated compared to an unrelated prime node (e.g. doctrine). These results demonstrate how the LWOW datasets can be used for investigating cognitive and linguistic phenomena in LLMs, demonstrating the validity of the datasets.

<p align="center">
  <img src="readme_images/spreading.png" alt="validation" width="400"/>

### Investigating gender biases
To demonstrate how this dataset can be used to investigate gender biases in LLMs compared to humans, we conducted an analysis using network models of semantic memory built from both the LWOW and the SWOW free association norms. We applied a methodology that simulates semantic priming within the networks to measure the strength of association between pairs of concepts, for example, "woman" and "forecful" vs. "man" and "forceful". We applied this methodology using a set of female-related and male-related primes, and a set of female-related and male-related targets. This analysis revealed that certain adjectives like "forceful" and "strong" are more strongly associated with certain genders, shedding light on the types of stereotypical gender biases that both humans and LLMs possess. Partial results of this analysis are shown in the heatmap below, which reflects the strength of assocation between pairs of words (primes and targets) within the Llama3 network, revealing significant gender biases.
<p align="center">
  <img src="readme_images/Llama3Heat.png" alt="validation" width="700"/>


## Technical notes
The free associations were generated (either via API or locally, depending on the LLM) by providing each LLM with a set of cue words and the following prompt: "You will be provided with an input word. Write the first 3 words you associate to it separated by a comma."

This prompt was repeated 100 times for each cue word, resulting in a dataset of 11,545 unique cues words and 3,463,500 total responses for each LLM.

### How to access and use the datasets
The LWOW datasets for Mistral, Llama3, and Haiku can be found in the "LWOW_datasets folder", which contains two subfolders. The .csv files of the processed cues and responses can be found in the "processed_datasets" folder while the .csv files of the edge lists of the semantic networks constructed from the datasets can be found in the "graphs/edge_lists" folder.

Since the LWOW datasets are intended to be used in comparison to humans, we have further processed the original SWOW dataset to create a Human dataset that is aligned with the processing that we applied to the LWOW datasets. While this human dataset is not included in this repository due to the license of the original SWOW dataset, it can be easily reproduced by running the code provided in the "reproducibility" folder. We highly encourage you to generate this dataset as it enabales a direct comparison between humans and LLMs. The Human dataset can be generated with the following steps:
- Go to the [SWOW research page](https://smallworldofwords.org/en/project/research) and download the English processed data (SWOW-EN18). Save this .csv file in the "reproducibility/data/original_datasets" folder.
- Run the python file FA_data_Cleaning.py saved in the "reproducibility" folder. This will generate a .csv of the processed Human dataset, which will be saved in the "reproducibility/data/processed_datasets" folder. Note that this python script will also regenerate the .csv files of the processed LWOW datasets (the same that are can be found in the "LWOW_datasets/processed_datasets" folder).
- Run the python file FA_build_Networks.py saved in the "reproducibility" folder. This will generate a .csv of the edge list of the semantic network constructed from the Human dataset, which will be saved in the "reproducibility/data/graphs/edge_lists" folder. Note that this python script will also regenerate the .csv files of the same edges lists of the LLM networks (the same that are can be found in the "LWOW_datasets/graphs/edge_lists" folder). This python script will also produce igraph versions of all the semantic networks.

### How to reproduce the data and analyses
To reproduce the analyses, first the required external files need to be downloaded:
- Go to the [SWOW research page](https://smallworldofwords.org/en/project/research) and download the English data SWOW-EN18. Save this .csv file with the name "SWOW-EN.R100.csv" in the "reproducibility/data/original_datasets" folder.
- Go to the [Semantic Priming Project](https://www.montana.edu/attmemlab/spp.html) and download the LDT Priming Data. Save this .csv file with the name "primingLDT_data.csv" in the "reproducibility/data/LDT_analyses" folder.

Once the files are saved in the correct folders, follow the instructions in each script, which can be found in the "reproducibility" folder. The scripts should be run in the following order:
- **FA_data_Generation.py:** generates the raw LLM datasets
- **FA_data_Cleaning.py:** processes the original SWOW dataset and the raw LLM datasets
- **FA_build_Networks.py:** builds the semantic networks from the datasets
- **FA_analyses_LDT_Gender.py** and **FA_spreadr.r:** implements spreading activation processes within the networks in order to validate the datasets and investigate gender biases

------

## Do you want to know more? Read the Preprint!

_Abramski, K., et al. (2024). The "LLM World of Words" English free association norms generated by large language models._

-----

# Further Information

### Funding & Legal

- SoBigData.it which receives funding from the European Union – NextGenerationEU – National Recovery and Resilience Plan (Piano Nazionale di Ripresa e Resilienza, PNRR) – Project: “SoBigData.it – Strengthening the Italian RI for Social Mining and Big Data Analytics” – Prot. IR0000013 – Avviso n. 3264 del 28/12/2021;
- EU NextGenerationEU programme under the funding schemes PNRR-PE-AI FAIR (Future Artificial Intelligence Research).
- The HumaneAI-Net project has received funding from the European Union’s Horizon 2020 research and innovation programme under grant agreement No 952026.
- COGNOSCO grant funded by Università di Trento (Grant ID: PS 22_27).

### Contacts

For speaking requests and enquiries, please contact:

Katherine Abramski : katherine.abramski@phd.unipi.it

Riccardo Improta : riccardo.improta@unitn.it

Giulio Rossetti : giulio.rossetti@isti.cnr.it 

Massimo Stella : massimo.stella-1@unitn.it 


### References

[1] _De Deyne, S., et al. (2019). The “Small World of Words” English word association norms for over 12,000 cue words. Behavior research methods, 51, 987-1006._ [Small World of Words Project](https://smallworldofwords.org/en/project/)

[2] _Hutchison, K. A. et al. (2013). The semantic priming project. Behavior research methods, 45, 1099-1114._ [Semantic Priming Project](https://www.montana.edu/attmemlab/spp.html)

[3] Siew, C. S. (2019). spreadr: An R package to simulate spreading activation in a network. Behavior Research Methods, 51(2), 910-929.

------

<div style="display: flex; justify-content: space-between; align-items: center;">
  <img src="readme_images/logo_unipi.png" alt="Unipi Logo" width="110" />
  <img src="readme_images/logo-composto-CNR_ISTI.png" alt="ISTI Logo" width="400" />
  <img src="readme_images/logo_unitn.png" alt="Unitn Logo" width="140" />
  <img src="readme_images/CogNoscoLab_Logo.png" alt="CogNoscoLab Logo.png" width="100" />
</div>
