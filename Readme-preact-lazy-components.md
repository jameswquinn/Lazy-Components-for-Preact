To refactor `preact-lazy-components` as an external npm package, follow these steps to ensure it's properly set up for distribution:

### **1. Project Structure**

Organize your project with a clear structure:

```
preact-lazy-components/
├── src/
│   ├── components/
│   │   ├── LazyImage.js
│   │   ├── LazyIframe.js
│   │   ├── LazyBackground.js
│   ├── styles.css
│   ├── index.js
├── dist/
├── package.json
├── rollup.config.js
└── README.md
```

### **2. Component Code**

Here's the refactored code for each component to be included in the npm package:

#### **`src/components/LazyImage.js`**

```jsx
// src/components/LazyImage.js
import { useState, useEffect, useRef } from 'preact/hooks';
import '../styles.css'; // Import the CSS file

const LazyImage = ({ src, srcSet, sizes, alt, placeholder, width, height, className }) => {
  const [isLoaded, setIsLoaded] = useState(false);
  const [hasPlaceholderLoaded, setHasPlaceholderLoaded] = useState(false);
  const imgRef = useRef(null);

  useEffect(() => {
    const image = new Image();
    image.src = src;
    image.onload = () => setIsLoaded(true);

    return () => {
      image.onload = null;
    };
  }, [src]);

  useEffect(() => {
    const placeholderImage = new Image();
    placeholderImage.src = placeholder;
    placeholderImage.onload = () => setHasPlaceholderLoaded(true);

    return () => {
      placeholderImage.onload = null;
    };
  }, [placeholder]);

  return (
    <div className={`lazy-image ${className}`} style={{ width, height }}>
      <img
        src={src}
        srcSet={srcSet}
        sizes={sizes}
        alt={alt}
        className={`loaded ${isLoaded ? 'loaded' : ''}`}
        ref={imgRef}
      />
      <img
        src={placeholder}
        alt=""
        className={`placeholder ${hasPlaceholderLoaded ? 'hidden' : ''}`}
      />
    </div>
  );
};

export default LazyImage;
```

#### **`src/components/LazyIframe.js`**

```jsx
// src/components/LazyIframe.js
import { useState } from 'preact/hooks';
import '../styles.css'; // Import the CSS file

const LazyIframe = ({ src, width, height, aspectRatio, className }) => {
  const [isLoaded, setIsLoaded] = useState(false);

  return (
    <div className={`lazy-iframe ${className}`} style={{ width, height, position: 'relative' }}>
      {!isLoaded && <div className="iframe-placeholder" style={{ aspectRatio }} />}
      <iframe
        src={src}
        width="100%"
        height="100%"
        onLoad={() => setIsLoaded(true)}
        style={{ display: isLoaded ? 'block' : 'none' }}
        loading="lazy"
        frameBorder="0"
        allowFullScreen
      />
    </div>
  );
};

export default LazyIframe;
```

#### **`src/components/LazyBackground.js`**

```jsx
// src/components/LazyBackground.js
import { useState } from 'preact/hooks';
import '../styles.css'; // Import the CSS file

const LazyBackground = ({ image, placeholder, className, style, children }) => {
  const [isLoaded, setIsLoaded] = useState(false);

  return (
    <div
      className={`lazy-background ${className}`}
      style={{
        ...style,
        backgroundImage: isLoaded ? `url(${image})` : `url(${placeholder})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        filter: isLoaded ? 'none' : 'blur(10px)',
        transition: 'filter 0.3s ease-in-out',
      }}
    >
      {children}
      <img
        src={image}
        alt=""
        style={{ display: 'none' }}
        onLoad={() => setIsLoaded(true)}
      />
    </div>
  );
};

export default LazyBackground;
```

### **3. CSS File**

Ensure that the CSS file is properly set up for styling:

**`src/styles.css`**

```css
/* src/styles.css */
.lazy-image {
  position: relative;
  display: block;
  overflow: hidden;
}

.lazy-image img {
  display: block;
  width: 100%;
  height: auto;
  transition: opacity 0.3s ease-in-out;
}

.lazy-image .placeholder {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: blur(10px);
  transition: opacity 0.3s ease-in-out;
  z-index: 1;
}

.lazy-image img.loaded {
  opacity: 1;
}

.lazy-image .placeholder.hidden {
  opacity: 0;
}

.lazy-iframe {
  position: relative;
  display: block;
}

.iframe-placeholder {
  background: #ddd;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  filter: blur(10px);
}

.lazy-background {
  position: relative;
  display: block;
  background-size: cover;
  background-position: center;
}

.lazy-background img {
  display: none;
}
```

### **4. Rollup Configuration**

Set up Rollup for bundling the library:

**`rollup.config.js`**

```js
import babel from "@rollup/plugin-babel";
import postcss from "rollup-plugin-postcss";
import { terser } from "rollup-plugin-terser";

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'esm',
    sourcemap: true,
  },
  plugins: [
    postcss({
      extract: true,
      minimize: true,
      extensions: ['.css'],
    }),
    babel({
      babelHelpers: 'bundled',
      presets: ['@babel/preset-env'],
      plugins: [
        ['@babel/plugin-transform-react-jsx', { pragma: 'h', pragmaFrag: 'Fragment' }]
      ],
      extensions: ['.js'],
    }),
    terser(),
  ],
};
```

### **5. Package Configuration**

Ensure your package configuration is correct:

**`package.json`**

```json
{
  "name": "preact-lazy-components",
  "version": "1.0.0",
  "description": "A lightweight library for lazy loading images, iframes, and background images with blur-up effect in Preact.",
  "main": "dist/bundle.js",
  "module": "dist/bundle.js",
  "exports": {
    ".": "./dist/bundle.js"
  },
  "scripts": {
    "build": "rollup -c",
    "prepublishOnly": "npm run build"
  },
  "dependencies": {
    "preact": "^10.0.0"
  },
  "peerDependencies": {
    "preact": "^10.0.0"
  },
  "devDependencies": {
    "@babel/core": "^7.16.0",
    "@babel/plugin-transform-react-jsx": "^7.16.0",
    "@babel/preset-env": "^7.16.0",
    "rollup": "^2.60.0",
    "rollup-plugin-postcss": "^4.0.0",
    "rollup-plugin-terser": "^7.0.2",
    "@rollup/plugin-babel": "^5.3.0"
  },
  "keywords": [
    "preact",
    "lazy-load",
    "blur-up",
    "image",
    "iframe",
    "background-image"
  ],
  "author": "Your Name",
  "license": "MIT"
}
```

### **6. README.md**

Include comprehensive usage documentation:

**`README.md`**

```markdown
# preact-lazy-components

A lightweight library for lazy loading images, iframes, and background images with a blur-up effect in Preact.

## Installation

```bash
npm install preact-lazy-components
```

## Usage

### LazyImage

```jsx
import { LazyImage } from 'preact-lazy-components';

const App = () => (
  <div>
    <LazyImage
      src="path/to/high-res-image.jpg"
      srcSet="path/to/image-small.jpg 480w, path/to/image-medium.jpg 800w, path/to/image-large.jpg 1200w"
      sizes="(max-width: 600px) 100vw, 50vw"
      alt="Description"
      placeholder="path/to/placeholder.jpg"
      width="600"
      height="400"
      className="custom-image"
    />
  </div>
);
```

### LazyIframe

```jsx
import { LazyIframe } from 'preact-lazy-components';

const App = () => (
  <div>
    <LazyIframe
      src="path/to/iframe-content.html"
      width="600"
      height="400"
      aspectRatio="16:9"
      className="custom-iframe"
    />
  </div>
);
```

### LazyBackground

```jsx
import { LazyBackground } from 'preact-lazy-components';

const App = () => (
  <div>
   

 <LazyBackground
      image="path/to/background-image.jpg"
      placeholder="path/to/placeholder.jpg"
      className="custom-background"
      style={{ height: '400px' }}
    >
      <p>Content over the background</p>
    </LazyBackground>
  </div>
);
```

## License

MIT
```

### **Summary**

- **Library Features**: Implements lazy loading with blur-up effects for images, iframes, and background images.
- **Components**: `LazyImage`, `LazyIframe`, and `LazyBackground`.
- **CSS**: Styles are included for handling lazy loading and blur effects.
- **Bundling**: Uses Rollup for bundling with Babel for compatibility.
- **Documentation**: Comprehensive README with usage examples and installation instructions.

This setup ensures that your `preact-lazy-components` library is ready for distribution as an npm package, with hooks-based implementation, CSS styling, and proper configuration for bundling and publishing.
