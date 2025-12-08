# Hugo Resume Website - Project Setup & Documentation

## Project Overview

This is a Hugo-based resume/portfolio website built with the Hugo Blox Resume template. The site is running locally and can be deployed to GitHub Pages.

**Live Development Server:** http://localhost:1313/

---

## Current Configuration

### Personal Information
- **Name:** Marco Siliezar 🇬🇹
- **Title:** Sr. Product Manager
- **Current Company:** Portico
- **Tagline:** Product Leader and Higher Ed Consultant
- **Profile Picture:** `/content/authors/admin/avatar.jpg`

### Social Links
- **GitHub:** https://github.com/meatloaf02
- **LinkedIn:** https://www.linkedin.com/in/marcosiliezar/
- **Instagram:** https://www.instagram.com/marco_sizzlr/
- **Note:** Email and phone number are NOT displayed on the public website

### Work Experience
1. **Portico** - Sr. Product Manager (August 2025 - October 2025)
2. **Workday** - Manager, Product Management (October 2021 - May 2025)
3. **Workday** - Senior Product Manager (April 2018 - October 2021)
4. **Workday** - Product Manager (April 2016 - April 2018)
5. **UC Berkeley** - Principal Institutional Research Analyst (June 2014 - April 2016)

### Education
1. **Northwestern University** - Advanced Graduate Certificate in AI (2025-2026)
2. **Northwestern University** - M.Sc Computer Science (2015)
3. **UC Santa Cruz** - B.A. Economics (2006)

### Skills
- **Technical:** Python, SQL, Statistical Modeling, CI/CD, JIRA
- **Hobbies:** Video Games, Functional Strength Training

### Languages
- English (100%)
- Spanish (100%)
- French (75%)

### Certifications
- ICAgile Certified Professional Agile Coach (March 2019)

### Site Theme
- **Theme Pack:** default (indigo/emerald)
- **Mode:** system (follows OS preference)
- **Font:** sans (Inter)
- **Font Size:** md (16px)
- **Border Radius:** md
- **Spacing:** comfortable
- **Avatar Size:** large (320px)
- **Avatar Shape:** rounded

---

## Installed Software

### Required Tools
- **Hugo Extended:** v0.152.2
- **Node.js:** v25.2.1
- **pnpm:** v10.14.0
- **Go:** v1.25.5

All installed via Homebrew on macOS.

---

## Important Commands

### Development
```bash
# Start development server (auto-reloads on changes)
pnpm dev

# Build for production
pnpm build

# Stop the server
# Press Ctrl+C in the terminal
```

### Installation (if starting fresh)
```bash
# Install dependencies
pnpm install

# Install required tools (if not already installed)
brew install hugo
brew install node
brew install pnpm
brew install go
```

---

## Key Files & Directories

### Content Files
- **Main Profile:** `/content/authors/admin/_index.md`
  - Contains all personal info, work history, education, skills, languages, awards

- **Homepage Layout:** `/content/_index.md`
  - Controls which sections appear on the homepage
  - Avatar size and shape settings

- **Profile Picture:** `/content/authors/admin/avatar.jpg`
  - Replace this file to update your photo

### Configuration Files
- **Site Config:** `/config/_default/params.yaml`
  - Site name, tagline, description
  - Theme settings (pack, colors, mode)
  - Typography and layout settings
  - SEO, analytics, and other site-wide settings

- **Navigation Menu:** `/config/_default/menus.yaml`
- **Languages:** `/config/_default/languages.yaml`
- **Hugo Modules:** `/config/_default/module.yaml`
- **Hugo Settings:** `/config/_default/hugo.yaml`

### Other Important Files
- **Package Config:** `/package.json` - npm scripts and dependencies
- **Go Modules:** `/go.mod` - Hugo module dependencies
- **Resume PDF:** `/static/uploads/resume.pdf` (if you want to add download)

---

## How to Make Common Changes

### Update Personal Information
Edit `/content/authors/admin/_index.md`:
- Change `title` for your display name
- Change `role` for your current position
- Change `organizations` for your current company
- Update `profiles` array for social links
- Edit the bio text at the bottom of the file

### Update Work Experience
Edit the `work` section in `/content/authors/admin/_index.md`:
```yaml
work:
  - position: Job Title
    company_name: Company Name
    date_start: YYYY-MM-DD
    date_end: YYYY-MM-DD  # or '' for current
    summary: |
      Description and bullet points
      - Achievement 1
      - Achievement 2
```

### Update Education
Edit the `education` section in `/content/authors/admin/_index.md`:
```yaml
education:
  - area: Degree Name
    institution: University Name
    date_start: YYYY-MM-DD
    date_end: YYYY-MM-DD
    summary: |
      Optional details about coursework, GPA, etc.
```

### Change Theme
Edit `/config/_default/params.yaml` and change the `pack` value:
```yaml
theme:
  pack: "default"  # or "minimal", "coffee", "retro", "marine", etc.
```

**Available Themes:**
- **Professional:** minimal, default, solar, contrast
- **Creative:** coffee, matcha, marine, retro
- **Bold:** dracula, synthwave, cupcake

### Change Colors
Edit `/config/_default/params.yaml`:
```yaml
theme:
  colors:
    primary: "indigo"     # or hex like "#6366f1"
    secondary: "emerald"
    neutral: "slate"
```

### Change Avatar Size or Shape
Edit `/content/_index.md` under the biography section:
```yaml
avatar:
  size: large  # small (150px), medium (200px), large (320px), xl (400px), xxl (500px)
  shape: rounded  # circle, square, rounded
```

### Update Profile Picture
Replace the file at `/content/authors/admin/avatar.jpg` with your new photo.
The site auto-rebuilds when files change.

### Add/Remove Sections
Edit `/content/_index.md` and modify the `sections` array.
Available blocks: biography, experience, skills, awards, languages

---

## Project Structure

```
meatloaf02.github.io/
├── assets/              # Static assets (CSS, images, etc.)
├── config/
│   └── _default/        # Configuration files
├── content/
│   ├── authors/
│   │   └── admin/       # Your profile content
│   │       ├── _index.md    # Main profile data
│   │       └── avatar.jpg   # Profile picture
│   └── _index.md        # Homepage layout
├── static/              # Static files (served as-is)
├── hugo-blox/           # Hugo Blox theme files
├── package.json         # Node dependencies
├── pnpm-lock.yaml       # Lock file
├── go.mod              # Go module dependencies
└── PROJECT_SETUP.md    # This file!
```

---

## Deployment

### GitHub Pages
This repository is set up as `meatloaf02.github.io` which is a GitHub Pages user site.

**To deploy:**
1. Build the site: `pnpm build`
2. The output will be in the `public/` directory
3. Commit and push changes to GitHub
4. Configure GitHub Pages to serve from the appropriate branch

### Alternative: Netlify
The project includes `/netlify.toml` for Netlify deployment.
Simply connect your GitHub repository to Netlify for automatic deployments.

---

## Troubleshooting

### Server won't start
- Check that all dependencies are installed: `pnpm install`
- Verify Hugo is installed: `hugo version`
- Verify Go is installed: `go version`

### Changes not appearing
- The dev server auto-reloads, but try hard-refreshing your browser (Cmd+Shift+R)
- Check the terminal for build errors

### Profile picture not updating
- Make sure the file is named `avatar.jpg` (or update the reference)
- Check that it's in `/content/authors/admin/`
- The server should auto-rebuild when you replace the file

---

## Reference URLs

- **Hugo Blox Documentation:** https://docs.hugoblox.com/
- **Hugo Documentation:** https://gohugo.io/documentation/
- **Theme Customization Guide:** https://docs.hugoblox.com/guides/customize/

---

## Notes

- The site uses Tailwind CSS v4 for styling
- Hugo modules are downloaded to `~/Library/Caches/hugo_cache/modules/`
- Background tasks are currently running (use `Ctrl+C` to stop the dev server)
- Changes to content files auto-rebuild the site in ~100-500ms

---

**Last Updated:** December 3, 2024
**Hugo Version:** v0.152.2+extended
**Repository:** https://github.com/meatloaf02/meatloaf02.github.io
