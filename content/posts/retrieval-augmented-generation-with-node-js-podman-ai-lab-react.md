---
title: "<div>Retrieval-augmented generation with Node.js, Podman AI Lab & React</div>"
date: 2025-03-19
---

Podman AI Lab, which integrates with Podman Desktop, provides everything you need to start developing Node.js applications that can integrate large language models and package the components in containers. It includes:

- A model server to run the large language model
- The tools need to build and run containers
- Recipes to get started quickly

This article takes you through the Podman AI Lab retrieval-augmented generation (RAG) recipe, explaining the key concepts to help you get started quickly.

## Install Podman AI Lab

Podman Desktop provides tooling with a nice graphical interface to build and run containers on Windows, macOS, and Linux. To get started, first download and install Podman Desktop from the instructions on podman-desktop.io.

Once you start Podman Desktop, you should see a suggestion to install the Podman AI Lab extension, as shown in Figure 1.

<figure>

![Picture of Podman Desktop UI suggesting install of ai lab](https://developers.redhat.com/sites/default/files/podman-desktop-ai-lab-install-2.png)

<figcaption>

Figure 1: Installing the Podman AI Lab extension from within Podman Desktop.

</figcaption>

</figure>

Install the Podman AI Lab extension as suggested. You will need a recent version of Podman AI Lab, version 1.5.0 or later. If the suggestion does not appear automatically, you can install the extension by going to the extension page (the puzzle piece in the navigation bar on the left) and then search for Podman AI Lab on the Catalog tab.

Once installed, you will see an additional icon in the navigation bar on the left, marked in Figure 2.

<figure>

![Picture of podman desktop left navigation bar with Podman AI lab icon circled.](https://developers.redhat.com/sites/default/files/podman-desktop-ai-lab-icon_0.png)

<figcaption>

Figure 2: The icon for the new Podman AI Lab extension.

</figcaption>

</figure>

## Podman AI Lab recipes

Now that you have Podman AI Lab installed, you can look at the available recipes. Select the Podman AI Lab icon in the left navigation bar and then select **Recipes Catalog** under **AI Apps**. Filter the list of recipes based on the language and select **javascript**. You should see at least two recipes, including one titled **Node.js RAG Chatbot**. See Figure 3.

<figure>

![Filtered list of recipes based on selectiong JavaScript as the language](https://developers.redhat.com/sites/default/files/filtered-javascript-recipes-2.png)

<figcaption>

Figure 3: Applying the JavaScript recipe filter.

</figcaption>

</figure>

Each recipe provides an easy way to start building an application in a particular category, along with starter code that will let you run, modify, and experiment with that application.

## What is retrieval-augmented generation (RAG)?

If you are familiar with large language models, you likely know that two key aspects of working with a model are crafting the question posed to the model and managing the context that goes along with the question. These are often covered as part of prompt engineering. Retrieval-augmented generation (RAG) is one way you can provide context that helps the model respond with the most appropriate answer. The basic concept is:

- Data that provides additional context is transformed into a format suitable for model augmentation. This often includes breaking up the data into maximum sized chunks that will later be provided as additional context to the model.
- An embedding model is used to generate set of vectors that can act as keys for the chunks of source data. The chunks of source data are then stored in a database so the data can be retrieved through a query against the matching vectors. Most commonly, the data is stored in a vector database like Chroma.
- The application is enhanced so that before passing on a query to the model, it queries the database for matching documents. The most relevant documents are then added to the context sent along with the query to the model as part of the prompt.
- The model returns an answer which in part is based on the context provided.

So why not simply pass the content of all of the documents to the model? The first reason is that the size of the context that can be passed to a model is limited. The second reason is that generating answers on information most related to the questions can result in better answers. For these reasons, we need to identify the more relevant context and pass only that subset.

When implementing RAG, it is important to consider the sensitivity of the data being used to build additional context. Often this data is proprietary and it’s important to understand that if you are using a public service like GPT-4 or some embedding implementations you might be effectively disclosing this data to the world. This is often one of the motivations for running models locally (as is done in this recipe) or within an organization. You should always ensure that you understand where the the model and embeddings (used to generated indexes for vector databases) that you are using run (locally or remote) and any implications to the privacy of your data based on where they are running.

## Node.js RAG chatbot recipe

The Node.js RAG chatbot recipe lets you quickly get started with an application that integrates a retrieval augmented generation (RAG) with a chatbot. 

Running the recipe will result in 3 deployed containers:

- A container running a large large language model (Granite by default)
- A container running a vector database (chroma)
- A container running a Next.js based application using Langchain.js and React Chatbotify that implements the LLM enabled chatbot.

### Running the recipe

To run the recipe, select **More details** on the recipe tile, **start** on the main page for the recipe, and then **Start Node.js RAG Chatbot recipe** on the start recipe page, as shown in Figure 4.

<figure>

![Start page in podman ai lab for Node.js RAG recipe](https://developers.redhat.com/sites/default/files/start-for-rag-recipe-2.png)

<figcaption>

Figure 4: Running the Node.js RAG chatbot recipe.

</figcaption>

</figure>

Note that while Granite is used by default, you can download and choose other models if you would like to experiment with others to see how they perform.

When you select **Start**, the containers for the application will be built and then started. This might take a little while. Once started, you should get confirmation that the AI app is running, as shown in Figure 5.

<figure>

![Pictures of progress of starting Node.js RAg recipe.](https://developers.redhat.com/sites/default/files/rag-recipe-started-2.png)

<figcaption>

Figure 5: The recipe shows confirmation that the AI app is running.

</figcaption>

</figure>

As you can see, based on the recipe, it has pulled the Granite model, started the inference server using the Granite model, built and started the container for the vector database (`chormadb-server`) and built and started the container for the application (`nodejs-rag-inference-app`).

We can go to the application from the **Open details** link to get to the summary of the running recipes and then use the link (the box with arrow on the right hand side, marked in Figure 6) to get to the main page of the application.

<figure>

![Picture of details for Node.js RAG application with link to open application UI circled.](https://developers.redhat.com/sites/default/files/deetails_for_rag_nodejs_0.png)

<figcaption>

Figure 6: Navigate to the chatbot application's main page.

</figcaption>

</figure>

When you select the box with the arrow, you will be shown 2 ports (Figure 7).

<figure>

![Picture of two ports you can select from for the app](https://developers.redhat.com/sites/default/files/arrow-select-nodejs-rag-2.png)

<figcaption>

Figure 7: Two ports are displayed: 41231 and 37655.

</figcaption>

</figure>

Select the bottom onem which should be for the application container; you should see the main page for the application, as shown in Figure 8.

<figure>

![Pictures of main UI page for Node.js RAG application](https://developers.redhat.com/sites/default/files/nodejs-main-page-2.png)

<figcaption>

Figure 8: The Node.js RAG DEMO application page.

</figcaption>

</figure>

This page allows you to do 3 things:

1. Ask questions to the chatbot.
2. Select and add documents that will be used by the LLM when answering questions.
3. Reset the database back to the empty state.

Start by asking the following question: `What did executive 2 say?` The answer should be something like:

```output
[The context provided does not contain any information about executive 2's statement. Therefore, I cannot answer the question based on the given context.]
```

This is because the prompt for the LLM instructs it to only use the context provided to answer the question and you have not yet added any documents that can be retrieved and used to answer the question.

Download the fake\_meeting.pdf using the download button on the page. Once you have downloaded it, use the **\+ Choose** button on the Node.js RAG Demo page to select the document (Figure 9) and add it to the documents that will be used as context for the LLM. 

<figure>

![Pictures of Node.js RAG UI selecting a document to be added to RAG context.](https://developers.redhat.com/sites/default/files/selectdoc-rag2-.png)

<figcaption>

Figure 9: Select and add the fake-meeting.pdf document.

</figcaption>

</figure>

Once the document has completed loading, ask the same question as before: `What did executive 2 say?` The chatbot should now respond with something like:

```output
[ Executive 2 agreed with Executive 1 about the importance of partnerships in a successful venture and emphasized the potential interest from companies looking for innovative ways to improve their operations and stay ahead of the competition. They also mentioned reaching out to potential clients once they have a solid proof of concept and showcasing the value of their platform.]
```

If you look at the pdf document, you can confirm that this is a summary of what Executive said in the `fake_meeting.pdf`. 

When you loaded the document, it was split up into a number of "chunks," a vector index was generated for each chunk using embeddings, and then each chunk was added to the chroma vector database using its index. 

When you asked the question the second time, the question was used to generate a vector that was used to query the vector database, the chunks which best matched that vector were returned, and finally the LLM was sent the question along with the selected chunks as context.  The LLM then generated a response based on that context. 

We will go into more detail in later sections as we look at the code being run behind the scenes.

If you select the **Reset Chroma Database** on the page and then ask the same question again, you will see that that chatbot has "forgotten" what was returned in the previous response as the content of the `fake_meeting.pdf` document is no longer available for the RAG process.

You can now experiment with adding your own documents and asking questions to see how the chatbot will use the documents provided when answering questions. You can load text, Markdown, or PDF documents.

### Looking at the running containers

If we switch to the Containers view in Podman Desktop, we can see the containers that have been started, as shown in Figure 10.

<figure>

![Picture of Containers view in Podman Desktop showing the containers for the Node.js Rag Recipe](https://developers.redhat.com/sites/default/files/ndoe-rag-containers-2.png)

<figcaption>

Figure 10: Viewing the running containers in Podman Desktop.

</figcaption>

</figure>

From this view, you can see the 3 running containers (the names will not be the exact same in your environment):

- **chromadb-server-podified-1739392032438:** The container running the chroma vector database.
- **nodejs-rag-inference-app-podified-1739392032438:** The container running the Node.js Next.js application.
- **stoic\_jennings:** The container running the Granite using `llama_cpp`.

Podman Desktop also makes it easy to inspect the containers and view the logs. For example, you can easily open a terminal in the container running the vector database and look at the environment or run commands, as shown in Figure 11.

<figure>

![Picture of the Podman Desktop UI that allows you to start a terminal for the chorma db container](https://developers.redhat.com/sites/default/files/chroma-db-terminal-2.png)

<figcaption>

Figure 11: A terminal within the Podman Desktop interface.

</figcaption>

</figure>

This makes it a lot easier to investigate what is going on in the containers if you have problems. Go ahead and poke around to explore the information that is easily available.

### Looking at the recipe

The Node.js chatbot recipe is part of the AI Lab recipes. Each recipe includes the same main ingredients: models, model servers, and AI interfaces. These are combined in various ways, packaged as container images, and can be run as pods with Podman. The recipes are grouped into different categories based on the AI functions. The current recipe groups are audio, computer vision, multimodal, and natural language processing.

The Node.js RAG chatbot recipe is under the "natural language processing" group: ai-lab-recipes/recipes/natural\_language\_processing/rag-nodejs. In addition, the recipe pulls in the template for the chroma database in ai-lab-recipes/vector-dbs/chromadb, illustrating how you can use common components across recipes.

Note

The article AI Lab Recipes dives into the details of recipes and the different ingredients that they contain. We recommend that you read through that article before looking at the ingredients in the `rag-nodejs` recipe, as it will help you understand the common ingredients and let you focus on the Node.js application specifics as you go through the `rag-nodejs` recipe.

As mentioned before, the recipe for the Node.js RAG chatbot builds and deploys 3 containers, one for the large language model served by `llama_cpp`, one for the vector database, and one for the Next.js-based application.

You can go to the recipe in GitHub (ai-lab-recipes/recipes/natural\_language\_processing/rag-nodejs), but it's even easier to open it in Visual Studio Code (VS Code) from the Start page for the recipe, as shown in Figure 12).

<figure>

![Picture of podman ai deskop extension showing button to open Node.js RAG recipe in VSCode](https://developers.redhat.com/sites/default/files/podman-desktop-open-in-vscode_0.png)

<figcaption>

Figure 12: Podman Desktop lets you open the recipe in VS Code.

</figcaption>

</figure>

If you edit the copy that is opened, you can then redeploy the application with your updates through the running recipes page.

### The application front end

The application is a fairly straightforward Next.js application with React Chatbotify in the front end. LangChain.js along with ChromaDB is being used in the back end to connect to the large language model server.

The front end (app/page.js) is as follows:

```typescript
'use client';
import io from 'socket.io-client';
import { lazy, useEffect, useState } from "react";
import { FileUpload } from 'primereact/fileupload';
import 'primereact/resources/themes/saga-blue/theme.css';
import 'primereact/resources/primereact.min.css';
import { Button } from 'primereact/button';
const Chatbot = lazy(() => import("react-chatbotify"));
function App() {
  /////////////////////////////////////
  // chatbotify flow definition
  const flow = {
    start: {
      message: 'How can I help you ?',
      path: 'get_question',
    },
    get_question: {
      message: async (params) => {
       return await answerQuestion(params.userInput);
      },
      path: 'get_question'
    },
  };
  /////////////////////////////////////
  // uses socket.io to relay question to back end
  // service and return the answer  
  async function answerQuestion(question) {
    return new Promise((resolve, reject) => {
      socket.emit('question', question);
      socket.on('answer', (answer) => {
        resolve(answer);
      });
    });
  };
  /////////////////////////////////////
  // react/next plubming
  const [isLoaded, setIsLoaded] = useState(false);
  const [socket, setSocket] = useState(undefined);
  useEffect(() => {
    setIsLoaded(true);
    // Create a socket connection
    const socket = io({ path: '/api/socket.io'});
    setSocket(socket);
    // Clean up the socket connection on unmount
    return () => {
        socket.disconnect();
    };
  }, [])
  return (
    <> 
      { isLoaded && (
        <div>
          <div>
          <h1>📚 Node.js RAG DEMO</h1>
          </div> 
          <div>  
            <hr></hr>
          </div>
          <table> 
            <tr>
              <td valign="top">
                <table>
                  <tr>Add files that will be used by the chatbot with Retrieval
                      Augmented Generation to improve answers
                  </tr>
                  <tr>&nbsp;</tr>
                  <tr>Files uploaded will be split up and added to the Chroma database</tr>
                  <tr>&nbsp;</tr>
                  <tr>
                    <div className="card">
                      <FileUpload name="file"
                              url={'api/upload'}
                              auto="true"
                              multiple
                              accept=".md,.txt,.pdf"
                              maxFileSize={1000000}
                              emptyTemplate={<p className="m-0">Drag and drop files to here to upload.</p>} />
                    </div>
                  </tr>
                  <tr>&nbsp;</tr>
                  <tr>&nbsp;</tr>
                  <tr><hr></hr></tr>
                  <tr>You can reset the Chroma database to remove all of the added documents</tr>
                  <tr>
                     <Button label="Reset Chroma Database"
                             onClick={() => fetch('api/delete')}/>
                  </tr>
                </table>
              </td>
              <td>
                <div>
                  <Chatbot options={{theme: {embedded: true, showFooter: false},
                     header: {title: 'chatbot - nodejs'},
                     chatHistory: {storageKey: 'history'}}} flow={flow}/>
                </div>
              </td>
            </tr>
          </table>
        </div>
      )}
    </>
  );
};
export default App;
```

The front end communicates with the back end both through websockets and endpoints that allow documents to be uploaded to the database and for the database to be reset.

#### Upload endpoint

The source for the API/upload endpoint is in app/api/upload/route.js. It uses the LangChain.js APIs to easily split the uploaded document and to add the split chunks to the database. It is as follows:

```typescript
import process from 'process';
import { NextResponse } from 'next/server';
import { MarkdownTextSplitter, RecursiveCharacterTextSplitter } from 'langchain/text_splitter';
import { PDFLoader } from "@langchain/community/document_loaders/fs/pdf";
import { Document } from 'langchain/document';
import { HuggingFaceTransformersEmbeddings } from '@langchain/community/embeddings/hf_transformers';
import { Chroma } from '@langchain/community/vectorstores/chroma';
const vectorDBHost = process.env.VECTORDB_HOST || '0.0.0.0';
const vectorDBPort = process.env.VECTORDB_PORT || 8000;
const vectorDBName = process.env.VECTORDB_NAME || 'nodejs_test_collection';
export async function POST(req, res) {
  /////////////////////////////////////////
  // Create the connection to the Chroma vector database
  // We use the HuggingFace transformers as since it does not need a remote
  // connect and seems to work pretty well
  const url =  `http://${vectorDBHost}:${vectorDBPort}`;
  const vectorStore = new Chroma(new HuggingFaceTransformersEmbeddings(), {
    collectionName: vectorDBName,
    url: url,
  });
  /////////////////////////////////////////
  // add files reveived to the Chroma database
  // get  the the files passed from the front end
  const fileFormData = await req.formData();
  const fileContents = fileFormData.getAll('file');
  if (!fileContents) {
    return NextResponse.json({ error: "no file received" }, { status: 400 });
  }
  // for each file load the file based on its type and use the appropriate
  // text splitter to split it up. Once split add to the vector database
  try {
    for (let i = 0; i < fileContents.length; i ++) {
      let splitDocs;
      if (fileContents[i].type === 'application/pdf') {
        const rawPDFContent = new Blob([await fileContents[i].arrayBuffer()]);
        const pdfLoader = new PDFLoader(rawPDFContent);
        const pdfContent = await pdfLoader.load();
        const splitter = await new RecursiveCharacterTextSplitter({
          chunkSize: 500,
          chunkOverlap: 50
        });
        splitDocs = await splitter.splitDocuments(pdfContent);
      } else if (fileContents[i].type === 'text/markdown') {
        const buffer = Buffer.from(await fileContents[i].arrayBuffer());
        const doc = new Document({ pageContent: buffer.toString() });
        const splitter = await new MarkdownTextSplitter({
          chunkSize: 500,
          chunkOverlap: 50
        });
        splitDocs = await splitter.splitDocuments([doc]);
      } else if (fileContents[i].type === 'text/plain') {
        const buffer = Buffer.from(await fileContents[i].arrayBuffer());
        const doc = new Document({ pageContent: buffer.toString() });
        const splitter = await new RecursiveCharacterTextSplitter({
          chunkSize: 500,
          chunkOverlap: 50
        });
        splitDocs = await splitter.splitDocuments([doc]);
      }
      if (splitDocs) {
        // it was one of the supported document type so add it to the datase
        // Otherwise we will ignore the file
        await vectorStore.addDocuments(splitDocs);
      }
    }
    return NextResponse.json({ Message: "Success", status: 201 });
  } catch (error) {
    console.log("upload failed", error);
    return NextResponse.json({ Message: "Failed", status: 500 });
  }
};
```

The uploaded document is split into chunks using one of the following, depending on the type of the uploaded document:

- `RecursiveCharacterTextSplitter`
- `MarkdownTextSplitter`

The chunks are then added to the database with the `HuggingFaceTransformersEmbedding` being used to generate the vector index for each chunk. The following part of the code shows were the embedding type is selected:

```typescript
 const url =  `http://${vectorDBHost}:${vectorDBPort}`;
  const vectorStore = new Chroma(new HuggingFaceTransformersEmbeddings(), {
    collectionName: vectorDBName,
    url: url,
  });
```

One split up, adding documents to the database using the LangChain.js APIs is as simple as this:

```typescript
      await vectorStore.addDocuments(splitDocs);
```

#### Reset endpoint

The reset endpoint simply resets the vector database so that it is empty. The code is in pages/api/delete/index.js as follows:

```typescript
import { ChromaClient } from 'chromadb';
const vectorDBHost = process.env.VECTORDB_HOST || '0.0.0.0';
const vectorDBPort = process.env.VECTORDB_PORT || 8000;
const vectorDBName = process.env.VECTORDB_NAME || 'nodejs_test_collection';
const url =  `http://${vectorDBHost}:${vectorDBPort}`;
const client = new ChromaClient({path: url});
/////////////////////////////////////////////////////
// delete and recreate the collection in Chroma when
// requested by the front end
const HANDLER = async (req, res) => {
  try {
    const collection = await client.getOrCreateCollection({name: vectorDBName});
    const result = await collection.get({include: []});
    if (result && result.ids && (result.ids.length > 0)) {
      await collection.delete({ids: result.ids});
    }
    res.statusCode = 200;
    res.statusMessage = 'Success';
    res.end("Deleted succesfully, count is:" + await collection.count());
  } catch (error) {
    res.statusCode = 500;
    res.statusMessage = 'Deletion failed';
    res.end(error.toString());
  }
};
export default HANDLER;
```

#### Chatbot back end

The chatbot back end uses LangChain.js to send the questions to the large language model being served with `llama_cpp` and then returns the response to the front end through the websocket. The source code is in pages/api/socket.io/index.js and is as follows:

```typescript
import { Server } from 'socket.io';
import { createRetrievalChain } from "langchain/chains/retrieval";
import { createStuffDocumentsChain } from "langchain/chains/combine_documents";
import { ChatPromptTemplate, MessagesPlaceholder } from '@langchain/core/prompts';
import { ChatOpenAI } from '@langchain/openai';
import { HuggingFaceTransformersEmbeddings } from '@langchain/community/embeddings/hf_transformers';
import { Chroma } from '@langchain/community/vectorstores/chroma';
const model_service = process.env.MODEL_ENDPOINT ||
                      'http://localhost:8001';
const vectorDBHost = process.env.VECTORDB_HOST || '0.0.0.0';
const vectorDBPort = process.env.VECTORDB_PORT || 8000;
const vectorDBName = process.env.VECTORDB_NAME || 'nodejs_test_collection';
/////////////////////////////////////
// Function to check if/which LLM is available
async function checkingModelService() {
  let server;
  const startTime = new Date();
  while (true) {
    try {
      let result = await fetch(`${model_service}/v1/models`);
      if (result.status === 200) {
        server = 'Llamacpp_Python';
        break;
      };
    } catch (error) {
      // container is likely not available yet
    }
    console.log('Waiting to connect to LLM');
    await new Promise(x => setTimeout(x, 100));
  };
  const endTime = new Date();
  return { details: `${server} Model Service Availablen` + 
                    `${(endTime.getSeconds() - startTime.getSeconds()) } seconds`,
           server: server
         };
};
/////////////////////////////////////
// Functions to connect with the vector database
async function getVectorDatabase() {
  const url =  `http://${vectorDBHost}:${vectorDBPort}`;
  while (true) {
    try {
      // wait until chroma db server is available
      let result = await fetch(`${url}/docs`);
      if (result.status === 200) {
        break;
      };
    } catch (error) {
      // container is likely not available yet
    }
    console.log('Waiting to connect to vector database');;
    await new Promise(x => setTimeout(x, 100));
  };
  const vectorStore = new Chroma(new HuggingFaceTransformersEmbeddings(), {
    collectionName: vectorDBName,
    url: url,
  });
  return vectorStore;
}
/////////////////////////////////////
// Functions to interact with the LLM
function createLLM(server) {
  if (server === 'Llamacpp_Python') {  
    const llm = new ChatOpenAI(
      { openAIApiKey: 'EMPTY' },
      { baseURL: `${model_service}/v1` }
    );
    return llm;
  } else {
    throw new Error('Unknown llm');
  };
};
async function createChain(server) {
  const llm = createLLM(server);
  const retriever = await (await getVectorDatabase()).asRetriever();
  // Create a prompt that tells the llm to only use the provided context
  // when answering quesions. That context will be extracted from the
  // vector database by the retrieval chain.
  const prompt =
    ChatPromptTemplate.fromTemplate(`Answer the following question based only on the provided context, if you don't know the answer say so:
    <context>
    {context}
    </context>
    Question: {input}`);
  const documentChain = await createStuffDocumentsChain({
    llm: llm,
    prompt,
  });
 // This creates a chain that will query the vector database to find
 // the documents most relevant to the question and include them in the
 // contenxt of what is sent to the llm.
 return createRetrievalChain({
    combineDocsChain: documentChain,
    retriever,
  });
};
async function answerQuestion(chain, question, sessionId) {
  const result = await chain.invoke(
    { input: question },
  );
  return result.answer;
};
/////////////////////////////////////
// socket.io handler that provides the service to which
// the front end can connect to in order to request
// answers to questions
const SocketHandler = async (req, res) => {
  if (res.socket.server.io) {
  } else {
    console.log('Socket is initializing');
    const sessions = {};
    const io = new Server(res.socket.server, { path: '/api/socket.io'});
    res.socket.server.io = io;
    const result = await checkingModelService();
    const chain = await createChain(result.server);
    console.log('Chatbot ready for connections');
    io.on('connection', (socket) => {
      socket.on('close', () => {
        sessions[socket.id] = undefined;
      });
      socket.on('question', async (question) => {
        const answer = await answerQuestion(chain, question, socket.id);
        socket.emit('answer', answer);
      });
    });
  };
  res.end();
};
export default SocketHandler;
```

The connection to the Chroma vector database is made in this line:

```typescript
  const retriever = await (await getVectorDatabase()).asRetriever();
```

A LangChain.js chain is created that uses this retriever so the most relevant documents are pulled from the database for every query:

```typescript
 // This creates a chain that will query the vector database to find
 // the documents most relevant to the question and include them in the
 // contenxt of what is sent to the llm.
 return createRetrievalChain({
    combineDocsChain: documentChain,
    retriever,
  });
};
```

The prompt which tells the LLM to only answer the question based on the context returned by the retriever is as follows:

```typescript
  const prompt =
    ChatPromptTemplate.fromTemplate(`Answer the following question based only on the provided context, if you don't know the answer say so:
    <context>
    {context}
    </context>
    Question: {input}`);
```

When the chain is invoked, the retriever provides the value of `{context}` based on what is returned by the Chroma database given the question being asked.

How closely the the model respects the direction to only use the provided context depends on the model used. For Granite 3.1, we saw earlier that it follows it strictly and only returns answers based on the context provided. 

Finally, this is the part that invokes the chain when a question is received from the front end over the websocket:

```plaintext
  const result = await chain.invoke(
    { input: question },
  );
```

## Wrapping up

Hopefully this post has given you a good introduction to Podman AI Lab, recipes, and in particular the Node.js RAG chatbot recipe. You should now be ready to experiment by extending the recipe to build your own application and to package it into containers using the ingredients provided by the recipe.

If you have not already looked at the first Node.js chatbot recipe, now might be a good time to do so. You can check out in A quick start with Large Language Models and Node.js using Podman AI Lab.

If you want to learn more about developing with large language models and Node.js, take a look at Essential AI tools for Node.js developers.

If you want to learn more about what the Red Hat Node.js team is up to in general, check these out:

- Visit our Red Hat Developer topic pages on Node.js and AI for Node.js developers.
- Download the e-book A Developer's Guide to the Node.js Reference Architecture.
- Explore the Node.js Reference Architecture on GitHub.

The post Retrieval-augmented generation with Node.js, Podman AI Lab & React appeared first on Red Hat Developer.  
  

Go to Source
