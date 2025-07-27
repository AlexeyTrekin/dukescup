# Dukes Cup

The Duke's Cup website built with Hugo static site generator.

## 🚀 Live Site

The website is automatically deployed to GitHub Pages: https://alexeytrekin.github.io/dukescup/

## 🛠️ Development

### Prerequisites
- [Hugo Extended](https://gohugo.io/installation/) (v0.128.0 or later)

### Local Development
1. Clone this repository
2. Navigate to the landing directory: `cd landing`
3. Start the Hugo development server: `hugo server -D`
4. Open your browser to `http://localhost:1313`

### Building for Production
```bash
cd landing
hugo --gc --minify
```

## 📁 Project Structure
- `landing/` - Hugo site source files
- `.github/workflows/` - GitHub Actions for automatic deployment
- `landing/themes/ananke/` - Hugo theme (Ananke)

## 🔄 Deployment

The site is automatically deployed to GitHub Pages using GitHub Actions whenever you push to the `main` branch. The workflow:

1. Checks out the code
2. Sets up Hugo
3. Builds the site
4. Deploys to GitHub Pages
