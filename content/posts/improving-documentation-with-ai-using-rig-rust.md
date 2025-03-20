---
title: "<div>Improving documentation with AI using Rig & Rust</div>"
date: 2025-01-12
---

## Introduction

Recently I was tasked with looking into improving the product documentation where I work. This isn't a huge task by itself, but often when you're at a startup you often have many things to juggle and different things to prioritise. While we _could_ just randomly look through each documentation page and check, that would be hugely time-consuming. Additionally as documentation increases due to product functionality, we need to make sure that the docs are as high quality as possible.

In this article, we'll build a small rudimentary tool that can download a GitHub repo of our choice, parse it for markdown files and then send each file through a pipeline that will use OpenAI to classify each documentation page as well as generating a `results.json` file that will contain a list of suggestions to improve all eligible documentation pages (assumed to be markdown files).

We will be classifying our documentation pages using the Diataxis framework, which means each page will be one of the following:

- How-to
- Tutorial
- Explanation
- Reference

Once classified, we will then ask our LLM of choice how we can improve each documentation page.

If you just want to try the project out, visit this GitHub repo then skip straight to the Usage section of this article.

## Building our docs improver

### Getting started

Before we get started, you need to make sure you have the Rust programming language installed. You will also need to ensure you have an OpenAI API key.

### Parsing GitHub Repositories

To interact with GitHub repositories, we can leverage the `octocrab` crate. The goal is to download a repository, extract its contents, and filter relevant Markdown files. Before we create our GitHub client though, we should ensure that we can parse our given repository for markdown files. We want a `File` struct that can hold a path as well as the contents of a given file and can be written as a string (for when we need to pass it to our LLM pipeline):  

```
#[derive(Debug, Clone)]
pub struct File {
    pub path: String,
    pub file_contents: String,
}

impl File {
    fn new(path: String, file_contents: String) -> Self {
        Self { path, file_contents }
    }
}

impl fmt::Display for File {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        let Self {
            path,
            file_contents,
        } = self;
        write!(f, "path: {path}nfile_contents: {file_contents}")
    }
}
```

Next, we'll create our function for parsing through a directory for markdown files. Some things to take note of here:

- If the file contents is too short, we omit the file. This means that if you're using a documentation framework (for example, Mintlify), short component files will be skipped
- We use a `Vec<PathBuf>` variable to keep a track of directories to visit

```
async fn collect_markdown_files(dir: &Path) -> Result<Vec<File>, Box<dyn std::error::Error>> {
    let mut markdown_files = Vec::new();
    let mut dirs_to_visit = vec![dir.to_path_buf()];

    while let Some(current_dir) = dirs_to_visit.pop() {
        let mut entries = read_dir(&current_dir).await?;

        while let Some(entry) = entries.next_entry().await? {
            let path = entry.path();

            if path.is_dir() {
                dirs_to_visit.push(path);
            } else if path.extension().and_then(|ext| ext.to_str()) == Some("mdx") {
                let file_contents = tokio::fs::read_to_string(&path).await.unwrap();
                if file_contents.len() > 250 {
                    markdown_files.push(File::new(path.to_str().unwrap().to_owned(), file_contents));
                }
            }
        }
    }

    Ok(markdown_files)
}
```

Once done, we'll create our `Github` struct which will hold our Github client in as well as downloading the repo we want.  

```
use std::{
    fmt,
    io::{Cursor, Read},
    path::{Path, PathBuf},
};
use flate2::read::GzDecoder;
use http_body_util::BodyExt;
use tokio::fs::read_dir;
use tokio_tar::Archive;
use octocrab::{Octocrab, OctocrabBuilder};

pub struct Github {
    octo: octocrab::Octocrab,
}

impl Github {
    pub fn new() -> Self {
        let octo = Octocrab::builder().build().unwrap();
        Self { octo }
    }

    pub async fn download_repo(
        &self,
        org: String,
        repo: String,
    ) -> Result<Vec<File>, Box<dyn std::error::Error>> {
        let repo = self.octo.repos(&org, repo);
        // get the latest commit
        let latest_commit = repo
            .list_commits()
            .send()
            .await?
            .items
            .first()
            .unwrap()
            .sha
            .clone();

        // use the latest commit to download compressed tarball
        // of the repo, then convert it to a Vec<u8>
        let tarball = repo
            .download_tarball(latest_commit)
            .await?
            .into_body()
            .collect()
            .await?
            .to_bytes()
            .to_vec();

        let mut gz_decoder = GzDecoder::new(Cursor::new(tarball));
        let mut decomp_bytes = Vec::new();
        gz_decoder.read_to_end(&mut decomp_bytes)?;

        let mut archive = Archive::new(Cursor::new(decomp_bytes));
        let temp_dir = tempfile::tempdir()?;
        archive.unpack(temp_dir.path()).await?;

        collect_markdown_files(temp_dir.path()).await
    }
}
```

### Prompt Engineering

Effective prompts are essential for guiding the LLM to produce useful outputs. There's two important agents that we want to create here, both extracting JSON from the response:

- An agent that can classify a documentation page as well as returning the selected option & filepath
- An agent that will actually write out the list of items we need to improve on per page, as well as a priority level We will be using the `extractor()` function to be able to create an agent that is able to directly extract a JSON response from an LLM response.

Firstly, we'll create our classifier agent. The classifier agent determines the type of documentation using the Diataxis framework (tutorial, how-to, explanation, reference), and we'll additionally tell it to return the original filepath that gets inputted, as-is.  

```
use serde::{Deserialize, Serialize};
use schemars::JsonSchema;

#[derive(Deserialize, Serialize, JsonSchema, Debug)]
struct ClassifierResponse {
    data_type: String,
    filepath: String,
}

let classifier_agent = openai
    .extractor::<ClassifierResponse>("gpt-4o")
    .preamble(
        "Categorise this page according to the Diataxis framework for the given input.

        Options: [tutorial, how-to, explanation, reference]

        Respond with only the option you have selected and the original filepath. Skip all prose.
"
    )
    .build();
```

Secondly, we'll need to create our advisor agent. You can see here that we focus on writing clearly and concisely, as well as things the model should focus on, any considerations and anything it needs to return. At the time of writing this, where I work uses Mintlify which supports `CodeGroup` tags for grouping code snippets together. The model may not understand this, so it has been added as a consideration.  

```
#[derive(Deserialize, Serialize, JsonSchema, Debug)]
struct Data {
    data_type: String,
    filepath: String,
    issues: Vec<Issue>,
}

#[derive(Deserialize, Serialize, JsonSchema, Debug)]
struct Issue {
    priority: String,
    content: String,
}

let advisor_agent = openai
    .extractor::<Data>("gpt-4o")
    .preamble(
        "You are now a Senior DevRel.

        Your job is to analyze the documentation found below and check whether or not the documentation is fit for purpose for someone who is looking to access information quickly and easily. The markdown code will be parsed into HTML for use on a web page. The Rust macros in code snippets do not need to be explained. If the snippet talks about something other than the product the documentation is for, assume that the user already knows how to use it and do not provide any improvement suggestions related to it.

        Focus on:
        - Things that are missing from the docs

        Considerations:
        - Do not write out any strengths and write concisely
        - <CodeGroup> tags are used to group code snippets together, so you don't need to comment on it.

        Return:
        - The document type
        - The filepath
        - Your response as a JSON list of issues
        - Additionally for each issue, return an priority level: [low, medium, high, urgent]
"
    )
    .build();
```

### Setting up the LLM Pipeline

Finally, we integrate everything into a pipeline using `rig::pipeline::new()`. We then do the following:

- Add a chain that passes the original query down the line, as well as prompting the

```
// lib.rs
use rig::pipeline::{Op, passthrough};
use rig::{pipeline, parallel};
use github::File;

pub async fn setup_pipeline(files: Vec<File>) {
    let files: Vec<String> = files.into_iter().map(|x| x.to_string()).collect();
    let openai = openai::Client::from_env();

    // .. insert previous code for creating our agents here

    let pipeline = pipeline::new()
        .chain(parallel!(passthrough(), extract(classifier_agent)))
        .map(|(query, classifier)| {
            let classifier = classifier.unwrap();
            format!(
                "Document type: {}nFilepath: {}nData: {}",
                classifier.data_type, classifier.filepath, query
            )
        })
        .chain(extract(advisor_agent));

    let result = pipeline.batch_call(files.len(), files).await;
    tokio::fs::write("result.json", serde_json::to_vec_pretty(&result).unwrap()).await.unwrap();
}
```

## Hooking it all back up

So now that we've written all the code we need to write, let's come back to our `src/bin/main.rs` file. Thanks to the work we did to set all the code up, we can keep our entrypoint file as clean as possible.

Note that we've selected `shuttle-hq/shuttle-docs` as the repository. However, this works for any repository where the primary documentation is made up of `.mdx` files.  

```
use docs_enricher::{github::Github, setup_pipeline};

#[tokio::main]
async fn main() {
    let github = Github::public_only();

    let files = github
        .download_repo("shuttle-hq".into(), "shuttle-docs".into())
        .await
        .unwrap();

    setup_pipeline(files).await;
}

```

## Usage

To use this, we need to export our `OPENAI_API_KEY` as an environment variable:  

```
export OPENAI_API_KEY=<key-goes-here>
```

Next, we use `cargo run` . And there we have it! Our program should output a `result.json` file which we can query.

If you have `jq` installed, you can query potential tickets by using the following one-liner:  

```
jq '.[] | {filepath, issue: (.issues[] | select(.priority == "{{priority}}"))}' result.json
```

## Finishing Up

Thanks for reading! Hopefully this has helped you understand how you can leverage LLMs in AI-assisted workflows.

Go to Source
