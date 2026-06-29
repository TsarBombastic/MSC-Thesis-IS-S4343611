# AO3 Scraper & DeepSeek Annotator &  BERToptic Topic Modeler

In this repository, you will find three notebooks designed to work sequentially:
- scraper.ipynb: Scrapes story text and metadata from Archive of Our Own (AO3).
- DeepSeek_Annotator.ipynb: Annotates the explicitness and focalization of fanfiction using the DeepSeek LLM API.
- BERTopic.ipynb: Performs topic modeling on the texts using BERTopic, DistilBERT embeddings, and DeepSeek for topic representations.

# INSTRUCTIONS

# Prerequisites
- A Google account (if using Google Colab) or a local Jupyter Notebook/Anaconda environment*.
- An active DeepSeek API Key.

# Running scraper.ipynb
1. Load the notebook in Google Colab or Jupyter Notebook/Anaconda*.
2. Configure Settings: 
   - Use the Configuration cell.
   - Set USE_EXCEL_INPUT = True if you want to scrape specific URLs from an Excel file. Make sure to upload your .xlsx file and define EXCEL_FILE_PATH (the path to the .xlsx file) and COLUMN_NAME (the column in which the URLs are present).
   - Set USE_EXCEL_INPUT = False to scrape based on the hardcoded search parameters (set by altering the search_url and params variables).
3. Run all cells sequentially. The scraper will sleep for 10 seconds between requests to avoid rate limits.
4. If using Colab, the final cell will zip your output folder into data.zip for easy downloading.

# Running DeepSeek_Annotator.ipynb
1. Ensure you have the data.zip file (or a folder of .txt files) generated from the scraper. 
2. Load DeepSeek_Annotator.ipynb in Colab or Jupyter Notebook/Anaconda*.
3. If using Colab, upload data.zip and the metadata CSV to the runtime environment.
4. - Locate the client = OpenAI(...) setup in the code.
   - Replace 'API-KEY' with your active DeepSeek API key.
5. Run the cells. The notebook will extract your text files, connect to the DeepSeek API for each file, and structure the responses.
6. Once processing is complete, download output.csv to view the annotated results.

# Running BERTopic.ipynb
1. Ensure you have an index file (in this case 00_index.csv) and the extracted folder of text files ready.
2. Point pd.read_csv() to your summary index file and modify folder_path to match the exact directory name where your .txt files reside.
3. Locate the openai.OpenAI() block and replace API_KEY with your valid DeepSeek credential.
4. Run the cells sequentially to model the topics of the supplied text files.

# NOTES
- *These files have been run on Google Colab during of the thesis, they should be able to run on Jupyter as well, running the notebooks on a local installation of Jupyter Notebook/Anaconda environment has not been tested.
- Do not remove the time.sleep(10) function in the scraper. AO3 enforces strict rate limits, and scraping too quickly will result in a broken connection. In addition to this, altering the search query to show results in descending order of Kudos might also cause a broken connection.
- If using Google Colab, remember that downloaded or processed files will be deleted once the runtime disconnects. Always download data.zip and output.csv immediately after the scripts finish running. If scraping a great amount of works, you also might run up against the maximum run time of a session.
- The annotator notebook contains a fallback try/except block to catch instances where the LLM fails to return strict JSON. These will be marked as "Error" in the resulting CSV. If this happens, you can manually set the annotated values in the .csv file.
- Because BERTopic.ipynb utilizes a transformer-based pipeline (distilbert-base-cased) for feature extraction, it is highly recommended to switch your Google Colab runtime (if you are using Google Colab) to a GPU hardware accelerator to significantly speed up calculation times.
