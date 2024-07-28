### Summary

The final refactored code provides a comprehensive solution for lazy loading images, iframes, and background images in Preact applications. Here's an overview of the key features and functionalities:

#### **1. Key Features:**

1. **Automatic WebP Detection:**
   - Uses a custom hook to check if WebP format is supported by the browser and dynamically selects the appropriate image format (WebP or JPEG).

2. **Responsive Image Handling:**
   - Supports `srcSet` and `sizes` attributes to handle responsive images efficiently, providing multiple image resolutions for different screen sizes.

3. **Error Handling and Customization:**
   - Includes mechanisms for custom loading placeholders and error states to improve user experience. Components can display custom loading messages and error notifications.

4. **Lazy Loading for Images, Iframes, and Backgrounds:**
   - Provides components for lazy loading images (`LazyImage`), iframes (`LazyIframe`), and background images (`LazyBackground`). These components load their content only when they come into view.

5. **Intersection Observer Hook:**
   - Implements a hook (`useIntersectionObserver`) to detect when an element enters the viewport, triggering the lazy loading process.

6. **Efficient Build Configuration:**
   - Uses Rollup for bundling, ensuring optimized performance with minimized code size.

#### **2. Components:**

- **`LazyImage`:**
  - Lazy loads images with support for multiple formats (WebP and JPEG) and responsive design. It handles image loading and errors with customizable placeholders.

- **`LazyIframe`:**
  - Lazy loads iframes and handles loading state. Adjusts aspect ratio and size to ensure responsive behavior.

- **`LazyBackground`:**
  - Lazy loads background images with support for placeholders and customizable styles. Useful for large background images and content sections.

#### **3. Hooks:**

- **`useIntersectionObserver`:**
  - A custom hook to observe when an element enters the viewport, triggering lazy loading behavior.

- **`useWebPSupport`:**
  - A custom hook to detect WebP format support and select the appropriate image format.

#### **4. Installation and Usage:**

- **Installation:**
  ```bash
  npm install preact-lazy-components
  ```

- **Basic Usage:**
  ```jsx
  import { LazyImage, LazyIframe, LazyBackground } from 'preact-lazy-components';

  const App = () => (
    <div>
      <LazyImage
        src="path/to/image"
        srcSet="path/to/image-small.jpg 480w, path/to/image-medium.jpg 800w, path/to/image-large.jpg 1200w"
        sizes="(max-width: 600px) 100vw, 50vw"
        alt="Description"
        width="600"
        height="400"
        placeholder="path/to/placeholder.jpg"
        className="custom-image"
      />
      <LazyIframe
        src="https://www.example.com"
        width="800"
        height="450"
        aspectRatio="16:9"
        className="custom-iframe"
      />
      <LazyBackground
        image="path/to/background-image.jpg"
        placeholder="path/to/placeholder.jpg"
        className="custom-background"
      >
        <div className="content">
          <h2>Content inside background</h2>
        </div>
      </LazyBackground>
    </div>
  );
  ```

#### **5. Documentation:**

- **README.md:**
  - Comprehensive documentation is provided, including installation instructions, component usage examples, advanced features, and contributing guidelines.

This package is designed to be efficient and versatile, catering to modern web standards and providing a user-friendly experience for handling lazy loading in Preact applications.
