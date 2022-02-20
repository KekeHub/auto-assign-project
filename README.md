# Auto Assign Project
[![CI][CI]][CI-status]
[![GitHub Marketplace][MarketPlace]][MarketPlace-status]
[![Mergify Status][mergify-status]][mergify]

A GitHub Action that assignes an issue or a pull requests to a organization or user project (currently `beta`).

## Usage

This is the basic usage.

```yml
on:
  issues: [opened]
  pull_request: [opened]

steps:
    - name: Assign issue to organization project
      uses: KekeHub/auto-assign-project@v1
      with:
        token: ${{ secrets.MY_GITHUB_TOKEN }}
```

Note that this can be used for either organization or user projects.
Please see the following sections for details.

## Authorization

The default workflow GitHub token `${{ secrtes.GITHUB_TOKEN }}` doesn't have enough permissions so you need to create a GitHub PAT or a GitHub App.
There are two ways to setup the client credentials.

<details><summary>GitHub PAT</summary>

### Using GitHub PAT

Please create a PAT with the following permissions.

* `repo`
* `admin:org`

Then pass it thought the `token` arguments.

```yml
steps:
    - name: Assign issue to organization project
      uses: KekeHub/auto-assign-project@v1
      with:
        token: ${{ secrets.MY_GITHUB_TOKEN }}
```

💡 Note that GitHub App described in the next sections has granular permissions and it's strongly recommended.

</details>


<details><summary>GitHub App</summary>

### Using GitHub App

Please create a GitHub App with the following permissions and install to the directory which will refer the issues or the pull requests.

* Repository
    * Issue: `Read only`
* Organization:
    * Project: `Read and write`


Then pass it thought the GitHub app arguments.

```yml
steps:
    - name: Assign issue to organization project
      uses: KekeHub/auto-assign-project@v1
      with:
        app-integration-id: ${{ secrets.MYBOT_INTEGRATION_ID }}
        app-installation-id: ${{ secrets.MYBOT_INSTALLATION_ID }}
        app-private-key: ${{ secrets.MYBOT_PRIVATE_KEY }}
```

If any of these arguments are missing, the `${{ secrets.GITHUB_TOKEN }}` will get used.

</details>

## Inputs

| NAME                  | DESCRIPTION                                                                            | TYPE     | REQUIRED | DEFAULT                                                                           |
|-----------------------|----------------------------------------------------------------------------------------|----------|----------|-----------------------------------------------------------------------------------|
| `app-installation-id` | ID of the GitHub App installation to your organization.                                | `number` | `false`  |                                                                                   |
| `app-integration-id`  | ID of the GiHub App a.k.a App ID                                                       | `number` | `false`  |                                                                                   |
| `app-private-key`     | Private key of the GitHub App.                                                         | `string` | `false`  |                                                                                   |
| `issue-id`            | ID  of the issue.                                                                      | `string` | `true`   | `${{ github.event.issue.node_id }}` or `${{ github.event.pull_request.node_id }}` |
| `owner`               | Organization or the user e.g. `KekeHub`                                                | `string` | `false`  | The workflows GitHub organization                                                 |
| `token`               | A GitHub token. If GitHub App arguments are configured, this argument will be ignored. | `string` | `false`  | `${{ github.token }}`                                                             |
| `project-id`          | ID (Number) of the project. e.g.) `1`                                                  | `number` | `true`   |                                                                                   |

## Outputs

| NAME              | DESCRIPTION                                                                                                                                                                                                                                                | TYPE     |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| `project-item-id` | ID of the item (issue or pull request) that was added to the project. Useful for later use if you want to update the fields. You can use [KekeHub/update-project-item-fields](https://github.com/KekeHub/update-project-item-fields) to update the fields. | `string` |

## Related Actions

* [KekeHub/update-project-item-fields](https://github.com/KekeHub/update-project-item-fields)
  * GitHub Action to update project item fields

## License

[MIT](LICENSE)

<!-- Badge links -->
[CI]: https://github.com/KekeHub/auto-assign-project/workflows/CI/badge.svg
[CI-status]: https://github.com/KekeHub/auto-assign-project/actions?query=workflow%3Abuild-test

[MarketPlace]: https://img.shields.io/badge/Marketplace-Assign%20Org%20Project-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=
[MarketPlace-status]: https://github.com/marketplace/actions/auto-assign-project

[mergify]: https://mergify.io
[mergify-status]: https://img.shields.io/endpoint.svg?url=https://gh.mergify.io/badges/KekeHub/auto-assign-project&style=flat
