# Inter-Repository Github Actions
This Repository is devoted to explain how can we trigger workflows between repositories
--
1) To trigger a workflow in another repository we need to make HTTP POST request to:
`https://api.github.com/repos/:owner/:repo/dispatches`
2) To make this post request we need to create a personal access token by following the [Github Guide](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)
3) Next, we need to set these credentials in a secret in each of the repositories (A & B). For each of the repositories, we’ll access the settings->secrets page and add a new secret named `ACCESS_TOKEN`. The value for the token will be our username a colon and the personal access token obtained in the previous step (username:token).
4) Then we need to set the jobs for the repositories to interact each other.
```yml
jobs:
  ping-pong:
    runs-on: ubuntu-latest
    steps:
      - name: PING - Dispatch initiating repository event
        if: github.event.action != 'pong'
        run: |
          curl -X POST https://api.github.com/repos/:owner/:repo/dispatches \
          -H 'Accept: application/vnd.github.everest-preview+json' \
          -u ${{ secrets.ACCESS_TOKEN }} \
          --data '{"event_type": "ping", "client_payload": { "repository": "'"$GITHUB_REPOSITORY"'" }}'
      - name: ACK - Acknowledge pong from remote repository
        if: github.event.action == 'pong'
        run: |
          echo "PONG received from '${{ github.event.client_payload.repository }}'" && \
          curl -X POST https://api.github.com/repos/:owner/:repo/dispatches \
          -H 'Accept: application/vnd.github.everest-preview+json' \
          -u ${{ secrets.ACCESS_TOKEN }} \
          --data '{"event_type": "ack", "client_payload": { "repository": "'"$GITHUB_REPOSITORY"'" }}'

```
