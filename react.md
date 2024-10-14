``` typescript
import React from 'react';

type ImageSource = {
  [breakpoint: string]: number; // Object where keys are breakpoints (e.g., '300', '768', '1280') and values are image widths in pixels
};

type ImageSources = {
  webp: ImageSource;
  jpeg: ImageSource;
};

type ResponsiveImageProps = {
  sources: ImageSources;
};

// Helper function to generate srcSet string
const getSrcSet = (source: ImageSource) => {
  return Object.entries(source)
    .map(([breakpoint, width]) => `${width}w`) // add "w" to the width value
    .join(', ');
};

// Helper function to generate sizes string
const getSizes = (source: ImageSource) => {
  const breakpoints = Object.keys(source).map((breakpoint) => parseInt(breakpoint, 10));

  return breakpoints
    .map((bp) => `(max-width: ${bp}px) ${bp}px`)
    .join(', ') + `, ${Math.max(...breakpoints)}px`; // For the largest size
};

const ResponsiveImage: React.FC<ResponsiveImageProps> = ({ sources }) => {
  return (
    <picture>
      <source
        type="image/webp"
        srcSet={getSrcSet(sources.webp)}
        sizes={getSizes(sources.webp)}
      />
      <source
        type="image/jpeg"
        srcSet={getSrcSet(sources.jpeg)}
        sizes={getSizes(sources.jpeg)}
      />
      <img src={sources.jpeg[1280]} alt="Responsive" />
    </picture>
  );
};

export default ResponsiveImage;
```