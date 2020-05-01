# Substrate Tutorials

This repository holds documentation for Substrate tutorials. The main website is hosted in
[substrate-cms](https://github.com/substrate-developer-hub/substrate-cms).

## Structure

All docs are written in markdown format. Each tutorial is in its own folder and has subfolders for
each of its versions; supporting figures and images for all versions of all tutorials go in the
`assets` folder.

## Contributing

Feel free to add an issue if there is documentation that you'd like to see. See the
[contributing guide](https://github.com/substrate-developer-hub/knowledge-base/blob/master/CONTRIBUTING.md)
for guidance on making PRs.

### Prettier

This repository uses [Prettier](https://prettier.io/) to ensure consistent formatting. Please run
Prettier before submitting any changes to the files in this repository. Prettier ships as an
[NPM](https://www.npmjs.com/) package, so NPM must installed to use it. Prettier can be installed
globally with `npm i -g prettier` in which case you can run `prettier --write .` in the project
root. If you prefer not to install Prettier you can run it on-the-fly with `npx prettier --write .`.
In either case, the `.prettierrc.json` file that ships with repository will be used to ensure
consistent formatting.

## License

All documentation is under the
[Apache 2 License](https://github.com/substrate-developer-hub/knowledge-base/blob/master/LICENSE).
