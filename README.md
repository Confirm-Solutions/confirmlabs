# confirmlabs

We use Quarto for running this website. To write a new post:
- create an .ipynb or .md file in the `posts/` folder.
- copy the header from one of the existing posts. Edit to set the date and author and such.
- leave the `bibliography` portion of the header. Then, you can add and cite articles in the `biblio.bib` file.
- for local preview: 
  - `brew install quarto` 
  - `quarto preview`
- committing and pushing to the `main` branch will trigger an Actions workflow that will update the main website.

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

