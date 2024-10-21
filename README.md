# `@verumex/download-encrypted-artifact`

Upload encrypted artifact from your workflow runs. Internally powered by [@actions/download-artifact](https://github.com/actions/download-artifact).
The downloaded artifact will be decrypted using provided GPG secret keys.

See also [upload-encrypted-artifact](https://github.com/Verumex/upload-encrypted-artifact)

- [`@verumex/download-encrypted-artifact`](#)
  - [Usage](#usage)
    - [Inputs](#inputs)

  - [Examples](#examples)

## Usage

### Inputs

```yaml
- uses: Verumex/download-encrypted-artifact@v1
  with:
    # A single, or list of GPG Private (secret) keys of the encrypted artifact recipients
    # Required
    gpg_secret_key:

    # Name of the artifact to download.
    # Optional.
    name:

    # Destination path. Supports basic tilde expansion.
    # Optional. Default is $GITHUB_WORKSPACE
    path:

    # A glob pattern to the artifacts that should be downloaded.
    # Ignored if name is specified.
    # Optional.
    pattern:

    # When multiple artifacts are matched, this changes the behavior of the destination directories.
    # If true, the downloaded artifacts will be in the same directory specified by path.
    # If false, the downloaded artifacts will be extracted into individual named directories within the specified path.
    # Optional. Default is 'false'
    merge-multiple:

    # The GitHub token used to authenticate with the GitHub API.
    # This is required when downloading artifacts from a different repository or from a different workflow run.
    # Optional. If unspecified, the action will download artifacts from the current repo and the current workflow run.
    github-token:

    # The repository owner and the repository name joined together by "/".
    # If github-token is specified, this is the repository that artifacts will be downloaded from.
    # Optional. Default is ${{ github.repository }}
    repository:

    # The id of the workflow run where the desired download artifact was uploaded from.
    # If github-token is specified, this is the run that artifacts will be downloaded from.
    # Optional. Default is ${{ github.run_id }}
    run-id:
```

### Examples

Download to current working directory (`$GITHUB_WORKSPACE`):

```yaml
steps:
  - uses: Verumex/download-encrypted-artifact@v1
    with:
      gpg_secret_key: ${{ secrets.GPG_SECRET_KEY }}
      name: my-artifact

  - name: Display structure of downloaded files
    run: ls -R
```

Download to a specific directory (also supports `~` expansion):

```yaml
steps:
  - uses: Verumex/download-encrypted-artifact@v1
    with:
      gpg_secret_key: |
        ${{ secrets.GPG_SECRET_KEY1 }}
        ${{ secrets.GPG_SECRET_KEY2 }}
      name: my-artifact
      path: your/destination/dir

  - name: Display structure of downloaded files
    run: ls -R your/destination/dir
```

Download from a different repository

```yaml
steps:
  - uses: Verumex/download-encrypted-artifact@v1
    with:
      gpg_secret_key: |
        ${{ secrets.GPG_SECRET_KEY1 }}
        ${{ secrets.GPG_SECRET_KEY2 }}
      name: my-artifact
      repository: my-infrastructure-repo
      run-id: 112233445566
      github-token: ${{ secrets.MY_INFRA_REPO_PULL_TOKEN }}
```
