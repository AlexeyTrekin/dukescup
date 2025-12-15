# Dukes Cup

The Duke's Cup website built with Hugo static site generator.

## 🚀 Live Site

The website is automatically deployed to GitHub Pages: https://alexeytrekin.github.io/dukescup/

## 🛠️ Development

### Prerequisites
- [Hugo Extended](https://gohugo.io/installation/) (v0.128.0 or later)
- [AWS CLI](https://aws.amazon.com/cli/) (for media sync)

### Local Development
1. Clone this repository
2. Navigate to the landing directory: `cd landing`
3. Sync media from S3 (first time only, or when media updates):
   ```bash
   aws s3 sync s3://dukes-cup-media ./media
   ```
4. Start the Hugo development server: `hugo server -D`
5. Open your browser to `http://localhost:1313`

### Media Management

Media files are stored in S3 bucket `dukes-cup-media` and mounted locally via Hugo module mounts. The `./media` folder is gitignored.

**Download media from S3:**
```bash
aws s3 sync s3://dukes-cup-media ./media
```

**Upload new media to S3:**
```bash
aws s3 sync ./media s3://dukes-cup-media
```

**Upload with public read access:**
```bash
aws s3 sync ./media s3://dukes-cup-media --acl public-read
```

### Building for Production
```bash
cd landing
hugo --gc --minify
```

## 📁 Project Structure
- `landing/` - Hugo site source files
- `media/` - Local media files (gitignored, synced from S3)
- `.github/workflows/` - GitHub Actions for automatic deployment
- `landing/themes/PaperMod/` - Hugo theme (PaperMod)

## 🔄 Deployment

The site is automatically deployed to GitHub Pages using GitHub Actions whenever you push to the `main` branch. The workflow:

1. Checks out the code
2. Sets up Hugo
3. Builds the site
4. Deploys to GitHub Pages
