# Git Commit Id

A Github Actions Runner based off of the Maven Git Commit ID Plugin.

## Overview

This Runner uses the current git commit history, to format a specific tag usefull for tagging docker images that are snapshots of the current commit, or work in progress.

## Prerequisites

Before getting started, make sure you have the following prerequisites:

- [Git](https://git-scm.com/downloads) installed

## Usage

To integrate the Git Commit ID plugin into your projects github action, follow these steps:

 **Add the Plugin Configuration**: Open your project's actions.yml file and add the following configuration:

    ```YAML
    name: Generate tag

    on:
    push:
        branches:
        - main

    jobs:
    build:
        name: Generate tag
        runs-on: ubuntu-latest
        steps:
        - name: Check out code
            uses: actions/checkout@v4
            with:
            fetch-tags: 'true'
            fetch-depth: '0'
        - name: Generate Git Commit ID
            id: git-commit-id
            uses: cryptex-data-solutions/git-commit-id@v1
            with:
                include_repo_name: 'true'
        

      - name: Display Git Commit ID
        run: echo "Git Commit ID: ${{ steps.git-commit-id.outputs.version_string }}"
    ```

> ***NOTE***: set the fetch-depth and fetch-tags for `actions/checkout@v4` to be sure you retrieve all commits to look up the various commit properties.

## Workflow

- Add this action to your project's github actions workflow
- Ensure that your checkout action is configured to fetch all tags
- Push a commit to your repository
- The action will generate a tag based on the current commit history
- The action will return the tag as an output variable named `version_string`
    This is read as `${{ steps.git-commit-id.outputs.version_string }}`

## Inputs
- **`include_repo_name`**: Whether to include the repository name in the version string. Default: `false`

## Outputs

- **`version_string`**: The version string to use for the current commit.
- **`latest_tag`**: The latest tag.
- **`commits_since_tag`**: The number of commits since the latest tag.
- **`short_commit_hash`**: The short commit hash.

## References

- [Maven Git Commit ID Plugin](https://github.com/ktoso/maven-git-commit-id-plugin)

## License

This project is licensed under the [MIT License](LICENSE.md).
