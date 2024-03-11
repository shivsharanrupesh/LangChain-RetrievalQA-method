# LangChain-RetrievalQA-method

Ever Wondered How to Seamlessly Integrate Documents into LLMs (Large Language Models) with LangChain?

Here are practical examples of different methods to send documents/chunks to an LLM using the LangChain RetrievalQA method:

◾RetrievalQA :
This method allows for a question-answering *chain* by retrieving embedded data from the vectorstore and passing it through our LLMs. 

The goal is to use the new documents as context to allow the LLMs to provide answers based on this newly acquired knowledge.

There are different ways to send (𝐜𝐡𝐚𝐢𝐧𝐢𝐧𝐠) the docs to the LLMs: *chain_type:*

▪ 𝐒𝐭𝐮𝐟𝐟 : the base chain:
It processes a list of documents by combining them into a single prompt and then submits that combined prompt to a language model.

It's well-suited for applications where documents are small.

▪ 𝐌𝐚𝐩_𝐫𝐞𝐝𝐮𝐜𝐞
The RetrievalQA using mapreduce makes several calls to the openAI model, each call corresponds to a document (by default 4 documents).
Then, it gathers a summary of the 4 calls, to make a final call and return the final answer.
𝘊𝘰𝘯𝘴:
When using map_reduce, since we send each chunk separately to the LLM, there is a possibility that our question's answer might be divided between 2 different chunks (at the end of one chunk and the beginning of another). This could result in the LLM being unable to find a relevant answer, leading to responses like "I don't know"...

▪ 𝐑𝐞𝐟𝐢𝐧𝐞:
We can improve map-reduce results, by using this other chain type "refine", which *𝐦𝐚𝐤𝐞𝐬 𝐬𝐞𝐪𝐮𝐞𝐧𝐭𝐢𝐚𝐥 𝐜𝐚𝐥𝐥𝐬 𝐭𝐨 𝐭𝐡𝐞 𝐎𝐩𝐞𝐧𝐀𝐈 𝐀𝐏𝐈.*

With this chain, we also call 4 times the OpenAI Chat API, but with different way than map_reduce:

At each time we call the LLM, we give: 
*   The current document + 
*   the LLM's answer from the previous call with the previous document + 
*   Adapt the prompt template to ask explicitly the LLM to *𝐫𝐞𝐟𝐢𝐧𝐞* the answer with the new context (current document)

▪ 𝐌𝐚𝐩_𝐫𝐞𝐫𝐚𝐧𝐤
With map rerank, we also call the LLMs multiple times (=number of documents). The difference with the other methods, is that we specify in the prompt to answer the question and also , specifically, *𝐬𝐜𝐨𝐫𝐞 𝐭𝐡𝐞 𝐚𝐧𝐬𝐰𝐞𝐫 "𝐇𝐨𝐰 𝐜𝐞𝐫𝐭𝐚𝐢𝐧 𝐢𝐬 𝐭𝐡𝐞 𝐋𝐌𝐌 𝐢𝐧 𝐢𝐭𝐬 𝐚𝐧𝐬𝐰𝐞𝐫"; Then the answer with the **highest score is returned* as the final answer.


In the notebook, you'll find and end-to-end example that covers loading a PDF, chunking, embedding and storing in Vectorstore, through to chatting with GPT-3.5-Turbo using the different methods explained above. Also, uncover the underlying template used in LangChain and learn how you can customize it: ⬇ 
