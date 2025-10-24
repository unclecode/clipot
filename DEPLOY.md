# Deploy Touch Up to GitHub Pages

## Quick Deploy (5 minutes)

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Name: `touch-up` (or any name you like)
3. Description: "Polish your thoughts with AI"
4. Keep it **Public** (required for free GitHub Pages)
5. Click "Create repository"

### Step 2: Push Code

```bash
# If you haven't committed yet
git add .
git commit -m "Initial commit - Touch Up PWA"

# Add your GitHub repo (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/touch-up.git
git branch -M main
git push -u origin main
```

### Step 3: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click "Settings" tab
3. Click "Pages" in the left sidebar
4. Under "Source", select "main" branch
5. Click "Save"
6. Wait 1-2 minutes for deployment

### Step 4: Access Your App

Your app will be live at:
```
https://YOUR_USERNAME.github.io/touch-up/
```

### Step 5: Install on iOS

1. Open the URL above in Safari on your iPhone
2. Tap Share button (□↑)
3. Tap "Add to Home Screen"
4. Tap "Add"
5. Done! App is now on your home screen

## Alternative: Deploy to Other Services

### Netlify (Also Free)

1. Go to https://app.netlify.com/drop
2. Drag the entire folder into the drop zone
3. Get instant URL
4. Works exactly like GitHub Pages

### Vercel (Also Free)

```bash
npm install -g vercel
vercel
# Follow prompts
```

### Cloudflare Pages (Also Free)

1. Go to https://pages.cloudflare.com/
2. Connect GitHub repo
3. Deploy automatically

## Update Your App

After making changes:

```bash
git add .
git commit -m "Update app"
git push
```

GitHub Pages updates automatically in 1-2 minutes.

## Custom Domain (Optional)

If you own a domain:

1. In GitHub repo settings → Pages
2. Enter your custom domain
3. Add CNAME record in your DNS settings

Example: `touchup.yourdomain.com`

## Troubleshooting

**App not updating?**
- Clear browser cache
- Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
- Wait 2-3 minutes for GitHub Pages to rebuild

**PWA not installing?**
- Make sure you're using HTTPS (GitHub Pages uses HTTPS)
- Try in Safari (iOS) or Chrome (Android)
- Check that manifest.json is accessible

**Icons not showing?**
- They should appear after installation
- May take a few seconds to load

## No Server Needed!

Once deployed to GitHub Pages:
- ✅ Works from anywhere
- ✅ No server to maintain
- ✅ Free forever
- ✅ Automatic HTTPS
- ✅ Fast global CDN
- ✅ Just update via git push
