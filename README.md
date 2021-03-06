# godot-ci
Docker image to export Godot Engine games and deploy to GitLab/GitHub Pages and Itch.io using GitLab CI and GitHub Actions.

<img src="https://i.imgur.com/3z4Sxhd.png" width=450>

## Docker Hub
https://hub.docker.com/r/barichello/godot-ci/

## How To Use

`.gitlab-ci.yml` and `.github/workflows/godot-ci.yml` are included in this project as reference.
<br>For live projects, examples and tutorials using this template check the list below:<br>

- [Video tutorial by Kyle Luce](https://www.youtube.com/watch?v=wbc1qut0vT4)
- Repository examples: [test-project](https://github.com/aBARICHELLO/godot-ci/tree/master/test-project) | [game-off](https://gitlab.com/BARICHELLO/game-off).
- Test deploys using this tool: [GitHub Pages](http://barichello.me/godot-ci/) | [GitLab Pages](https://barichello.gitlab.io/godot-ci/) | [Itch.io](https://barichello.itch.io/test-project).

## Platforms

Here's a mapping between each supported CI service, the template jobs and a live example.

|CI|Template|Example
|-|-|-|
|GitLab CI|[Godot Exports](https://github.com/aBARICHELLO/godot-ci/blob/master/.gitlab-ci.yml#L16-L58) / [GitHub Pages](https://github.com/aBARICHELLO/godot-ci/blob/master/.gitlab-ci.yml#L60-L76) / [GitLab Pages](https://github.com/aBARICHELLO/godot-ci/blob/master/.gitlab-ci.yml#L78-L91) / [Itch.io](https://github.com/aBARICHELLO/godot-ci/blob/master/.gitlab-ci.yml#L93-L113)|[GitLab CI Pipelines](https://gitlab.com/BARICHELLO/godot-ci/pipelines)
|GitHub Actions|[Godot Exports](https://github.com/aBARICHELLO/godot-ci/blob/master/.github/workflows/godot-ci.yml#L8-99) | [GitHub Actions running](https://github.com/aBARICHELLO/godot-ci/actions)

## Environment configuration

First you need to remove unused jobs/stages from the `.yml` file you are using as a template(`.gitlab-ci.yml` or `.github/workflows/godot-ci.yml`).<br>
Then you have to add these environments to a configuration panel depending on the chosen CI and jobs:
- **GitHub**: `https://github.com/<username>/<project-name>/settings/secrets`
- **GitLab**: `https://gitlab.com/<username>/<repo-name>/settings/ci_cd`

### GitHub Pages

Secrets needed for a GitHub Pages deploy via GitLab CI:

|Variable|Description|Example|
|-|-|-|
| REMOTE_URL | The `git remote` where the web export will be hosted (in this case GitHub), it should contain your [deploy/personal access token](https://github.com/settings/tokens)|`https://<github username>:<deploy token>@github.com/<username>/<repository>.git`
| GIT_EMAIL | Git email of the account that will commit to the `gh-pages` branch. | `email@example.com`
| GIT_USERNAME | Username of the account that will commit to the `gh-pages` branch. | `username`

Others variables are set automatically by the `gitlab-runner`, see the documentation for [predefined variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html).<br>

### Itch.io

Deployment to Itch.io is done via [Butler](https://itch.io/docs/butler/).<br>
Secrets needed for a Itch.io deploy via GitLab CI:

|Variable|Description|Example|
|-|-|-|
| ITCHIO_USERNAME | Your username on Itch.io, as in your personal page will be at `https://<username>.itch.io` |`username`
| ITCHIO_GAME | the name of your game on Itchio, as in your game will be available at `https://<username>.itch.io/<game>`  |`game`
| BUTLER_API_KEY | An [Itch.io API key](https://itch.io/user/settings/api-keys) is necessary for Butler so that the CI can authenticate on Itch.io on your behalf. **Make that API key `Masked`(GitLab CI) to keep it secret** |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## Troubleshoot

#### Problems while exporting
- Check that the export names on `export_presets.cfg` match the ones used in your CI script.
- Check the paths used in your CI script, some commands may be running in the wrong place if you are keeping the project in a folder (like the `test-project` template) or not.

#### Authentication errors with Butler
- If using GitLab check that the 'protected' tag is disabled in the [CI/CD variables panel](https://github.com/aBARICHELLO/godot-ci#environment-configuration).
