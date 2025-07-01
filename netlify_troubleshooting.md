# Netlify Deployment Troubleshooting Guide

## Common Issues and Solutions

### 1. Build Failures

#### Python Version Issues
```bash
# Error: Python version not supported
# Solution: Update runtime.txt
echo "python-3.9.18" > runtime.txt
```

#### Missing Dependencies
```bash
# Error: ModuleNotFoundError
# Solution: Update requirements.txt
pip freeze > requirements.txt
git add requirements.txt
git commit -m "Update dependencies"
git push
```

#### Memory Limit Exceeded
```python
# Error: Build killed due to memory limit
# Solution: Reduce sample size in build_static.py

# In build_static.py, line ~50:
trips_df = loader.download_divvy_data(2023, 6)
# Change to:
trips_df = loader.download_divvy_data(2023, 6).head(500)  # Smaller sample
```

### 2. Site Not Loading

#### Missing index.html
```bash
# Check if dist/index.html exists after build
ls -la dist/
# Should show index.html and other files
```

#### Incorrect Publish Directory
```toml
# In netlify.toml, ensure:
[build]
  publish = "dist"  # Not "build" or other folder
```

### 3. Performance Issues

#### Slow Loading
```python
# Optimize build_static.py for faster generation
def generate_sample_analysis():
    # Use smaller datasets
    trips_df = loader.download_divvy_data(2023, 6).head(1000)
    spending_df = loader.download_spending_data(['17031'])  # Single county
```

#### Large Bundle Size
```bash
# Check dist folder size
du -sh dist/
# Should be under 100MB for free tier
```

### 4. Environment Variables

#### Missing API Keys
```bash
# In Netlify dashboard:
# Site settings → Environment variables → Add variable
CENSUS_API_KEY=your_key_here
```

#### Environment Not Set
```bash
# Ensure ENVIRONMENT is set to production
ENVIRONMENT=production
```

### 5. Custom Domain Issues

#### DNS Configuration
```bash
# For subdomain (recommended):
# CNAME: analytics.yourdomain.com → your-site.netlify.app

# For apex domain:
# A record: yourdomain.com → 75.2.60.5
# AAAA record: yourdomain.com → 2600:1f14:e22:d::2
```

#### SSL Certificate Issues
```bash
# Wait 24-48 hours for DNS propagation
# Force SSL renewal in Netlify dashboard if needed
```

### 6. Build Time Optimization

#### Cache Dependencies
```toml
# In netlify.toml
[build]
  command = "pip install --cache-dir .pip-cache -r requirements.txt && python build_static.py"
```

#### Parallel Processing
```python
# In build_static.py, disable parallel processing for build
config = PipelineConfig(
    enable_parallel_processing=False,  # Reduce memory usage
    max_workers=1
)
```

### 7. Debugging Steps

#### Check Build Logs
1. Go to Netlify dashboard
2. Click on failed deploy
3. View build log for specific errors

#### Test Locally
```bash
# Test build script locally
python build_static.py
ls -la dist/
# Should create dist folder with index.html
```

#### Validate HTML
```bash
# Check if generated HTML is valid
python -c "
import os
if os.path.exists('dist/index.html'):
    with open('dist/index.html', 'r') as f:
        content = f.read()
        if '<html' in content and '</html>' in content:
            print('✅ HTML file is valid')
        else:
            print('❌ HTML file is invalid')
else:
    print('❌ index.html not found')
"
```

### 8. Emergency Fixes

#### Quick Demo Mode
```python
# In build_static.py, force demo mode
def generate_sample_analysis():
    print("Using demo data for quick deployment...")
    return generate_demo_data()  # Skip complex analysis
```

#### Minimal Build
```python
# Create minimal index.html if build fails
def create_minimal_site():
    html = """
    <!DOCTYPE html>
    <html>
    <head><title>Consumer Segmentation Analytics</title></head>
    <body>
        <h1>🎯 Consumer Segmentation Analytics</h1>
        <p>Advanced analytics platform coming soon...</p>
        <p>Currently optimizing for deployment.</p>
    </body>
    </html>
    """
    os.makedirs('dist', exist_ok=True)
    with open('dist/index.html', 'w') as f:
        f.write(html)
```

### 9. Contact Support

If issues persist:

1. **Netlify Support**
   - Free tier: Community forum
   - Paid tier: Direct support

2. **GitHub Issues**
   - Create issue in your repository
   - Include build logs and error messages

3. **Community Help**
   - Stack Overflow with `netlify` tag
   - Netlify community forum

### 10. Success Checklist

Before going live, verify:

- [ ] Build completes successfully
- [ ] Site loads without errors
- [ ] All visualizations work
- [ ] Mobile responsive design
- [ ] Fast loading times (<3 seconds)
- [ ] HTTPS enabled
- [ ] Custom domain configured (if applicable)
- [ ] Analytics tracking setup
- [ ] Error monitoring enabled

## Quick Fix Commands

```bash
# Reset and redeploy
git add .
git commit -m "Fix deployment issues"
git push origin main

# Force new deploy in Netlify
# Go to Deploys → Trigger deploy → Deploy site
```

Your Consumer Segmentation Analytics platform should now be successfully deployed on Netlify! 🎉