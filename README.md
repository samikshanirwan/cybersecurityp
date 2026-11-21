# StegoLab

StegoLab is a single-page, browser-based steganography learning and analysis tool. It embeds and extracts messages in images, hides text with zero-width Unicode characters, and checks images for several possible signs of hidden data. Processing is performed locally in the browser; files are not uploaded.

## Run it

Open the HTML file in a modern browser. No build step or server is needed for the basic features. Image encryption uses the browser's Web Crypto API, which requires a browser that supports `crypto.subtle`.

## Features

- **Hide in image:** Embed a text message in an image's RGB least significant bits (LSBs). Choose 1–3 bits per color channel to trade image quality for capacity. Optionally protect the message with AES-256-GCM; the key is derived from the password with PBKDF2 and a random salt. Download the result as PNG to preserve the embedded bits.
- **Extract from image:** Read StegoLab messages from an image. For password-protected messages, enter the same password used during embedding.
- **Invisible text:** Encode a secret as zero-width Unicode characters inside visible cover text, copy the result, and scan text to reveal a StegoLab-formatted hidden message.
- **Detect & analyze:** Inspect an image's LSB plane, check for a StegoLab header, calculate basic LSB statistics, look for bytes after PNG/JPEG end markers, and display the image dimensions, file size, and SHA-256 hash. The verdict is heuristic and is not proof that a file does or does not contain hidden data.
- **Self-test:** Run six in-page image embed/extract round-trip checks across bit-depth and password settings.
- **Theme:** Toggle light/dark appearance; the page also follows the system color preference by default.

## Image message format

The image payload begins with a four-byte `STG1` or `STG2` tag, followed by a four-byte payload length and the message bytes. RGB channels are used in pixel order; alpha values are skipped. `STG2` marks an AES-GCM-encrypted payload. JPEG recompression or other pixel-changing edits can damage or remove an embedded message, so use the downloaded PNG when sharing an encoded image.

## Limitations

This is an educational utility, not a forensic detector. Its analysis checks can produce false positives and false negatives, and it only recognizes its own image header and zero-width text format. The image detector's LSB statistics and trailer checks are indicators, not a general steganography scan. Use it only with files you own or are authorized to inspect.
