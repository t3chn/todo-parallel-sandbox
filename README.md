# Todo App

A minimal single-user todo list app built with vanilla JavaScript.

## Quickstart

Run the app locally with Python's built-in web server:

```bash
python3 -m http.server 8000
```

Open http://localhost:8000 in your browser.

## Features

- Add new todos
- Mark todos as complete/incomplete
- Delete todos
- Automatic persistence via localStorage (todos survive page reloads)
- No dependencies, no build step

## Development

### Workflow

1. Create a feature branch:
   ```bash
   git checkout -b your-branch-name
   ```

2. Make your changes and commit:
   ```bash
   git add .
   git commit -m "description of changes"
   ```

3. Push and open a PR:
   ```bash
   git push -u origin your-branch-name
   gh pr create
   ```

### Project Structure

- Pure HTML/CSS/JavaScript (no frameworks or bundlers)
- Static files served directly
- Client-side storage only (no backend or database)
