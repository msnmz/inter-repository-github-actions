# Inter-Repository Github Actions
This Repository is devoted to explain how can we trigger workflows between repositories
--
1) To trigger a workflow in another repository we need to make HTTP POST request to:
`https://api.github.com/repos/:owner/:repo/dispatches`
2) To make this post request we need to create a personal access token by following the [Github Guide](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)
3) Next, we need to set these credentials in a secret in each of the repositories (A & B). For each of the repositories, we’ll access the settings->secrets page and add a new secret named `ACCESS_TOKEN`. The value for the token will be our username a colon and the personal access token obtained in the previous step (username:token).
