# golemWebR

This repositories shows a `{golem}` powered app boilerplate with the
necessary elements to be hosted on Netlify, __without any Shiny server__.

The app is deployed [here](https://golem-webr.rinterface.com/) at https://golem-webr.rinterface.com/.

I leveraged the `{golem4Shiny}` [package](https://github.com/RinteRface/webR4Shiny) to create the corresponding infrastructure. What does `{golem4Shiny}`? Essentially, it:

- Copies relevant part of the `{golem}` app (`R`, `inst`) into the folder of your choice, default being `./webR`.
- Tweaks some of the `{golem}` copied files. Importantly, this __does not impact__ the main app files.
- Adds a slightly modified version of the JS technology to run webR and adapt with Shiny (originally provided by to George Stagg: https://github.com/georgestagg/shiny-standalone-webr-demo).
- Adds a makefile to run the app locally and test.

## Run it locally

cd to `./webr` and run `make` to serve the app locally.
Browse to `localhost:8080` to view the app.

## Deploy on Netlify

Link your repo to Netlify and deploy the `./webr` folder only.

## Notes

Don't forget that there are few tweaks necessary to make it possible to work:

- We can't use `pkgload::load_all()` as in the usual `{golem}`
`app.R` file because local packages can't be installed on webR (we would need
a compiler for wasm). So we rely on the `{shiny}` autoload feature since 1.5.0, to load all files from the R folder (except the autoload one, which is not uploaded to the webR VFS).
- `{golem}` `app_sys()` also needs some tweaks since we can't do `system.file(..., package = "<local_package_name>)`, the package being
impossible to install locally.
- `golem::favicon()` does not seem to work, likely because of an issue
between `{fs}` and `webR` so we commented it out.
