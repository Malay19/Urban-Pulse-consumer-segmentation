# 🚀 Deploy to Netlify - Step by Step Guide

## Quick Deploy (Recommended)

### Option 1: One-Click Deploy
Click this button to deploy directly to Netlify:

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/your-org/consumer-segmentation)

### Option 2: Manual Deployment

## Prerequisites

1. **GitHub Account** - Your code needs to be in a GitHub repository
2. **Netlify Account** - Sign up at [netlify.com](https://netlify.com) (free)

## Step 1: Prepare Your Repository

First, make sure your code is in a GitHub repository:

```bash
# If you haven't already, initialize git and push to GitHub
git init
git add .
git commit -m "Initial commit - Consumer Segmentation Analytics"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/consumer-segmentation.git
git push -u origin main
```

## Step 2: Deploy via Netlify Dashboard

1. **Login to Netlify**
   - Go to [app.netlify.com](https://app.netlify.com)
   - Sign in with GitHub

2. **Create New Site**
   - Click "New site from Git"
   - Choose "GitHub"
   - Select your `consumer-segmentation` repository

3. **Configure Build Settings**
   ```
   Build command: python build_static.py
   Publish directory: dist
   ```

4. **Environment Variables** (Optional)
   - Go to Site settings → Environment variables
   - Add these variables:
     ```
     ENVIRONMENT=production
     CACHE_ENABLED=true
     LOG_LEVEL=INFO
     ```

5. **Deploy**
   - Click "Deploy site"
   - Wait for build to complete (2-3 minutes)

## Step 3: Verify Deployment

Once deployed, you'll get a URL like: `https://amazing-name-123456.netlify.app`

Your site should show:
- ✅ Consumer Segmentation Analytics homepage
- ✅ Interactive features and visualizations
- ✅ Sample analysis results
- ✅ Professional design and layout

## Step 4: Custom Domain (Optional)

1. **Add Custom Domain**
   - Go to Site settings → Domain management
   - Click "Add custom domain"
   - Enter your domain (e.g., `analytics.yourdomain.com`)

2. **Configure DNS**
   - Add CNAME record pointing to your Netlify URL
   - Or use Netlify DNS for automatic configuration

3. **Enable HTTPS**
   - Netlify automatically provides SSL certificates
   - Force HTTPS redirect in Site settings

## Troubleshooting

### Build Fails

If the build fails, check the deploy log for errors:

1. **Python Dependencies**
   ```bash
   # Make sure requirements.txt is complete
   pip freeze > requirements.txt
   ```

2. **Build Script Issues**
   ```bash
   # Test build locally
   python build_static.py
   ```

3. **Memory Issues**
   - Reduce sample size in build_static.py
   - Use demo data instead of full analysis

### Site Not Loading

1. **Check Build Output**
   - Verify `dist` folder was created
   - Check for `index.html` in dist folder

2. **Check Console Errors**
   - Open browser developer tools
   - Look for JavaScript errors

### Performance Issues

1. **Optimize Build**
   - Reduce sample data size
   - Enable caching
   - Compress assets

## Advanced Configuration

### Custom Build Command

If you need a custom build process, update `netlify.toml`:

```toml
[build]
  command = "python build_static.py && npm run build"
  publish = "dist"

[build.environment]
  PYTHON_VERSION = "3.9"
  NODE_VERSION = "18"
```

### Redirects and Headers

The `netlify.toml` file already includes:
- SPA redirects for single-page application
- Security headers
- Cache optimization
- Environment-specific settings

### Form Handling

To add contact forms or feedback:

```html
<form name="contact" method="POST" data-netlify="true">
  <input type="text" name="name" placeholder="Name" required>
  <input type="email" name="email" placeholder="Email" required>
  <textarea name="message" placeholder="Message" required></textarea>
  <button type="submit">Send</button>
</form>
```

## Monitoring and Analytics

### Netlify Analytics

1. **Enable Analytics**
   - Go to Site overview → Analytics
   - Enable Netlify Analytics (paid feature)

2. **Custom Analytics**
   - Add Google Analytics
   - Use Plausible or other privacy-focused analytics

### Performance Monitoring

1. **Lighthouse Scores**
   - Netlify automatically runs Lighthouse
   - Check performance scores in deploy details

2. **Core Web Vitals**
   - Monitor loading performance
   - Optimize based on metrics

## Continuous Deployment

### Automatic Deploys

Every push to your main branch will automatically:
1. Run tests (if configured)
2. Build the static site
3. Deploy to Netlify
4. Update your live site

### Deploy Previews

Pull requests automatically get preview deployments:
- Test changes before merging
- Share previews with stakeholders
- Automatic cleanup after merge

## Cost Optimization

### Free Tier Limits

Netlify free tier includes:
- 100GB bandwidth/month
- 300 build minutes/month
- 1 concurrent build

### Optimization Tips

1. **Reduce Build Time**
   - Cache dependencies
   - Optimize build script
   - Use incremental builds

2. **Reduce Bandwidth**
   - Compress images
   - Enable gzip compression
   - Use CDN for assets

## Security Best Practices

### Environment Variables

Never commit sensitive data:
```bash
# Use environment variables for API keys
CENSUS_API_KEY=your_key_here
MAPBOX_TOKEN=your_token_here
```

### Content Security Policy

Add CSP headers in `netlify.toml`:
```toml
[[headers]]
  for = "/*"
  [headers.values]
    Content-Security-Policy = "default-src 'self'; script-src 'self' 'unsafe-inline'"
```

## Support and Resources

- **Netlify Docs**: [docs.netlify.com](https://docs.netlify.com)
- **Community Forum**: [community.netlify.com](https://community.netlify.com)
- **Status Page**: [netlifystatus.com](https://netlifystatus.com)

## Success! 🎉

Your Consumer Segmentation Analytics platform is now live on Netlify!

**Next Steps:**
1. Share your live URL with stakeholders
2. Set up custom domain if needed
3. Monitor performance and usage
4. Iterate based on user feedback

**Your deployment includes:**
- ✅ Professional analytics dashboard
- ✅ Interactive visualizations
- ✅ Consumer persona insights
- ✅ Business opportunity analysis
- ✅ Mobile-responsive design
- ✅ Fast global CDN delivery
- ✅ Automatic HTTPS
- ✅ Continuous deployment

Congratulations on deploying your advanced analytics platform! 🚀