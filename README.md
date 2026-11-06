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

**Generate thumbnails** 

```bash
for img in *.jpg; do
  [ -f "$img" ] && magick "$img" -resize 200x200^ -gravity center -extent 200x200 -quality 85 "thumbnails/$img"
done
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

### Print-Ready Rules Documents

Generate DOCX files for printing the tournament rules. Requires [Pandoc](https://pandoc.org/installing.html) and `rsvg-convert` (`brew install librsvg`) for the SVG target-zone images.

```bash
# Create output directory
mkdir -p print-ready

# Generate DOCX files (current rules live in rules/, previous years in rules/<year>/)
RULES=landing/content/rules
OUT=print-ready
# strip the +++ front matter
strip_fm() { awk 'n>=2; /^\+\+\+$/{n++}' "$1"; }

# Competition rules (main page)
strip_fm $RULES/_index.md | pandoc -f markdown -o $OUT/competition-rules.docx --metadata title="Competition Rules"

# Gear requirements
strip_fm $RULES/gear-requirements.md | pandoc -f markdown -o $OUT/gear-requirements.docx --metadata title="Gear Requirements"

# Tournament rules (with images)
strip_fm $RULES/tournament-rules.md \
  | sed 's|\.\./\.\./images/rules/|landing/static/images/rules/|g' \
  | sed -E 's|<img src="([^"]+)" alt="([^"]*)"[^>]*/>|![\2](\1)|g' \
  | sed '/<div style/d' \
  | pandoc -f markdown -o $OUT/tournament-rules.docx --metadata title="Tournament Rules"

# Judges' guidelines
strip_fm $RULES/judges-guidelines.md | pandoc -f markdown -o $OUT/judges-guidelines.docx --metadata title="Judges' Guidelines"

```

The `print-ready/` folder is gitignored.

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
