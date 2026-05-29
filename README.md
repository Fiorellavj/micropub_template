# ISP Micropublication Template

This repo contains a template for creating Impact Scholars Program micropublications using [MyST Markdown](https://mystmd.org/).

For a complete tutorial, read [Authoring an ISP micropublication](https://impact-scholars.github.io/isp-intro)

## Quick setup

If you're familiar with the tools involved, these few steps are all you need. We expect you to have a working conda/mamba setup. For guidance on this, see the [Conda/Mamba Tutorial](https://impact-scholars.github.io/conda-tutorial).


1. Install MyST Markdown using the provided environment file:

   ```bash
   mamba env create -f environment.yml
   mamba activate isp-paper
   ```

1. Edit your content in `index.md`
1. Add your figure (replace `figure.png`)
1. Update `myst.yml`
1. Replace the thumbnail in `thumbnails/thumbnail.png` for the gallery display
1. **Preview locally**:
   - While you're working on the paper, the best option is to run `myst start`
   - Before pushing, check that the static build (more detail in optional reading) is working as expected

   ```bash
   myst build --typst && myst build --html && npx serve _build/html
   ```
1. Update this README.md: include any information that someone reaching the repository may need. You can keep this README if you prefer, it won't be rendered.

## File Structure

| Path | Purpose |
|------|---------|
| `myst.yml` | Metadata: authors, title, keywords, DOI |
| `environment.yml` | Conda/mamba environment for MyST |
| `source/index.md` | Paper content (MyST Markdown) |
| `source/bib.bib` | Bibliography in BibTeX format |
| `source/figure.png` | Main composite figure |
| `source/thumbnails/thumbnail.png` | Gallery thumbnail image |
| `output/` | Built outputs from initial submission (PDF, HTML) |
| `revision/` | Revision materials (response to reviewers, revised PDF) |

---

## I want to know more!

We've outlined some technical details [here](https://impact-scholars.github.io/myst-deeper).

