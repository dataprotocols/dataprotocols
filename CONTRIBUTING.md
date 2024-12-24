---
title: Contributing
sidebar:
  order: 8
---

There are many ways to contribute to the Data Package standard. Here are a few:

- **Use the standard**: The best way to contribute is to use the standard in your projects and provide feedback on what works and what doesn't.

- **Spread the word**: Help us spread the word about the Data Package standard. The more people who know about it, the more people who can contribute.

- **Participate in discussions**: Share your ideas and feedback in the [Data Package Discusions](https://github.com/frictionlessdata/datapackage/discussions). We use a voting system to prioritize issues and feature requests.

- **Report issues**: If you find a bug or have a feature request, please report it in the [GitHub issue tracker](https://github.com/frictionlessdata/datapackage/issues). There a few predefined issue templates to help you get started.

- **Contribute to the repository**: You can contribute to the Data Package standard repository. Please read this guide for more details on the process.

## Research and development

The Data Package standard is developed using a research and development model. This means that the standard is developed in an iterative way, with new features and changes being proposed, discussed, and implemented in a collaborative way.

There are two main feedback trackers for the Data Package standard:

- **[Data Package Discussions](https://github.com/frictionlessdata/datapackage/discussions)**: This is where general discussions and new feature proposals to the Data Package standard take place. You can use this forum to share your ideas, ask questions, and provide feedback. Using discussions and the voting mechanism, the community and the Data Package Working Group can help prioritize issues and feature requests. The discussions is a good place to design and share a feature draft or implementation details. Generally speaking, this is a research phase.

- **[Data Package Issues](https://github.com/frictionlessdata/datapackage/issues)**: This is where bugs, general improvements, and specific feature requests are tracked. The issues here must be actionable and have a clear scope. The Data Package maintainers might move an issue to the Discussions if it needs to be discussed or voted first or close one if it is not planned to implement. Generally speaking, this is a development phase.

## Branching and releasing

:::note
The term `profile` is used to refer to the Data Package profiles -- JSONSchema files generated from the `profiles` folder in the repository.
:::

The Data Package project uses two main branches:

- **`main`**: This is the main public branch. The live https://datapackage.org website is built from this branch. Following pull requests types are allowed: documentation updates, chore improvements, and other minor changes.

- **`next`**: This is the development branch. New features and changes are implemented in this branch. It also includes all the bug fixes, that need to update profiles, as Data Package follows the immutable public profiles model.

### Releasing model

This process is used for a new Data Package version release:

- A maintainer creates a new pull request named `Release vX.Y` from the `next` branch to the `main` branch that includes a version bump in `package.json` with `npm run generate` command run to update the profiles.

- All pull request meant to be included in the release should be merged into the `next` branch following standard [review/voting process](/overview/governance#decision-making). It is recommended to include a changelog entry as a part of a pull request. Maintrainers merge these pull requests using "Squash and merge" strategy.

- When the `Release vX.Y` is ready to be released, the maintainer ensure the changelog correctness, resolves conflicts if needed, and merges the `next` branch into the `main` branch using "Create a merge commit" strategy. After the website successfully builds, the maintainer creates a new tag and release on GitHub linking the changelog entry with release notes for the version released.

### Codebase contribution

The Data Package project is based on Astro Starlight static-site generator and uses TypeScript/Node.js for building process. Here are the steps to contribute to the codebase.

1. **Fork the repository**: Click the "Fork" button in the upper right corner of the repository page.
2. **Clone the repository**: Clone the forked repository to your local machine.
3. **Install dependencies**: Run `npm install` to install the project dependencies.
4. **Start a dev server**: Run `npm start` to see the changes locally.
5. **Make changes**: Make your changes to the codebase.
6. **Generate profiles**: Run `npm run generate` to generate the profiles.
7. **Format style**: Run `npm run format` to format the style.
8. **Run tests**: Run `npm test` to run the tests.
9. **Commit changes**: Commit your changes to your forked repository.
10. **Create a pull request**: Create a pull request from your forked repository to the appropriate branch of the main repository (see the Branching Model above).

When a pull request is created, it will be reviewed by the Data Package maintainers. Github Automation creates a live preview site for every new pull request. Once the pull request is approved, it will be merged into the main repository.

Note that the project uses two different types of produced artifacts:

- **npm run generate**: This command generates the profiles from the `profiles` folder and sync profile version in the specs. It is required to run this command after changing the YAML/JSON files in `profiles` folder, and the output has to be committed to the repository.

- **npm run build**: This command builds the project and generates the static site. This command is run by automation and the output is not commited to the repository. As a contributor, you don't need to run this command although you can use `npm run preview` to debug the site built in production-mode locally.

## Backward compatibility

This section outlines the rules for backward compatibility in the Data Package specification.

1. An existing `datapackage.json` that is valid MUST NOT becoming invalid in the future.
2. A new `datapackage.json` MAY be invalid because a software implementation does not support the latest version of the specification (yet).

### Versioning

1. The Data Package specification is versioned. This is new over 1.0, where changes were added without increasing the version.
2. The Data Package specification is versioned as a whole: a number of changes are considered, discussed, added or refused and released as a new minor version.

### Property changes

1. A property MUST NOT change `type`
2. A property MAY allow additional `type` (array)
3. A property MUST NOT become `required`
4. A property MAY become optional. Example: https://github.com/frictionlessdata/datapackage/pull/7
5. A property MUST NOT add `enum`
6. A property MAY remove `enum`. Example: https://github.com/frictionlessdata/specs/pull/809
7. A property MUST NOT remove `enum` values
8. A property MAY add `enum` values

### Table schema changes

1. A field type MUST NOT change default `format`. Example: does https://github.com/frictionlessdata/datapackage/pull/23 align with this?
2. A field type MUST NOT remove `format` pattern options
3. A field type MAY add `format` pattern options

### New properties

1. A new property MAY make a `datapackage.json` invalid (because of general rule 2). Example: https://github.com/frictionlessdata/datapackage/pull/24
2. A new property CANNOT be `required`

### Removed properties

1. Removing a property CANNOT make a `datapackage.json` invalid (because of general rule 1)
