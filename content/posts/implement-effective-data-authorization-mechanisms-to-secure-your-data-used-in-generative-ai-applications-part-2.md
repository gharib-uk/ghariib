---
title: "Implement effective data authorization mechanisms to secure your data used in generative AI applications – part 2"
date: 2025-02-06
---

In part 1 of this blog series, we walked through the risks associated with using sensitive data as part of your generative AI application. This overview provided a baseline of the challenges of using sensitive data with a non-deterministic large language model (LLM) and how to mitigate these challenges with Amazon Bedrock Agents. The next question that might come to mind is where and how to use sensitive data across LLM training, fine-tuning, vector databases, agents, and tooling. In this blog post, we build on part 1 with a more detailed discussion about data governance. Then, knowing the data governance baseline, we walk through how to use sensitive data across different data sources with the correct data authorization model. Lastly, we talk about how to implement data authorization mechanisms as part of a generative AI application that uses Retrieval Augmented Generation (RAG) as part of the architecture.

## Data governance with LLMs

In this section, we’ll look into data governance as part of the overall data security landscape in more detail than we did in part 1. Many traditional workloads rely upon structured data stores, such as relational databases, for their data source. In contrast, one of the main benefits when you build a generative AI application is the ability to gain insight from massive amounts of both structured (schema-based) and unstructured data, including logs, documents, warehouse data, and other data sources. In the past, access to the unstructured data was limited to specific applications where authorization was granted to specific principals. In this type of architecture, a frontend application makes a decision whether to authorize the user’s access to the data and then uses a single AWS Identity and Access Management (IAM) role to the backend data source, providing access to the data in an object store, data warehouse, or other location. Access to the application incorporates authorization decisions to permit or deny access to the data or a subset of data. AWS has implemented access patterns tied to principal identity, including AWS trusted identity propagation and Amazon Simple Storage Service (Amazon S3) access grants.

Managing data access for generative AI applications presents challenges when you’re interested in using multiple data sources as part of the application, due to a lack of visibility into what data exists in what location and whether there is sensitive data as part of the data source. With data stored across locations, departments, and systems, many customers don’t know what data they have within each data source. And if you don’t know what data you have, it’s difficult to determine authorization policies to govern access to this data with generative AI applications, or whether the data source should be part of the generative AI application to begin with. From a data governance perspective, you need to look across four different pillars of the process: data visibility, access control, quality assurance, and ownership. Do the data sources include customer data? Is it internal data? Is it a combination of both? Do you need to remove certain objects or documents from the data source to align to the business goals of the application you’re building? Who should be authorized to access that data if you have different authorization levels within generative AI applications? AWS provides services in this area, including AWS Glue, Amazon DataZone, and AWS Lake Formation, to govern data to use with generative AI applications. Having a grasp on data governance is a critical prerequisite to implementing the data authorization capabilities we discuss in this post.

With that, how do you securely integrate sensitive data into your generative AI applications? Let’s walk through the different locations where sensitive data can exist: LLM training and fine-tuning, vector databases, tools, and agents.

## LLM training and fine-tuning

The first location where sensitive data might reside within a generative AI application is in the LLM itself. The majority of foundation models (FMs) and LLMs are built and developed by third-party organizations, including Anthropic, Cohere, Meta, and other model providers. In these models, LLMs are becoming increasingly large, training on trillions of data points across both regular data and synthetic data created by other LLMs. However, most model providers today do not disclose the data sources used by the models because of privacy and proprietary reasons. FMs developed by third-party organizations are not trained on your private data, but if you are a large enterprise, you might train your own LLMs using sensitive data, licensed data, and public data for your use cases, or you might fine-tune existing models with additional data. This allows you to choose which data to include in training the model.

However, and as mentioned in part 1, LLMs do not make data authorization decisions, which causes challenges with granting access to different groups of principals. It is your application that will decide whether a given principal should be authorized to invoke the model. In addition, if you need to remove data from the LLM, the only way to remove training or fine-tuned data today is to retrain the model without that data. Although fine-tuning and prompt engineering can influence the completions the LLM returns, training data or fine-tuned data can be returned to whomever has access to query the model. Therefore, if you choose to fine-tune an existing model, carefully consider what data you use during training. Proprietary data that is included in training can be accessible to users who perform inference using that model. You should carefully evaluate training data to remove personally identifiable information (PII) or data that requires additional authorization above that which is required to access the model itself.

It’s important to note that there are LLM guardrails that support responsible AI mechanisms. For example, Amazon Bedrock Guardrails implementations remove certain content from prompts and completions. However, guardrails are non-deterministic and focus on filtering out harmful content, denied topics, word filters, or PII data from prompts and completions.

> **Important:** You should not rely on responsible AI mechanisms such as guardrails or built-in model safety mechanisms for your data security, because they do not use identity as a signal as part of the filtering.

## Retrieval-Augmented Generation (RAG)

The second location where sensitive data sits in generative AI applications is in vector databases. RAG implementations provide generative AI applications with access to contextual information from your organization’s private data sources to deliver relevant, accurate, and customized responses from LLMs. RAG allows you to add additional context to a prompt that is sent to an LLM and does not require you to train or fine-tune a model with your own data. When you use RAG as part of the generative AI application, you query the vector database to find documents or chunks of information similar to the principal’s prompt. Data that is returned from the vector database will be sent to the model with the original prompt as additional context for the request. For AWS services, we implement RAG using Amazon Bedrock Knowledge Bases and Amazon Q Connectors.

Figure 1 shows the RAG runtime execution flow with vector databases and models. When a user queries the application, the query is turned into embeddings that the vector database uses to find documents that are similar to the query. These documents or chunks are sent to the LLM to augment the original query from the user, so that the LLM can generate a response.

![Figure 1: RAG runtime execution flow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/31/img1-3.png)

Figure 1: RAG runtime execution flow

In order to implement strong data authorization with RAG, you need to authorize the data before sending the additional content as part of the prompt to the LLM. This can be implemented at the generative AI application or the vector database. With RAG, you build your own authorization workflow within your application and perform authorization at different granularity levels. If you authorize the access to the vector database itself, then you allow a user with access to the application access to documents within the vector database. Therefore, for example, if you have two departments (such as finance and HR), you can create two vector databases, one for finance and one for HR. Principals who have the finance entitlement will be allowed access to the finance vector store, but not the one for HR, and vice versa.

What if you want to shift authorization granularity into the vector database itself? In a different deployment, if the vector database includes documents for separate groups of principals in the vector database, the API call to the vector database must include information on the group membership for the principal making the request. For example, if HR employees have access to certain documents within the vector database, the generative AI application or vector database must authorize whether the principal has access to the data that is returned. You can implement document-level filtering in Amazon Bedrock Knowledge Bases by using the retrievalConfiguration metadata field as part of the API call. As shown in the example in the next section, with metadata filtering, you add metadata key/value pairs that the vector database uses to filter the results that are returned, similar to group membership. Because the metadata filter is part of the API request and not the prompt, threat actors cannot use prompt injections to get access to data they are not authorized to access—authorization is tied to the principal’s identity that is passed to the frontend application and the metadata filters that are passed to the RAG implementation.

In order to build secure RAG implementations, it’s important that you use the correct authorization and data governance implementation. The data sent to the LLM should include only data the principal is authorized to have access to. LLM and guardrail features are probabilistic, and therefore they should not be used to make data authorization decisions.

## Tools

A third pattern used by generative AI applications to interface with sensitive data is function or tool calling. With tools, the LLM doesn’t directly call the tool. Rather, when you send a request to an LLM, you also supply a definition for one or more tools that help the LLM generate a response. If the LLM determines it needs the tool to generate a response for the message, the LLM responds with a request for the application to call the tool. It also includes the input parameters to pass to the tool. Then, in the generative AI application, the application calls the tool on the LLM’s behalf, for example an API, an AWS Lambda function, or other software. The application continues the conversation with the LLM by providing the output from the tool as part of the prompt, and then the LLM generates a response based on the new data. This runtime execution flow is shown in Figure 2.

![Figure 2: Tools runtime execution flow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/31/img2-4.png)

Figure 2: Tools runtime execution flow

Although the LLM decides whether a tool is required, the application code must perform security checks on the parameters passed back by the LLM and make authorization decisions on what tools can be called, what permissions the tool should have, and what actions can be taken. Traditional security mechanisms still apply. For example, tools should be sandboxed so that the side effects of running the tool will not affect future invocations. In addition, parameters generated by the LLM for use by the tool should be sanitized before they are passed into the tool to help avoid potential privilege escalation or remote code execution issues (for more information, see the OWASP top 10 for LLM, Improper Output Handling).

As with the other generative AI patterns mentioned earlier, the application also makes the tool authorization decisions. Similar to RAG implementations, the generative AI application decides on the appropriate authorization implementation, including application-level authorization, group-level authorization, or user-level authorization, or passes that decision to the tool through the use of an identity token, which was part of the discussion of agents in the part 1 post. With these capabilities, you can use multiple types of data sets (sensitive data, public data) in a function call implementation. However, as with authorization decisions with APIs today, authorization decisions in generative AI applications should be made based on the identity of the principal that is accessing the generative AI application and validated as part of every call to the tool. As mentioned previously, you should not allow the LLM to decide which authorization level a principal should have access to, because this can lead to excess agency (for more information, see the OWASP Top 10 for LLM Applications, Excess Agency).

## Agents

The fourth pattern that we spoke about at length in the previous post is the use of agents. Here, we’ll discuss how to make use of multiple different data sources with agents. An agent helps principals complete multi-step actions based on principal input and data provided to the model. Agents, including Amazon Bedrock Agents, orchestrate between LLMs, data sources (RAG), software applications (tools), and principal conversations. With an agent, you choose an LLM that the agent invokes to interpret prompt input and subsequent prompts in its orchestration process, including generating follow-up steps. You configure the agent with actions, which might include eliciting clarification from the end user through additional questions, function calling for API operations, or RAG to augment the query with extra relevant context from knowledge bases. These actions are used during the orchestration process, which might take multiple steps, in order to answer the end user’s original query. These components are gathered to construct base prompts for the agent to perform orchestration until the principal request is complete, as shown in Figure 3.

![Figure 3: Agent runtime execution flow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/31/img3-4.png)

Figure 3: Agent runtime execution flow

For agents and the use of external data sources, there are some additional considerations beyond the data authorization decisions we discussed earlier. First, in order to use the right data authorization context, identity information needs to be passed to the agent as part of the generative AI API call to the agent. With Amazon Bedrock Agents, this is done by using session attributes for tools and metadata filtering for vector databases. You use these attributes as part of calling different data sources within the agent configuration.

Second, the goal of using agents is to perform a task for the principal. Unlike RAG, these tasks may include making API calls to change data or take actions on behalf of the end user (principal). This differs from other data sources discussed previously, where the implementation for data access was data retrieval. With agents, the goal is to have the autonomous orchestration perform API actions, including the add, update, and delete categories of the function. You should take additional care when deciding the authorization you give principals as part of the execution flow of the agent. One option to consider when using agents is adding validation steps. This provides the principal (user) with validation steps for the work the agent performed before the agent changes data or makes calls to APIs to perform actions with data.

Now that we’ve discussed where and how to use data with generative AI applications, let’s walk through an example with a RAG implementation.

## Data filtering and authorization with RAG

Let’s say you’re an enterprise that is interested in using a generative AI application for internal groups to retrieve information about policies and historical information. For this implementation, a single Amazon S3 data source for the vector database, which includes documents for both the Finance department and the HR department, is used as part of a RAG implementation. For our simplified example, users are interested in knowing what `SECRET_KEY` they need to use for their work. Each department has separate `SECRET_KEY` values that only users who are part of the respective groups have access to. The S3 bucket is the source of the Amazon Bedrock knowledge base, which the generative AI application uses as part of the implementation. This is shown in Figure 4.

![Figure 4: Architecture overview with Finance and HR users accessing a generative AI application](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/31/img4-4.png)

Figure 4: Architecture overview with Finance and HR users accessing a generative AI application

Without any data authorization implemented, when an HR user queries the generative AI application, the Amazon Bedrock knowledge base will return the following results when using the `Retrieve` API call. (The `Retrieve` API call allows you to call the Amazon Bedrock knowledge base and have the results sent back to the generative AI application, in comparison to the `RetrieveAndGenerate` API call, which sends the results along with the prompt to the LLM without the generative AI application seeing the results from the knowledge base call until after the LLM responds to the prompt.)

```
aws bedrock-agent-runtime retrieve 
--knowledge-base-id FF6MZUZQMQ 
--retrieval-query text="What is the SECRET_KEY?"
{
    "retrievalResults": [
        {
            "content": {
                "text": "HR SECRET_KEY is HRBOT"
            },
            "location": {
                "s3Location": {
                    "uri": "s3://amzn-s3-demo-bucket/hr/hr.txt"
                },
                "type": "S3"
            },
            "metadata": {
                "x-amz-bedrock-kb-source-uri": "s3://amzn-s3-demo-bucket/hr/hr.txt",
                "x-amz-bedrock-kb-chunk-id": "1%3A0%3A5pe-v5IBdy11OzJ9mB2-",
                "x-amz-bedrock-kb-data-source-id": "OVJKWTMXQD",
                "group": "HR"
            },
            "score": 0.50864935
        },
        {
            "content": {
                "text": "Finance SECRET_KEY is FinanceBOT"
            },
            "location": {
                "s3Location": {
                    "uri": "s3://amzn-s3-demo-bucket/finance/finance.txt"
                },
                "type": "S3"
            },
            "metadata": {
                "x-amz-bedrock-kb-source-uri": "s3://amzn-s3-demo-bucket/finance/finance.txt",
                "x-amz-bedrock-kb-chunk-id": "1%3A0%3AvVK-v5IBeX5eb0Bilm5H",
                "x-amz-bedrock-kb-data-source-id": "OVJKWTMXQD"
            },
            "score": 0.4856355
        }
    ]
}
```

As shown, the `SECRET_KEY` for both the Finance department (FinanceBOT) and the HR department (HRBOT) are returned from the knowledge base, sourced from the respective prefixes in S3. However, to follow company policy, the Finance department and HR department do not want users outside the department to gain access to information within the S3 buckets that they are not authorized to view, including PII data for employees, unreleased financial data, internal HR policies, and other information that is only for users within each department. How would you go about implementing this restriction using the proper data authorization as described here?

There are two options for the solution. First, you could create two separate vector stores, one for Finance and one for HR. When a Finance user accesses the generative AI application, the application will only request data from the Finance vector store, because the user does not have authorization to the HR vector store. When an HR user accesses the generative AI application, it’s the opposite, with the application only allowing access to the HR vector store.

The second option is using a common vector store, where you might have common data for both departments in addition to sensitive data for the use of specific groups. Metadata filtering provides the generative AI application with a way to filter out context from the vector store at the vector store itself. When you add metadata as a `*.metadata.json` file that’s associated with an S3 object, you can apply filters within the Amazon Bedrock API call to filter out data that is returned by the knowledge base. For example, you can add metadata to both objects (`hr.txt` and `finance.txt`) within S3, by adding a `hr.txt.metdata.json` file and `finance.txt.metadata.json` file within the S3 bucket. When the vector database indexes from the S3 bucket, it will pull the metadata from the S3 bucket to allow you to filter on the metadata associated with the respective file. An example of the `hr.txt.metadata.json` file is shown following, along with the `vectorSearchConfiguration` filter that is used alongside the `Retrieve` API.

```
// hr.txt.metadata.json

{
    "metadataAttributes" : { 
        "group" : "HR"
    }
}

// retrieveconfiguration.json

{
    "vectorSearchConfiguration": {
        "filter": {
            "equals": {
                "key": "group",
                "value": "HR"
            }
        }
    }
}
```

With both of these metadata files in place, you will reindex the knowledge base to associate the metadata with each file. When you call the knowledge base with the filter as part of the API call, you get the following response:

```
aws bedrock-agent-runtime retrieve 
--knowledge-base-id FF6MZUZQMQ 
--retrieval-configuration="file://retrieveconfiguration.json" 
--retrieval-query text="What is the SECRET_KEY?"
{
    "retrievalResults": [
        {
            "content": {
                "text": "HR SECRET_KEY is HRBOT"
            },
            "location": {
                "s3Location": {
                    "uri": "s3://amzn-s3-demo-bucket/hr/hr.txt"
                },
                "type": "S3"
            },
            "metadata": {
                "x-amz-bedrock-kb-source-uri": "s3://amzn-s3-demo-bucket/hr/hr.txt",
                "x-amz-bedrock-kb-chunk-id": "1%3A0%3A5pe-v5IBdy11OzJ9mB2-",
                "x-amz-bedrock-kb-data-source-id": "OVJKWTMXQD",
                "group": "HR"
            },
            "score": 0.49277097
        }
    ]
}
```

As you can see, you only receive the chunks from the HR folder, because only the `hr.txt` object has the `"group' : "HR"` metadata applied to the objects. Due to this, the generative AI application can pass these chunks along with your prompt to the LLM for the user to receive the `SECRET_KEY`. You can find more information on metadata filtering in the blog post Amazon Bedrock Knowledge Bases now supports metadata filtering to improve retrieval accuracy.

Regardless of how you assign metadata to objects within the data source, the filter used with the API call is applied after the data authorization decision is made by the generative AI application. When a user logs in to the generative AI application, the application authenticates the user to identify who the user is and what department the user is in through the use of OpenID Connect (OIDC) or OAuth2, depending on the application. This step is required if you want your generative AI application to have strong authorization policies. After the generative AI application authenticates the user, it will authorize the user and apply the filters that are required when making API calls to the Amazon Bedrock knowledge base. It’s worth repeating that it’s the application that makes the data authorization decision, and the resulting API call to the knowledge base is post-authorization. By passing metadata through a secure side channel within the API and not the prompt, this practice helps to prevent threat actors and unintended users from gaining access to data they aren’t authorized to access.

## Conclusion

Implementing the correct data authorization mechanisms is a foundational step that is required when you use sensitive data as part of generative AI applications. Depending on where the data sits as part of the generative AI application, you will need to use different implementations of data authorization, and there isn’t a one-size-fits-all solution. In this post, we walked through how to use sensitive data across these different data sources with the correct data authorization model. Then, we discussed how to implement data authorization mechanisms as part of a generative AI application and RAG by using metadata filtering. For additional information on generative AI security, take a look at other blog posts in the AWS Security Blog Channel and AWS blog posts covering generative AI.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Riggs Goodman III](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/31/goriggs_headshot1.png) Riggs Goodman III  
Riggs is a Principal Partner Solution Architect at AWS. His current focus is on AI security and data security, providing technical guidance, architecture patterns, and leadership for customers and partners to build AI workloads on AWS. Internally, Riggs focuses on driving overall technical strategy and innovation across AWS service teams to address customer and partner challenges.

Go to Source
