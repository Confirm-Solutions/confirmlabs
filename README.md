# confirmlabs

We use Quarto for running this website. To write a new post:
- Create an .ipynb or .md file in the `posts/` folder.
- Copy the header from one of the existing posts. In the YAML "frontmatter" at the top of the post, edit to set the date and author and such.
	- Add `description: 'some description'` to the frontmatter to set the short blurb that is used on the main post listing page. 
	- Add `image: filename` to the frontmatter to set the image used on the main post listing page.
- Leave the `bibliography` portion of the header. Then, you can add and cite articles in the `biblio.bib` file.
- For local preview: 
  - install on mac: `brew install quarto` 
  - install on ubuntu: `sudo curl -LO https://quarto.org/download/latest/quarto-linux-amd64.deb; sudo apt-get install gdebi-core; sudo gdebi quarto-linux-amd64.deb`
  - `quarto preview`
- Committing and pushing to the `main` branch will trigger an Actions workflow that will update the main website.
	- You can watch that Actions workflow here: https://github.com/Confirm-Solutions/confirmlabs/actions

## Writing checklist

- the obvious: typos and grammar.
- did the post get messed up somehow when pushed and visible on confirmlabs.org instead of locally?
  - did you forget to `git add` an image?
- arxiv links are links to abstracts and not pdfs.
- all links work correctly.
- consistent capitalization and word usage.
- where appropriate, convert parentheticals to footnotes?

## More info:

- [the quarto guide is great](https://quarto.org/docs/guide/)
- [good blog post](https://blog.djnavarro.net/posts/2022-04-20_porting-to-quarto/#fnref1)


options for data tables in quarto:
- https://vincjo.fr/svelte-simple-datatables/#/
- https://mwouts.github.io/itables/quick_start.html
- observablejs
- https://datatables.net
	- https://datatables.net/examples/data_sources/server_side

js plotting libraries:
- plotly
- d3
- threejs
- Plot (based on d3)
- raw svelte
- layercake
- observable: https://observablehq.com/@observablehq/plot-gallery

svelte + d3:
- https://www.connorrothschild.com/post/svelte-and-d3
- https://www.youtube.com/watch?v=JDEtUjjdVag
- https://github.com/JeffreyPalmer/svelte-d3-examples
- https://layercake.graphics

mixing svelte in quarto:
- https://rollupjs.org
- sverto: https://sverto.jamesgoldie.dev
	- demo: https://svelte-in-quarto.pages.dev/test-quarto-dynamic
	- https://github.com/jimjam-slam/sverto (note the "Use pre-compiled Svelte components" section)
	- proof of concept: https://github.com/jimjam-slam/svelte-in-quarto

