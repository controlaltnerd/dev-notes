# CI/CD for Hugo Website

GitHub Action for checking out the latest version of a Hugo site, building and deploying it to the target server.

```
name: CI/CD Website

on:
  workflow_dispatch:
  # push:

jobs:
  build:
    name: Build and Deploy
    runs-on: ubuntu-latest
    steps:
      - name: 🛎️ Checkout
        uses: actions/checkout@v3

      - name: 🪄 Set Up Hugo
        env:
          HUGO_VERSION: extended_0.111.3
        run: |
          mkdir ~/hugo
          cd ~/hugo
          curl -L "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz" --output hugo.tar.gz
          tar -xvzf hugo.tar.gz
          sudo mv hugo /usr/local/bin

      - name: 🏗️ Build
        run: hugo --source public --minify

      - name: 🔑 Get SSH Key
        run: |
          install -m 600 -D /dev/null ~/.ssh/id_rsa
          echo "${{ secrets.PRIVATE_SSH_KEY }}" > ~/.ssh/id_rsa
          echo "${{ secrets.KNOWN_HOSTS }}" > ~/.ssh/known_hosts

      - name: 🚀 Deploy
        run: rsync --archive --delete --stats -e 'ssh -p 25354' 'SERVER/FOLDER/' ${{ secrets.REMOTE_DEST }}
```
