## SynBioHub Documentation

This API documentation is live at https://synbiohub.github.io/api-docs/

This project is a customized API documentation site based on Slate.

- Forked from: [slatedocs/slate](https://github.com/slatedocs/slate)

- Built using Docker: Follows instructions from [Using Slate in Docker](https://github.com/slatedocs/slate/wiki/Using-Slate-in-Docker)

- Automated Build: GitHub Actions workflow adapted from [Slate Documentation Builder](https://github.com/marketplace/actions/slate-documentation-builder)

- Key Differences from the Original Slate Repo:
Customized `index.html.md` with project-specific API details

### Run a local instance:

#### Building Slate：

Grab the slate image:

```docker pull slatedocs/slate```

To use Docker to just build your site, run:

```docker run --rm --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate build```

#### Running Slate：

If you wish to run the development server for Slate to aid in working on the site, run:

```docker run --rm --name slate -p 4567:4567 -v $(pwd)/source:/srv/slate/source slatedocs/slate serve```
and you will be able to access your site at http://localhost:4567 until you stop the running container process.

#### Publishing Your Docs to GitHub Pages:

```git commit -a -m "Update index.md```

```git push```

GitHub Actions automatically build and deploy your site to GitHub Pages
