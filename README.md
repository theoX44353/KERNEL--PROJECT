name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install direnv
        run: |
          curl -sfL https://direnv.net/install.sh | bash
          echo "$HOME/.local/bin" >> $GITHUB_PATH
      
      - name: Load environment variables
        run: |
          direnv allow .
          direnv export gha > "$GITHUB_ENV"
      
      - name: Run tests
        run: |
          # Your environment variables from .envrc are now available
          echo "DATABASE_URL=$DATABASE_URL"
          make test
