# Code Manifesto

## Repositories and Workflow

-   The default branch is always `develop` and the production code is located in the branch `main`
-   We do not commit code directly on main branches like `develop` or `main`, instead we always create an own branch to work with
-   We always create a pull request with short, specific and clear comment for new or altered issues and let it be reviewed by an internal Swiss Red Cross (SRC) developer. The code is only merged by the SRC if one of the following conditions apply:
    1. The PR has been approved
    2. All change requests and feedback has been resolved
    3. Change requests and feedback has been answered and follow-up tasks has been defined (ideally with an issue in Github) and further steps for this issue are planned.
-   We add sufficient inline comments to explain why this lines of codes exists (rather then what they do)
-   All exported functions contain a jsdocs comment block at least containing
    -   A description
    -   Params (with type & description)
    -   return value
-   repositories are not located in a private user account but in `swissredcross` and assigned to a team.
-   The repos must contain rules which regulates the allowance to push to the main and develop branch (only with an approval in PRs)

## Branch Naming Convention

#### We name branches according to following convention:

-   Monorepos (eg. unity): `[type]/[target]-[description]`
-   other repos: `[type]/[description]`

#### Explanation

-   `[type]` should be one of the following types: `feature`, `fix`, `refactor`, `docs`, `test` or `chore`
-   `[target]` equals the short code for the site changed, the name of the package changed or `unity` if it applies to everything.
-   `description` contains the description, words are separated by a `-`

#### Examples:

-   `feature/report/download-view`
-   `fix/2030/overflow`
-   `chore/unity/upgrade-dependencies`

## File Naming

-   For react components we name the file exactly the same as the component (PascalCase).
    -   Example: the file name for the component `<FooBarHeader>` would be `FooBarHeader.js`
-   for js files we give them a all small case name while separating words with a `-`
    -   Example: the file name for the function `addFooBarItem()` would be `add-foo-bar-item.js`
-   If multiple files belong strongly together, we use the same beginning and add the specific usecase separated by a dot after the name and before the file extensions
    -   Example: For the extensive Component `<FooBar>` we end up with a files structure like:
        -   `FooBar.component.js`
        -   `FooBar.hooks.js`
        -   `FooBar.stories.mdx` - `FooBar.types.js`

## Packages

-   Each package must contain a `src` folder containing all of the actual sourcecode
-   Each package must contain a `index.js` file which only conatin export to other file but no code itself
-   We scope all packages using `@swissredcross`
-   For UI-Components, we place them in the folder `ui-components` and use a name like `@swissredcross/ui-components_react_[NAME]`
-   Other packages are located in `packages`

## Definition of Done

To consider a task / feature done, it must fullfill all of the following requirements:

-   [ ] Reviewed by another SRC Developer
-   [ ] Valid Linting for JS and CSS without errors
-   [ ] UI-Components: a storybook story with a brief example and explanation exists
-   [ ] Best practices in UX/UI must be fullfilled

## General JS rules

-   Try to take advantage of newer, performant features of ECMAScript 2018+
-   Do not emit unnecessary messages to the console (eg. logging for testing etc.)
-   If emiting an error, always prefix it with the name of the site or the name of the package (for easier debugging)
-   Use [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) when using 2+ variables in a string

## Storybook

-   Each react component should be paired with an `ComponentName.stories.mdx` file
-   For each available variants a own `<Story>` should be created with a preview of this variant
-   The props are displayed (`<Props>`)
-   A meaningful introduction with some explanation and background information is desirable
-   if the component expect some very specific input (deep arrays, complex objects etc.) some example data must be provided within the story
-   For Layout Components, additional examples combined with other elements are desirable.

## React Guideline

-   We follow the main [Thinking in React](https://reactjs.org/docs/thinking-in-react.html) principles
-   Create functional React components with React Hooks over class components
-   We always declare `PropTypes` and `DefaultProps`
-   Avoid using `props` as only prop
    -   Bad: `const FooBar = (params) => {}`
    -   Good: `const FooBar = ({id, slug}) => {}`
-   The order of instructions within a component should be like following (exceptions for grouping it based on feature can be made): 1. constants from hooks 2. states 3. constants used in whole component (if only used for a few lines place them in the beginning of the section) 4. Effects and other react core functions 5. further code
-   Instead of inventing the wheel again, we can build up on existing frameworks. We prefer [reach-ui](https://github.com/reach/reach-ui) as first choice and [reakit](https://reakit.io/) as a second option.
-   A great collection of different hooks (for event listening, resizing, scrolling etc.) we like to use is [react-hook](https://github.com/jaredLunde/react-hook)
-   Another awesome list of React components & libraries can be found at [awesome-react-components](https://github.com/brillout/awesome-react-components)

## Versioning

If we need a versioning, we follow the [semver](https://semver.org/) guidelines

## Styling

-   We try to avoid `:global` rules as much as possible and try to keep the code simple
-   We use `styled-jsx` in combination with `PostCSS`
-   We follow the [Enduring CSS](https://ecss.io/) guide for naming
-   Consider separating component scoped `<style jsx>` and global styles `<style jsx global>` for performance reasons.
-   Try to not redesign already existing features but do reuse the functionalities, for example:
    -   Get theme based variables, Breakpoints / Viewports, Spacings, fontSizes etc. from `@swissredcross/ui-components_react_theme`
    -   Use the `<Container>`, `<Col>` and `<Row>` for grids (see `@swissredcross/ui-components_react_foundation`)

### Z-Indexes

When handling with z-index, we ensure to stay in the ranges defined below.  
To be able to distinguish between the main layout components, we increment by 100 between components. An example: The footer has to be always underneath the header so it has z-indexes somewhere in between `100-199`. The floating navigation must be over the footer but below the header bar so it uses `200-299`. Finally the header bar is always at the top and therefore recieves the highest numbers `300-399`.
We try to not skip numbers (per range).

| z-index Range | Usage                                                      |
| ------------- | ---------------------------------------------------------- |
| 0-99          | Arrangement within a component                             |
| 100-499       | Main Layout Components (Header / Navigation / Footer etc.) |
| 500-599       | Popovers, Popups etc. (not fullscreen)                     |
| 600-699       | Overlays                                                   |

## Linting

We use [eslint](https://eslint.org/) as linting tool and provide an automated validation through a script in `package.json`
Following our custom configuration `.prettierrc.json`

```
{
    "parser": "babel-eslint",
    "env": {
        "browser": true,
        "commonjs": true,
        "es6": true,
        "node": true
    },
    "extends": [
        "eslint:recommended",
        "standard",
        "plugin:jsx-a11y/recommended",
        "eslint-config-prettier",
        "plugin:prettier/recommended",
        "plugin:react/recommended",
        "prettier/react"
    ],
    "plugins": ["react", "react-hooks", "jsx-a11y", "sort-destructure-keys"],
    "settings": {
        "react": {
            "version": "detect"
        },
        "linkComponents": [
            // Components used as alternatives to <a> for linking, eg. <Link to={ url } />
            "Hyperlink",
            {
                "name": "Link",
                "linkAttribute": "to"
            }
        ]
    },
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "prettier/prettier": "error",
        "linebreak-style": ["error", "unix"],
        "sort-destructure-keys/sort-destructure-keys": "error",
        "react/jsx-sort-default-props": "off",
        "react/no-array-index-key": "error",
        "react/no-unused-prop-types": "error",
        "react-hooks/rules-of-hooks": "error",
        "react-hooks/exhaustive-deps": "warn",
        "no-var": 1
    }
}

```

## Formatting

We use [prettier](https://prettier.io/) as formatting tool and provide an automated validation through a script in `package.json`
Following our custom configuration `.prettierrc.json`

```
{
    "singleQuote": true,
    "semi": false,
    "trailingComma": "none",
    "arrowParens": "always"
}
```
