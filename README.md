# website

Documentation website for [roche](https://github.com/roche-rs/roche) commandline.

Hosted at https://roche-rs.org 

[![Netlify Status](https://api.netlify.com/api/v1/badges/f50fd857-bda0-489b-9ba1-f9d207f18384/deploy-status)](https://app.netlify.com/sites/roche-rs/deploys)

<div>Icons made by <a href="https://www.flaticon.com/authors/freepik" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>

## develop

This website is built using [mdbook](https://github.com/rust-lang/mdBook) from rust-lang.

1. install mdbook - Detailed instructions [here](https://rust-lang.github.io/mdBook/cli/index.html)
    ```bash
    cargo install mdbook
    ```

2. clone this repo
    ```bash
    git clone https://github.com/roche-rs/website
    cd website
    ```

3. Use mdbook to serve the pages and auto-rebuild when changes are made.
    ```bash
    mdbook serve
    ```

4. View the site in a browser - http://localhost:3000

## contribution

This website is an **OPEN Open Source Project**. This means that:

> Individuals making significant and valuable contributions are given commit-access to the project to contribute as they see fit. This project is more like an open wiki than a standard guarded open source project.

See the [Contribution Guide](CONTRIBUTING.md) for more details.
