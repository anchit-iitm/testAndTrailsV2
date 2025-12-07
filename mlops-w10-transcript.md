Here is the script for your 10-minute video.

(Start Video)

1. (vertex ai tuning page) Problem Statement "Hello, this is my Week 10 demo. The goal was to apply LLMOps principles. Instead of training a standard classifier, I used Vertex AI to fine-tune a Gemini Large Language Model to classify Iris flowers."

2. (jpyuter lab data.py/ tune.py)Approach "My approach was to test if 'Dataset Engineering' improves LLM performance. I trained two versions of the model:

v1 trained on raw numerical data.

v2 trained on human-readable descriptions (like 'large petal'), to exploit the LLM's language capabilities."

3. Cloud Setup (Show Vertex AI workbench) "I used Google Vertex AI for the heavy lifting. I did not use a GPU in my notebook; instead, I submitted Supervised Fine-Tuning (SFT) jobs to Google's managed cluster in us-central1."

4. Input Data (Show data.py) "I wrote a script to convert the Iris sklearn dataset into the specific JSONL format required by Gemini. You can see here how I map numbers to adjectives for the v2 dataset."

5. Sequence of Actions (run history cmd on terminal) "1. First, I ran prepare_llm_data.py to generate the jsonl files. 2. I uploaded these files to my Google Cloud Storage bucket. 3. I ran tune_models.py to launch the training jobs on Vertex AI. 4. Finally, I ran evaluate_final.py to send validation data to both models and compare them."

6. Code Explanation (Show eval.py) "The evaluation script uses the Vertex AI REST API. Because gemini-2.5-flash is a preview model, I had to construct raw HTTP POST requests to the endpoints URL to get predictions. I then parse the JSON response to extract the flower class name."

7. Errors Encountered (Show the terminal logs) "I faced significant challenges with Model Compatibility:

Newer models like 1.5-flash-002 were not supported for tuning in the US region.

Older models like 1.0-pro were deprecated.

I solved this by using the gemini-2.5-flash-001 preview model, but I had to debug 400 Bad Request errors by switching from the Python SDK to raw curl commands."

8. Working Demonstration (Show res.txt output) "Here is the final evaluation running. You can see it processing the 30 validation rows. It sends the prompt to the v1 endpoint and the v2 endpoint, parses the answer, and calculates accuracy."

9. Output & Results "The final output shows that Model v1 (Raw Data) achieved 53% accuracy, while Model v2 (Descriptive) achieved 33%. Surprisingly, the raw data performed better in this specific experiment."

10. Learnings "My main learning is that Vertex AI Fine-Tuning is a powerful tool but requires careful management of regions and model versions. I also learned that 'making data human-readable' doesn't always guarantee better results for an LLM; sometimes the raw data is more precise."

(End Video)
