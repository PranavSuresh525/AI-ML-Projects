# Challenges
The project involves using API keys to generate a response based on the user’s query on finance. The pipeline includes a RAG (retriever augmented generation) system.

The first but important challenge was deciding the right llm to use. Claude, OpenAI, and Grock have only paid versions. This left me with only Google’s Gemini. But the biggest drawback is that it allows only 20 calls per day for the free version. Thus within 2 calls my program was locked out for the day. A large amount of time was spent on optimising llm calls or using the most powerful local llm that can be acquired. But optimising llm calls complicated the pipeline too much and the most powerful local llm were still very bad in text generation. Just as I was about to settle with a weak local llm, I searched googles ai studio for models which can allow large usage when I came across the gemma models. These allow 14k calls per day and are just as powerful as Gemini as it is a beta model of Gemini 3. Thus, I settled upon using the gemma models, but decided to keep the local llm as a fallback in case the gemma model fails

The biggest challenge by far was ticker extraction. This is a crucial step as the entire pipeline after that depends on the extraction of the ticker. Thus, after intense trial and error I settled on a complex and long code which uses a helper function and 3 fallback options to prevent the 0-ticker case. This finally seemed to work. This was needed as the usual methods like finding it through yahoo finance is hard as it has a hard time recognising Indian stocks. The 3-stage code prevents this as it uses a llm to understand the context and extract the ticker. The first stage first uses a regex search, which if fails then proceeds to use yahoo API and last option is to directly extract it using llm

The news extractor function is a complicated function too as using imports like google news didn’t account for regional changes like the ones in Indian stocks. Thus, I was forced to switch to news API setup. I used an API to access news sites and collect the URL's and the site's names. This info is then fed into the RAG pipeline which extracts the information from this and generates the output

# The Final Flow
We start by doing all the pip installs and getting all the imports. This is followed by initialising the local model (a flan t5 large) and a smart one - gemma-3-27b from google. This is followed by a function which will be used throughout the program to invoke the correct llm based on the availability. 

The next series of functions gets all the necessary details, recent data, financial statements for a given ticker. The following function is the ticker validation function. Next a fetch currency function is defined to get the correct currency for a given ticker. The next function analyses market sentiments, first using a llm but if that fails proceeds to use a list of words to check the sentiment of the text. This list also has intensifiers and negators to amplify some expressions. 

The next cell contains all the parameters of the agent. This is followed by a RAG pipeline which contains embedding models and text splitters followed by context retrievers. This basically embeds all the news fed into it into vectors, which is later accessed by context retrievers.

The rag retriever smoothly calls the RAG pipeline when it needs to be used
The next cell contains the intent classifier which classes the users query into particular classes

The next 2 functions are used in ticker extraction. The first is a helper function which uses yahoo API to get the ticker when a company name is given. within it also has a ranking system to give higher priority to popular companies in case 2 company's names match or the ticker extractor takes a subsidy of the larger company. The ticker extractor following this has 3 fallback options. it first tries regex search, which if fails, calls upon the yahoo API function to get the ticker. If both of these fail it proceeds to feed the input into a llm to extract the ticker

The next 2 functions update the agent class based on if certain criteria are met
The next cell contains the news fetcher node whose working is explained in the challenge's section. The news analyser updates the agent class on the findings of the news retrieval
The response generation node is given all the information from all the functions mentioned above and uses a llm to stitch together a coherent sentence which connects all the points stored in the agent class
The build workflow uses Langraph to build a flow of information from on node to another to another through edges thus stitching together all the functions we defined till now. 
Lastly the query stock’s function is a helper function which helps initialise all the agent values and creates an agent to solve the query.

