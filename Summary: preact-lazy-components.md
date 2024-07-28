### Summary: `preact-lazy-components`

The `preact-lazy-components` library provides a suite of components and hooks designed for efficient lazy loading of images, iframes, and background images in Preact applications. Here’s a concise overview of the library’s features and setup:

#### **Features**

1. **Components:**
   - **`LazyImage`**: Lazy-loads images with a smooth blur-up effect. Supports `srcSet` and `sizes` for responsive images.
   - **`LazyIframe`**: Lazy-loads iframes with customizable aspect ratios and a placeholder while loading.
   - **`LazyBackground`**: Lazy-loads background images with support for placeholder images and custom styles.

2. **Hooks:**
   - **`useIntersectionObserver`**: Custom hook to manage intersection observer logic for detecting when an element is in view.
   - **`useWebPSupport`**: Custom hook to check if the browser supports WebP images.

3. **Blur-Up Effect:**
   - Includes a CSS-based blur-up effect for images, enhancing user experience by providing a smooth transition from a blurred placeholder to the final image.

4. **Responsive Image Handling:**
   - Supports `srcSet` and `sizes` for responsive images, allowing the library to handle different screen sizes and resolutions efficiently.

5. **Automatic Format Handling:**
   - Handles image formats by loading WebP if supported, otherwise falling back to JPEG, ensuring compatibility across browsers.

#### **Setup**

1. **Installation:**
   Install the library using npm:
   ```bash
   npm install preact-lazy-components
   ```

2. **Usage:**
   - **`LazyImage` Example:**
     ```jsx
     import { LazyImage } from 'preact-lazy-components';

     const App = () => (
       <div>
         <LazyImage
           src="path/to/actual-image.jpg"
           srcSet="path/to/image-small.jpg 480w, path/to/image-medium.jpg 800w, path/to/image-large.jpg 1200w"
           sizes="(max-width: 600px) 100vw, 50vw"
           alt="Description"
           width="600"
           height="400"
           placeholder="path/to/placeholder.jpg"
           className="custom-image"
         />
       </div>
     );
     ```

   - **`LazyIframe` Example:**
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

   - **`LazyBackground` Example:**
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

3. **Custom Hooks:**
   - **`useIntersectionObserver` Example:**
     ```jsx
     import useIntersectionObserver from 'preact-lazy-components/hooks/useIntersectionObserver';

     const App = () => {
       const ref = useRef(null);
       useIntersectionObserver(ref, () => {
         console.log('Element is visible');
       });

       return <div ref={ref}>Observe me!</div>;
     };
     ```

   - **`useWebPSupport` Example:**
     ```jsx
     import useWebPSupport from 'preact-lazy-components/hooks/useWebPSupport';

     const App = () => {
       const isWebPSupported = useWebPSupport();

       return (
         <div>
           {isWebPSupported ? 'WebP is supported!' : 'WebP is not supported.'}
         </div>
       );
     };
     ```

#### **Build Configuration**

- **Rollup Configuration:** Uses Rollup for bundling the library with support for CSS and JavaScript transpilation.
- **Package Configuration:** Defined in `package.json` with necessary dependencies and build scripts.

This library provides a streamlined and customizable approach to lazy loading, offering features comparable to `lazysizes` while being tailored for Preact applications.
