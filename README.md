
# HelmDocs GitHub Action

> GitHub Action for generation helm-docs

This is a [GitHub action](https://developer.github.com/actions/) to generate helm docs for your Helm Charts. It makes use of [helm-docs](https://github.com/norwoodj/helm-docs).
This action runs in a Docker container on which helm-docs, git & bash are installed. 

## Usage

The following example will generate a README.md for the directories sample_chart and sample_chart2 recursively. Meaning subfolders are also affected.  <br> 
A template file is provided (README.md.gotmpl) that helm-docs can use for rendering the README.md file. For more information on this visit [helm-docs](https://github.com/norwoodj/helm-docs) documentation page. <br>
If you wish to ignore certain directories you can do so by providing the name of the dirs to the `ignored_dirs` varibale. This creates a .helmdocsignore file and pasts the directory names inside. In this case dir1 and dir2 will be pasted inside the .helmdocsignore file and therefore will be ignored.  Optionally you can create an .helmdocsignore file manually and add the directories that shall be ignored directly into it. <br> 
`git_push` defines, that changes will be pushed back to Repo. 

```yml
name: Generate Helm Chart Documentation 
on:
  push:
jobs:
  generate-helm-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: Aimee-888/HelmDocs@v5
        with: 
          # provide list of dirs to run helm-docs on, seperate by comma without a space
          src_path: sample_charts,sample_charts2
          template_file: README.md.gotmpl
          # provide name of dirs to ignore, seperate by comma without a space
          ignored_dirs: dir1,dir2
          commit_message: my custom commit message
          username: sample-username
          email: sample@mail.com
          git_push: true 

```

## Options 

The following input variable options can be configured:

|Input variable|Necessity|Description|Default|
|--------------------|--------|-----------|-------|
|`src_path`|Optional|The source path to the file(s) or folder(s) to run helm-docs on. For example `.` or `some/path`. | . | |
|`ignored_dirs`|Optional|Directories you wish to ignore. Will create an .helmdocsignore file if not existend||
|`template_file`|Optional|Provide README.md.gotmpl file to customize output. See [helm-docs](https://github.com/norwoodj/helm-docs#markdown-rendering) Markdown Rendering for more information. | [default-template](https://github.com/norwoodj/helm-docs)|
|`username`|Optional|The GitHub username to associate commits made by this GitHub action.| `github-actions-bot`|
|`email`|Optional|The email used for associating commits made by this GitHub action.| `github-actions-bot@mail.com`|
|`commit_message`|Optional|A custom git commit message.| "Updating `README.md` via Github Actions with helm-docs" |
|`git_push`|Optional|Configure whether changes shall be committed and pushed or not.|true|


