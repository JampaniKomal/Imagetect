# Imagetect

## Overview

Imagetect is a versatile, client-side web application designed to provide a suite of powerful image manipulation tools. Built with simplicity and efficiency in mind, it allows users to perform various image processing tasks directly in their browser, ensuring privacy and speed as no files are uploaded to a server.

## Current Features

*   **Image Compressor:** Upload an image and specify a target size (in KB or MB). Imagetect uses a sophisticated algorithm to compress the image to meet your exact requirements, providing a preview of both the original and compressed versions.

## How to Use the Compressor

1.  **Upload Image:** Click the "Upload Image" button and select the image file you want to compress.
2.  **Set Target Size:** Enter your desired file size in the "Target Size" input field and select the appropriate unit (KB or MB).
3.  **Compress:** Click the "Compress" button to start the process.
4.  **Preview & Download:** Once complete, you will see a preview of the compressed image along with its new size. You can then click the "Download Image" button to save it.
5.  **(Optional) Set Output Filename:** A new input field will appear with the download button. You can enter a custom name for the compressed file before downloading.
6.  **Download:** Click the "Download Image" button to save your compressed file.

## Technology Stack

*   **Frontend:** HTML5, CSS3, JavaScript
*   **Libraries:** jQuery

## Testing & Verification

The compressor was actually exercised in a real browser (Playwright,
served locally), not just read: uploaded a real image with a
transparent background, set a target size well below the original,
and inspected the actual output pixels and downloaded filename.

That run surfaced two real bugs, both fixed:

- **Transparency silently became solid black.** Output is always
  re-encoded as JPEG, which has no alpha channel. The canvas used for
  re-encoding was never explicitly filled before drawing the source
  image onto it, so any originally-transparent pixels rendered as
  black (a browser canvas's default backing color) once flattened to
  JPEG — a logo or icon with a transparent background would come out
  looking broken. Fixed by filling the canvas white before drawing,
  the standard convention for flattening transparency to a
  non-alpha format.
- **Downloaded files kept the wrong extension.** The output filename
  always preserved the original file's extension (e.g. `photo.png` ->
  `compressed_photo.png`), but the actual file content is always
  JPEG-encoded regardless of the source format — so a "PNG" download
  was actually JPEG data under a mismatched extension. Fixed the
  default filename and the custom-filename fallback to use `.jpg`.

## Known Limitations

- Always re-encodes to JPEG; there's no option to preserve the
  original format or to compress losslessly.
- Very small target sizes on large images fall back to aggressively
  downscaling (down to as little as 10x10px) to hit the target,
  which can produce a barely-recognizable result — this is an
  inherent tradeoff of the size-first compression approach, not a
  bug, but worth knowing before setting an unrealistic target.
- No automated test suite — verification was exercising the real app
  in a real browser.

## Future Development

Imagetect is designed to be an expandable platform for image utilities. Future updates will introduce a range of new tools, including:

*   **Image Converter:** Convert images between different formats (e.g., PNG, JPG, WEBP).
*   **Bulk Resizer:** Resize multiple images to specific dimensions at once.
*   **Watermarking Tool:** Add custom text or image watermarks to your pictures.
*   **Filter Library:** Apply a variety of filters and effects.

## License

MIT — see [LICENSE](LICENSE).

---

We hope you find Imagetect useful! We are excited to continue its development and add more features soon.
