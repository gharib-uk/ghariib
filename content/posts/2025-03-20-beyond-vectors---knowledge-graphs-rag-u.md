---
title: "Beyond Vectors - Knowledge Graphs RAG Using GenAI"
date: 2025-03-20T17:02:41.889Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Beyond Vectors - Knowledge Graphs RAG Using GenAI

<br/>

<br/>
Knowledge Graphs can reshape how we think about [Retrieval-Augmented Generation](https://www.google.com/url?q=https://www.digitalocean.com/resources/articles/rag&sa=D&source=docs&ust=1742493110309070&usg=AOvVaw1ciau07lJX6EPkdP-Q7OSM) (RAG). Vector databases are great for semantic similarity, but they often miss deeper relationships hidden in the data. By storing information as nodes and edges, a graph database surfaces context that can help Large Language Models (LLM) produce better, more grounded responses.

![Benefits of RAG Agents using Graph Databases](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Beyond-Vectors-A-Guided-Tour-of-Knowledge-Graphs-for-RAG/graph-retrieval.png)

In this tutorial, we’ll walk through how to use a graph database to power a RAG pipeline. We’ll explore ingestion steps, where we combine [Named Entity Recognition](/resources/articles/natural-language-processing) (NER) with graph modeling, then see how to build queries that fetch relevant context for your Large Language Model. By the end, you’ll have a foundation for a graph-based approach that handles both structured and unstructured data in a single workflow.

[🚀 What You’ll Learn](#what-you-ll-learn)[](#what-you-ll-learn)
----------------------------------------------------------------

In this tutorial, you’ll learn how to build a [Retrieval-Augmented Generation](https://www.google.com/url?q=https://www.digitalocean.com/community/conceptual-articles/ai-hallucinations-with-rag-and-knowledge-graphs&sa=D&source=docs&ust=1742493110310504&usg=AOvVaw3B2D0-1hka60eTx921XT2-) (RAG) agent using a graph database. We’ll cover how to ingest data into a graph database with Named Entity Recognition to create rich relationships, and then query these relationships to extract contextual snippets that drive better responses from a language model. Finally, you’ll see how to adapt the code to work with DigitalOcean’s [GenAI Agent](/products/gen-ai) or [1-Click Models](/products/ai-ml/1-click-models) using an OpenAI-compatible API, providing a clear, step-by-step guide to combining structured graph data with powerful language generation.

[🛠 What You’ll Need](#what-you-ll-need)[](#what-you-ll-need)
-------------------------------------------------------------

To make the most out of this tutorial, you should ensure you have:

-   A Linux or Mac-based Developer’s Laptop
    -   Windows Users should use a VM or Cloud Instance
-   Python Installed: version 3.10 or higher
-   (Recommended) Using a miniconda or venv virtual environment
-   [Docker](https://docs.docker.com/desktop/) (Linux or MacOS) Installed: for running a local Neo4j instance
-   Basic familiarity with shell operations
-   [Download the Dataset](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Beyond-Vectors-A-Guided-Tour-of-Knowledge-Graphs-for-RAG/bbc.zip) used in this Tutorial. [Source: BBC Full Text Document Classification](https://www.kaggle.com/datasets/shivamkushwaha/bbc-full-text-document-classification?resource=download)

[Why Choose Graph Databases for RAG?](#why-choose-graph-databases-for-rag)[](#why-choose-graph-databases-for-rag)
-----------------------------------------------------------------------------------------------------------------

RAG systems live and die by their ability to retrieve the right information. Vector stores are fast and excel at finding semantically similar passages, but they ignore the web of relationships that can matter in real-world data. For example, you might have customers, suppliers, orders, and products—each with relationships that go beyond text similarity. Graph databases track these links, letting you do multi-hop queries that answer more complex questions.

![Graph Databases for RAG Agents](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Beyond-Vectors-A-Guided-Tour-of-Knowledge-Graphs-for-RAG/graph-rag.gif)

Another big benefit is transparency. Graph structures are easier to visualize and debug. If a model cites the wrong piece of information, you can trace the node and edge connections to see where it came from. This approach reduces hallucinations, increases trust, and helps developers fix issues quickly.

### [Step 1: Setup Project Dependencies](#step-1-setup-project-dependencies)[](#step-1-setup-project-dependencies)

-   Add the Python dependencies using pip.
    
    ```
    pip install neo4j \
      requests \
      ctransformers \
      spacy \
      flask \
      openai
    ```
    
-   Create a [Neo4j](https://neo4j.com/) graph database using [Docker](https://docs.docker.com/desktop/)
    
    ```
    docker run \
        -d \
        --publish=7474:7474 --publish=7687:7687 \
        -v $HOME/neo4j/data:/data \
        -v $HOME/neo4j/logs:/logs \
        -v $HOME/neo4j/import:/var/lib/neo4j/import \
        -v $HOME/neo4j/plugins:/plugins \
        neo4j:5
    ```
    

### [Step 2: Ingest The Dataset Into Our Graph Database](#step-2-ingest-the-dataset-into-our-graph-database)[](#step-2-ingest-the-dataset-into-our-graph-database)

Before we query, we need to ingest. Below is a sample Python script that uses [spaCy for NER and Neo4j](https://spacy.io/) as a storage layer. The script loops through text files in a BBC dataset, tags the content with named entities, and creates connections in the database:

-   Ingest the dataset into `Neo4j` using the Python application below.
    
    ```
    import os
    import uuid
    import spacy
    from neo4j import GraphDatabase
    
    NEO4J_URI = "bolt://localhost:7687"
    NEO4J_USER = "<YOUR PASSWORD>"
    NEO4J_PASSWORD = "<YOUR USERNAME>"
    
    DATASET_PATH = "./bbc"  # Path to the unzipped BBC dataset folder
    
    def ingest_bbc_documents_with_ner():
      # Load spaCy for NER
      nlp = spacy.load("en_core_web_sm")
    
      driver = GraphDatabase.driver(NEO4J_URI, auth=(NEO4J_USER, NEO4J_PASSWORD))
      with driver.session() as session:
          # Optional: clear old data
          session.run("MATCH (n) DETACH DELETE n")
    
          for category in os.listdir(DATASET_PATH):
              category_path = os.path.join(DATASET_PATH, category)
              if not os.path.isdir(category_path):
                  continue  # skip non-directories
    
              for filename in os.listdir(category_path):
                  if filename.endswith(".txt"):
                      filepath = os.path.join(category_path, filename)
                      # FIX #1: handle potential £ symbol or other characters
                      # Option 1: Use a different codec
                      # with open(filepath, "r", encoding="latin-1") as f:
                      #   text_content = f.read()
                      #
                      # Option 2: Replace invalid bytes (keep utf-8):
                      with open(filepath, "r", encoding="utf-8", errors="replace") as f:
                          text_content = f.read()
    
                      # Generate a UUID in Python
                      doc_uuid = str(uuid.uuid4())
    
                      # Create (or MERGE) the Document node
                      create_doc_query = """
                      MERGE (d:Document {doc_uuid: $doc_uuid})
                      ON CREATE SET
                          d.title = $title,
                          d.content = $content,
                          d.category = $category
                      RETURN d
                      """
                      session.run(
                          create_doc_query,
                          doc_uuid=doc_uuid,
                          title=filename,
                          content=text_content,
                          category=category
                      )
    
                      # Named Entity Recognition
                      doc_spacy = nlp(text_content)
    
                      # For each entity recognized, MERGE on name+label
                      for ent in doc_spacy.ents:
                          # Skip small or numeric or purely punctuation
                          if len(ent.text.strip()) < 3:
                              continue
    
                          # Generate a unique ID for new entities
                          entity_uuid = str(uuid.uuid4())
    
                          merge_entity_query = """
                          MERGE (e:Entity { name: $name, label: $label })
                          ON CREATE SET e.ent_uuid = $ent_uuid
                          RETURN e.ent_uuid as eUUID
                          """
                          record = session.run(
                              merge_entity_query,
                              name=ent.text.strip(),
                              label=ent.label_,
                              ent_uuid=entity_uuid
                          ).single()
    
                          ent_id = record["eUUID"]
    
                          # Now create relationship by matching on doc_uuid & ent_uuid
                          rel_query = """
                          MATCH (d:Document { doc_uuid: $docId })
                          MATCH (e:Entity { ent_uuid: $entId })
                          MERGE (d)-[:MENTIONS]->(e)
                          """
                          session.run(
                              rel_query,
                              docId=doc_uuid,
                              entId=ent_id
                          )
    
      print("Ingestion with NER complete!")
    
    if __name__ == "__main__":
      ingest_bbc_documents_with_ner()
    ```
    

This code shows how to merge a Document node, link recognized entities, and store the entire structure. You can swap in your own data, too. The core idea is that once these relationships exist, you can query them to get meaningful insights, rather than just retrieving text passages.

### [Step 3: Query The RAG Agent Using Our Knowledge Graph](#step-3-query-the-rag-agent-using-our-knowledge-graph)[](#step-3-query-the-rag-agent-using-our-knowledge-graph)

After ingesting your documents, you’ll want to ask questions. The next script extracts named entities from a user query, matches those entities to the Neo4j graph, and collects top matching documents. Finally, it sends a combined context to a local language model endpoint:

-   Query the [RAG Agent](https://www.google.com/url?q=https://www.digitalocean.com/community/conceptual-articles/rag-ai-agents-agentic-rag-comparative-analysis&sa=D&source=docs&ust=1742493110310416&usg=AOvVaw19FecI-5mqAp7J5Rt5TdCV) using the Python application below.
    
    ```
    import spacy
    from neo4j import GraphDatabase
    import openai
    import os
    
    NEO4J_URI = "bolt://localhost:7687"
    NEO4J_USER = "<YOUR PASSWORD>"
    NEO4J_PASSWORD = "<YOUR USERNAME>"
    
    def connect_neo4j():
      return GraphDatabase.driver(NEO4J_URI, auth=(NEO4J_USER, NEO4J_PASSWORD))
    
    def extract_entities_spacy(text, nlp):
      doc = nlp(text)
      return [(ent.text.strip(), ent.label_) for ent in doc.ents if len(ent.text.strip()) >= 3]
    
    def fetch_documents_by_entities(session, entity_texts, top_k=5):
      if not entity_texts:
          return []
    
      query = """
      MATCH (d:Document)-[:MENTIONS]->(e:Entity)
      WHERE toLower(e.name) IN $entity_list
      WITH d, count(e) as matchingEntities
      ORDER BY matchingEntities DESC
      LIMIT $topK
      RETURN d.title AS title, d.content AS content, d.category AS category, matchingEntities
      """
      entity_list_lower = [txt.lower() for txt in entity_texts]
      results = session.run(query, entity_list=entity_list_lower, topK=top_k)
    
    
      docs = []
      for record in results:
          docs.append({
              "title": record["title"],
              "content": record["content"],
              "category": record["category"],
              "match_count": record["matchingEntities"]
          })
      return docs
    
    def generate_answer(question, context):
      """
      Replaces the local LLM server call with a DigitalOcean GenAI Agent call,
      which is OpenAI API-compatible.
      """
    
      # Build a RAG-style prompt
      prompt = f"""You are given the following context from multiple documents:
    {context}
    
    Question: {question}
    
    Please provide a concise answer.
    Answer:
    """
    
      # Example of using the ChatCompletion endpoint (Chat API)
      # If you prefer the older Completion endpoint, you can adapt similarly.
      try:
          openai_client = openai.OpenAI(
              # Comment the next 2 lines out to point to a DigitalOcean GenAI Agent
              # base_url = "https://<YOUR AGENT URL>/api/v1/",
              # api_key=os.environ.get("DIGITALOCEAN_GENAI_ACCESS_TOKEN_GENERIC"),
          )
    
          completion = openai_client.chat.completions.create(
              model="n/a",
              messages=[
                  {"role": "user",   "content": prompt}
              ],
          )
    
          return completion.choices[0].message.content
      except Exception as e:
          print("Error calling the DigitalOcean GenAI Agent:", e)
          return "Error generating answer"
    
    
    if __name__ == "__main__":
      user_query = "What do these articles say about Ernie Wise?"
      nlp = spacy.load("en_core_web_sm")
      recognized_entities = extract_entities_spacy(user_query, nlp)
      entity_texts = [ent[0] for ent in recognized_entities]
    
      driver = connect_neo4j()
      with driver.session() as session:
          docs = fetch_documents_by_entities(session, entity_texts, top_k=5)
    
      combined_context = ""
      for doc in docs:
          snippet = doc["content"][:300].replace("\n", " ")
          combined_context += f"\n---\nTitle: {doc['title']} | Category: {doc['category']}\nSnippet: {snippet}...\n"
    
      final_answer = generate_answer(user_query, combined_context)
      print("RAG-based Answer:", final_answer)
    ```
    

The flow goes like this:

-   Recognize entities in the user’s question with spaCy.
-   Match those entities in Neo4j to find relevant documents.
-   Concatenate snippets from those documents into a context string.
-   Send the context and question to your local language model.

This approach helps the model focus on precise information. Instead of searching a huge text index, you retrieve curated data based on structured relationships. That means higher-quality answers and a powerful way to handle complex queries that go beyond simple keyword matching.

To use a [GenAI Agent](/products/gen-ai) or [1-Click Models](/products/ai-ml/1-click-models) as the LLM, you can simply remove the commented out code below:

```
openai_client = openai.OpenAI(
    # Comment the next 2 lines out to point to a DigitalOcean GenAI Agent
    # base_url = "https://<YOUR AGENT URL>/api/v1/",
    # api_key=os.environ.get("DIGITALOCEAN_GENAI_ACCESS_TOKEN_GENERIC"),
)
```

[🤔 Final Thoughts](#final-thoughts)[](#final-thoughts)
-------------------------------------------------------

Graph databases add a new dimension to [RAG workflows](https://www.google.com/url?q=https://www.digitalocean.com/resources/articles/rag-vs-fine-tuning&sa=D&source=docs&ust=1742493110310469&usg=AOvVaw3NjZ7kzP0N3udxCU_bgRI4). They handle detailed relationships, reduce unhelpful answers, and allow you to track how the system arrives at a conclusion. When you pair them with entity recognition and a large language model, you create a pipeline that captures nuance and context from your data.

With these code snippets, you have a starting point for building a robust RAG agent. Feel free to expand on this design by introducing your own data, adjusting the query logic, or experimenting with additional graph features. Whether you’re creating a customer-facing chatbot or an internal analytics tool, knowledge graphs can bring clarity and depth to your AI-driven experiences.

#### [Source](https://www.digitalocean.com/community/tutorials/beyond-vectors-knowledge-graphs-and-rag)

<br/>
---
