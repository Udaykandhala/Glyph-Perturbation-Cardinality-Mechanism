Glyph Perturbation Cardinality (GPC)
Raster-Domain Steganography for Multimodal Data Embedding

Overview
This repository contains the reference implementation for Glyph Perturbation Cardinality (GPC), a raster-domain steganographic framework that embeds information directly into the pixel structure of rendered text glyphs.
Unlike traditional text steganography methods that operate on linguistic, syntactic, or encoding-level properties, GPC functions exclusively after font rasterization. Information is embedded by perturbing a controlled number of interior ink pixels within each glyph, producing visually imperceptible yet deterministically decodable signals.

The framework is modality-agnostic and supports embedding of:
Text to Text
Image to Text
Audio to Text
Video to Text

All embeddings rely on the same underlying raster perturbation mechanism.

Core Idea
Each glyph is rendered using a strictly deterministic text rasterization pipeline (fixed font, size, resolution, and rendering backend).
A payload value 
𝑣 ∈ [0,26]
v ∈ [0,26] is encoded by perturbing exactly v interior ink pixels of the glyph bitmap:

Perturbations are minimal (e.g., 0 to 1 grayscale increment)
Glyph contours, shapes, and visual identity remain unchanged
Decoding is achieved by re-rendering the canonical glyph and counting pixel wise differences
This transforms rendered text into a covert, structured raster communication channel.

Key Properties
Deterministic: Identical rendering parameters produce identical glyph rasters
Reversible: Payload recovery via exact perturbation counting
Visually Imperceptible: Modifications are confined to glyph interiors
Modality Independent: Same channel used for text, image, audio, and video
Lightweight: No learning, training, or optimization required

Implemented Pipelines
1. Text to Text
Encodes secret text characters into cover text glyphs using perturbation cardinality.

Payload: A-Z >> integers 1-26
Metrics: CER, BER, MSE
Result: Perfect recovery under deterministic rendering

2. Imageto→ Text
Embeds RGB image content into glyph streams.

Images resized to canonical resolution
Pixel intensities normalized to [0, 26]
Channels embedded independently
Metrics: MSE, MAE, RMSE, PSNR, SNR, SSIM

3. Audio to Text
Encodes audio using frame level RMS descriptors.

Audio segmented into overlapping frames
RMS values normalized to [0, 26]
Reconstructed using overlap-add synthesis
Metrics: MSE, MAE, SNR, spectral diagnostics

4. Video to Text
Processes video frame-by-frame without temporal coupling.

Each frame treated as an independent RGB image
Canonical, encoded, and difference glyph rasters saved per frame
Decoded frames reconstructed and stored
Aggregate metrics computed across frames
Cover Text Utilization

All scripts explicitly report:

Total alphabetic glyphs available in the cover text
Glyphs used per modality or frame
Unused glyphs
Effective utilization percentage
This makes channel capacity and reuse behavior explicit and auditable.
Deterministic Rendering Assumptions

All experiments assume:
Fixed font file and font size
Fixed raster resolution
No adaptive anti-aliasing or subpixel jitter
Identical rendering backend during encoding and decoding
These constraints ensure stable glyph rasters and exact payload recovery.

Intended Use
This codebase is intended for:
Research and reproducibility
Steganography and information hiding studies
Cross modal representation research
Deterministic raster communication experiments
It is not intended as a cryptographic system or secure messaging product.

License
This repository is released for academic and research use.
Please contact the author for commercial licensing or derivative work permissions.

Datasets are not included.
Users may supply any text, image, audio, or video file.
