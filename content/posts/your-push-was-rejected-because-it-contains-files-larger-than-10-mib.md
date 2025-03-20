---
title: "Your push was rejected because it contains files larger than 10 MiB"
date: 2025-01-25
---

I am working on a AI model that have a few files larger than 10MiB of trained model. While pushing those models to huggingface repository I got an error saying the files are too large to upload. Hugging Face has a size limit for single files, and files larger than 10MiB need special handling. This means I need to change the way I upload or store these files.

This article will help you understand why this error happens and how you can fix it. You will learn how to push large files to a Hugging Face repository step by step.

## The Error

```
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 16 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (11/11), 326.86 MiB | 1.72 MiB/s, done.
Total 11 (delta 1), reused 0 (delta 0), pack-reused 0
remote: -------------------------------------------------------------------------
remote: Your push was rejected because it contains files larger than 10 MiB.
remote: Please use https://git-lfs.github.com/ to store large files.
remote: See also: https://hf.co/docs/hub/repositories-getting-started#terminal
remote:
remote: Offending files:
remote:   - my_model.safetensors (ref: refs/heads/master)
remote: -------------------------------------------------------------------------
To https://huggingface.co/tecadmin/testing-model
 ! [remote rejected] master -> master (pre-receive hook declined)

```

The error indicates that you’re trying to push a file larger than 10 MiB to your Hugging Face repository without using Git Large File Storage (LFS). Hugging Face requires files over 10 MiB to be stored using Git LFS.

## Steps to Push Large Files Using Git LFS

Here is the step by step instructions to resolve this issue:

**- Install Git LFS:**First, ensure that Git LFS (Large File Storage) is installed on your system. You can install it by running the following command:

```
git lfs install
```

5. Track Large Files: Identify the files larger than 10 MiB and track them with Git LFS. Run this command to track the specific file type or file:
    
    ```
    git lfs track "*.model"
    ```
    
    This will add a reference to .gitattributes, telling Git to use LFS for those files.
    

7. Add and Commit Changes:
    
    Add the changes to your repository, including the `.gitattributes` file, and commit them. Use the following commands:
    
    ```
    git add .gitattributes
    ```
    

9. Push to the Repository: Push your changes to the Hugging Face repository. The tracked large files will now be uploaded using Git LFS.
    
    ```
    git push
    ```
    

11. Verify File Upload: Check your Hugging Face repository to confirm that the large files have been successfully uploaded.

By following these steps, you can easily resolve the issue of uploading files larger than 10 MiB to your Hugging Face repository.

## Still Getting Same Error

If you are still facing the same issue, it may be your repository already contains a large file, you may need to remove it and re-commit using Git LFS:

```
git rm --cached my_model.safetensors
```

The post Your push was rejected because it contains files larger than 10 MiB appeared first on TecAdmin.

Go to Source
