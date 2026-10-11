# Practical Image Workflows for Web Publishing

A field guide for bloggers and web builders preparing photographs, screenshots, and illustrations for publication. Start with the publishing requirement, keep an untouched master, and compare the actual exported files.

**Research checked: 11 October 2026.** Product limits below are documented vendor claims, not independently measured performance results. No compression benchmark was collected for this edition.

**Affiliation:** This resource is published by someone affiliated with TinyImage, DR Checker, HEIC to PNG, and HEIC to JPEG. Competing tools are included. Inclusion is not an independent endorsement or a promise of better search rankings.

## Contents

- [Choose an output](#choose-an-output)
- [Choose a tool](#choose-a-tool)
- [Workflow: iPhone photograph to blog](#workflow-iphone-photograph-to-blog)
- [Workflow: screenshot with small text](#workflow-screenshot-with-small-text)
- [Responsive publishing example](#responsive-publishing-example)
- [Run a reproducible comparison](#run-a-reproducible-comparison)
- [Privacy and metadata](#privacy-and-metadata)
- [Inclusion and contributions](#inclusion-and-contributions)
- [Sources](#sources)

## Choose an output

| Content and requirement | Starting candidate | What to inspect |
| --- | --- | --- |
| Photograph for an ordinary article | JPEG; compare WebP or AVIF when your publishing system supports them | Skin tones, foliage, gradients, output bytes |
| Screenshot containing interface text | PNG; compare lossless WebP | Letter edges at the rendered size |
| Image requiring transparency | PNG or WebP | Edge halos against both light and dark backgrounds |
| Scalable logo already available as a vector | SVG | Rendering and suitability of the source; do not rasterize by default |
| HEIC from a phone that your CMS rejects | Decode to PNG for an editing intermediate, or JPEG for a straightforward photo export | Orientation, colors, dimensions, metadata |

These are starting points, not universal winners. PNG can preserve the decoded pixels without further lossy encoding, but cannot recover detail already lost in the source. JPEG is lossy; repeatedly saving a JPEG can add degradation. Modern formats still need an actual comparison on your images. See the [MDN format guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types).

Do not choose PNG merely because it sounds higher quality. A large photographic PNG may be an unnecessary download. Conversely, compressing a screenshot until its text becomes blurry defeats the purpose of publishing it.

## Choose a tool

| Tool | Useful role | Documented free access | Processing and practical limitation |
| --- | --- | --- | --- |
| [TinyImage](https://tinyimage.io/) | Hosted compression, conversion, resizing and related image tasks | Website: 250 successful conversions per month; batches up to 20 and 28 MiB per file | Compression/conversion can use temporary server storage. Tool-specific limits can differ. |
| [HEIC to PNG](https://heictopng.io/) | HEIC/HEIF to PNG before editing | No signup or watermark; up to 10 files, 50 MB each, 150 MB total | Vendor describes local browser conversion; EXIF is not copied. Browser memory can constrain large batches. |
| [HEIC to JPEG](https://heictojpeg.io/) | HEIC/HEIF to a broadly compatible photo file | No signup or watermark; up to 10 files, 50 MB each | Vendor describes local conversion, resize and ZIP download; default JPEG quality is 92%. EXIF is not copied. |
| [Squoosh](https://squoosh.app/) | Interactive comparison of encoding choices | Free browser application | Processes images locally according to its homepage; useful for inspecting individual exports. |

TinyImage's **API allowance is different**: the documented free 250 conversions are a one-time trial following registration and email verification. Do not design an ongoing free automation around a monthly API allowance. Consult the [current product information](https://tinyimage.io/) before building a pipeline.

The two HEIC converters are task-specific alternatives. Use one appropriate output, rather than converting the same photograph through both simply to complete a checklist. Their advertised lack of a repeat-use quota does not imply unlimited file size or unlimited browser memory. Limits and claims in this table come from the linked homepages, checked on the date above.

## Workflow: iPhone photograph to blog

1. Save the original HEIC in your private archive. Work on a copy and choose a nonsensitive test image when evaluating a new service.
2. Check whether your editor accepts HEIC directly. If it does, an intermediate conversion may be unnecessary.
3. If it does not, choose a conversion path. [HEIC to JPEG](https://heictojpeg.io/) is a direct option for a photographic export. [HEIC to PNG](https://heictopng.io/) can provide a decoded editing intermediate when you intend to crop, annotate or retouch before the final export.
4. Correct the crop before resizing. Decide what the article needs the reader to see: a whole scene, a product detail, or a specific part of an interface.
5. Export candidates at the dimensions your layout actually needs. Try your CMS image pipeline, TinyImage, or Squoosh. Compare output files rather than treating quality numbers from different encoders as equivalent.
6. Open the downloaded file. Check orientation, skin tones, gradients and fine detail. A successful download alone does not establish visual quality.
7. Upload the chosen result, supply contextual alternative text, and inspect the published page on a narrow screen and a desktop.

If an image will appear at 800 CSS pixels wide, an 800-pixel and a 1600-pixel candidate give you a useful initial comparison for ordinary and denser displays. This is an illustrative sizing decision, not a recommendation to send a 1600-pixel image to every visitor. Responsive selection should let the browser request an appropriate candidate.

## Workflow: screenshot with small text

Keep the screenshot lossless while cropping and annotating. Remove unrelated interface areas so the instructional detail occupies more of the final image. Export PNG and, if your stack supports it, a lossless WebP alternative. View each at the same rendered width as the article.

Check punctuation, thin rules, and labels against the original. If readers need to zoom to understand the screenshot, consider splitting it into steps or adding a caption that states the important result. A smaller byte count is useful only when the screenshot still communicates its information.

Never publish a screenshot before checking visible tabs, notifications, email addresses, access tokens and customer information. Removing EXIF will not remove sensitive text already drawn into the pixels.

## Responsive publishing example

The following is an original illustrative example. Generate the referenced files first and replace the text and dimensions with those of your own image. It has not been deployed or performance-tested as part of this guide.

```html
<img
  src="/images/garden-800.jpg"
  srcset="/images/garden-800.jpg 800w,
          /images/garden-1600.jpg 1600w"
  sizes="(max-width: 840px) calc(100vw - 32px), 800px"
  width="1600"
  height="1067"
  alt="Raised garden beds beside a gravel path"
>
```

The `sizes` value must reflect the actual CSS layout. Preserve the output aspect ratio in `width` and `height`. Add lazy loading to appropriate below-the-fold images after checking your framework's behavior; do not automatically delay the article's main visible image.

Google's [image publishing guidance](https://developers.google.com/search/docs/appearance/google-images) covers discoverable image elements, responsive sources and descriptive image context. Serving a well-prepared image is only one part of publishing a useful page.

## Run a reproducible comparison

Use this protocol before claiming that a tool is faster, smaller or visually better:

1. Select a photograph, a screenshot with text, and an image with transparency for which you have publishing rights. Record a SHA-256 hash for each original.
2. Record browser/version, operating system, date, output format, target dimensions, quality setting and any metadata option.
3. Keep output dimensions and format identical for a fair first comparison. Changing both the encoder and dimensions answers a different question.
4. Save every output using a descriptive filename. Record original and output bytes from the files, rather than copying rounded interface labels.
5. Calculate `100 × (original_bytes - output_bytes) / original_bytes`. Negative savings are valid and should remain in the results.
6. Inspect at the published size and at 100% pixel view. Record visible failures rather than presenting one subjective score as universal quality.
7. If timing, define the start and end events. Separate network transfer from local processing where possible and repeat runs. Do not infer speed from a single attempt.

Suggested result columns:

```text
date,source_sha256,content_type,tool,browser,format,width,height,
quality_setting,original_bytes,output_bytes,savings_percent,
elapsed_seconds,visual_notes,metadata_notes
```

Leave unknown fields empty. This edition deliberately contains no invented result rows. A site's example savings percentage is a marketing example until you reproduce it on a documented input.

## Privacy and metadata

Decide whether uploading the image is acceptable before selecting a tool. Local conversion is useful when an image should stay on the device, but a claim about file processing does not establish that a website collects no analytics or connection logs.

TinyImage's [privacy policy](https://tinyimage.io/privacy) describes temporary media storage, scheduled cleanup after 24 hours, and exceptions for legal, security or fraud matters. This is not a promise that every category of account or analytics data disappears within 24 hours. Its background-removal workflow is described separately as browser based; do not extend that claim to all tools.

The HEIC sites state that EXIF is not copied into converted output. If capture time, GPS or camera data is important to your archive, preserve the original separately. If privacy is the goal, inspect the downloaded output before publishing. Also inspect filenames, captions and visible content; metadata removal alone is insufficient.

## Inclusion and contributions

Include tools that solve a concrete publishing task, have a usable free route, explain meaningful restrictions, and can be linked to primary documentation. Commercial and open-source tools can both qualify. Paid-only products, unsubstantiated benchmarks, affiliate redirect links and unrelated profiles do not qualify.

To suggest a correction, open an issue or pull request with the affected section, the exact official source URL, verification date, free limits and processing model. Disclose your affiliation. For a performance claim, attach the input provenance and reproducible result records. Keep entries concise and avoid superlatives. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Sources

Primary references checked on 11 October 2026:

- [TinyImage product information](https://tinyimage.io/) and [privacy policy](https://tinyimage.io/privacy)
- [HEIC to PNG product information](https://heictopng.io/) and [about](https://heictopng.io/about-us/)
- [HEIC to JPEG product information](https://heictojpeg.io/)
- [Squoosh](https://squoosh.app/)
- [MDN image format guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types)
- [Google image publishing guidance](https://developers.google.com/search/docs/appearance/google-images)

Vendor descriptions establish what the vendors document. They do not independently verify every implementation, privacy practice or output-quality claim. Recheck changing limits before relying on a service.
