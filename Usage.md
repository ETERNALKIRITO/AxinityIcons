# How to Use Axinity Icons

This repository serves as a centralized, CDN-backed icon database for all Axinity projects. Instead of hardcoding image links into individual projects, your websites can dynamically fetch `icons.json` to load the exact icon, size, or variation they need.

---

## 1. The CDN Base Links

* **The Configuration JSON:**
  `https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json`
  
* **The CDN Image Base Path:**
  `https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/`

---

## 2. Basic JavaScript Fetch Template

To load any icon, fetch the `icons.json` file, target the project key and the size key, and combine it with the jsDelivr base URL:

```javascript
fetch('https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json')
  .then(response => response.json())
  .then(data => {
    // Access syntax: data["PROJECT_KEY"]["VARIATION_OR_SIZE"]
    const relativePath = data["PROJECT_KEY"]["VARIATION_OR_SIZE"];
    
    // Combine to get the complete working URL
    const fullUrl = 'https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/' + relativePath;

    console.log(fullUrl);
  });
```

---

## 3. Practical Examples

### Example A: Setting a Dynamic Favicon (Page Tab Icon)
Paste this script into the `<head>` of your website to dynamically pull the `32px` developer icon:

```html
<script>
  fetch('https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json')
    .then(response => response.json())
    .then(data => {
      const relativePath = data["axin-dev"]["32"];
      const fullUrl = 'https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/' + relativePath;

      let link = document.querySelector("link[rel*='icon']") || document.createElement('link');
      link.rel = 'icon';
      link.href = fullUrl;
      document.head.appendChild(link);
    })
    .catch(err => console.error('Favicon fetch failed:', err));
</script>
```

### Example B: Injecting a Specific Logo Size into an HTML Image Element
If you have an `<img>` tag in your HTML:

```html
<img id="project-logo" src="placeholder.png" alt="Axinity Logo" />
```

You can populate it with the `512px` developer logo using this script:

```html
<script>
  fetch('https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json')
    .then(response => response.json())
    .then(data => {
      const relativePath = data["axin-dev"]["512"];
      const fullUrl = 'https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/' + relativePath;

      // Update the HTML image source
      document.getElementById('project-logo').src = fullUrl;
    });
</script>
```

### Example C: Fetching Alternative Styles (e.g., Skettle OS Rounded 2)
If you need to fetch a specific style variation like the second rounded style of the Skettle OS logo:

```html
<script>
  fetch('https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json')
    .then(response => response.json())
    .then(data => {
      const relativePath = data["skettle-os"]["rounded2"];
      const fullUrl = 'https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/' + relativePath;

      // Update your HTML or JS image element
      document.getElementById('skettle-logo').src = fullUrl;
    });
</script>
```

---

## 4. Cache-Busting (Updating Immediately)

Browsers and CDNs cache files for speed. If you change a link inside `icons.json` and want your websites to fetch the changes immediately without using cached data, append a time-based query parameter to your fetch request:

```javascript
// Generates a value that updates once every hour
const hourlyBuster = Math.floor(Date.now() / 3600000);

fetch(`https://cdn.jsdelivr.net/gh/ETERNALKIRITO/AxinityIcons@main/icons.json?v=${hourlyBuster}`)
  .then(response => response.json())
  .then(data => {
    // ... use data
  });
```
