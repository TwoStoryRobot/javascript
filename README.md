# TSR Javascript Style Guide

This guide represents Two Story Robot's approach to Javascript. You can use this
document for internal reference or to share with others.

Thanks to [AirBNB](AirBNB) for heavily inspiring this document.

## How to Update This

To update this document, make a pull request with your recommended changes. The
pull request must receive **an approved review** from at least half the active
development team prior to merging. The pull request itself can be used for 
discussion, comments, and iteration on the proposed change. To ensure others 
are notified, please request reviews from appropriate developers.

Rules here should be 1) actionable and 2) clearly testable in a PR review.

## Style Prescedence

- A project's established style convention takes prescedence over this guide.
  - Why? Many of our projects predate this guide, we may be experimenting with
    a new style, or we may be working on code maintained by someone else.
- Don't include rules in this guide that could be covered by eslint or prettier
  configurations.
  - Why? This lets prettier and eslint worry about the minute details of
    standardizing our code, and keeps this guide focused on high-level patterns
    and things we can't programmatically capture.

## Package Manager

Use `npm` unless there is a specific reason to use `yarn` instead. If `npm` is
not being used, make note in the README as to which package manager to use and
why.

## Automatic Styling

- Use prettier with the configuration from @twostoryrobot/prettier-config
- Use eslint with the configuration from @twostoryrobot/eslint-config
- `npm test` should validate both prettier and eslint in a CI environment
  - You can use [jest-runner-eslint] to run eslint within jest
  - You can use [is-ci-cli] to automatically run either prettier + jest or jest
    interactive

## General

- Use a line length of 80 characters.
- Use camelCase when naming objects, functions, and instances.

```jsx
// bad
const UsTaxes = 1000
const playerID = 10
const OBJEcttsssss = {}
const this_is_my_object = {}
function getUNESCOProperties() {}

// good
const usTaxes = 1000
const playerId = 10
const thisIsMyObject = {}
function getUnescoProperties() {}
```

- Use PascalCase when naming constructors, classes, and components.

```jsx
// bad
const myContainer = styled.div`
  color: blue;
`

// good
const MyContainer = styled.div`
  color: blue;
`
```

- Group your shorthand properties at the end of your object declaration.

```jsx
const anakinSkywalker = 'Anakin Skywalker'
const lukeSkywalker = 'Luke Skywalker'

// bad
const obj = {
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
}

// good
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
  lukeSkywalker
}
```

- Use function declarations instead of function expressions.
  - Why? This improves readability.
  - An exception to this is arrow functions, which must be assigned to
    variables.

```jsx
// bad
const foo = function() {
  // ...
}

// good
function foo() {
  // ...
}
```

- Never declare functions inside blocks. This might work but is invalid JS.

```jsx
// bad
if (currentUser) {
  function test() {
    console.log('Nope.')
  }
}

// good
let test
if (currentUser) {
  test = () => {
    console.log('Yup.')
  }
}
```

- Always put default parameters last.

```jsx
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

- You may optionally uppercase a constant only if it (1) is a const and (2) the
  programmer can trust it (and its nested properties) to never change.
  - Why? This is an additional tool to assist in situations where the
    programmer would be unsure if a variable might ever change.
    UPPERCASE_VARIABLES are letting the programmer know that they can trust
    the variable (and its properties) not to change.

```jsx
// bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased'

// bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables'

// allowed but does not supply semantic value
export const apiKey = 'SOMEKEY'

// better in most cases
export const API_KEY = 'SOMEKEY'
```

## Filenames

- Store source code in a subdirectory called `/src/`.
- Store components in a subdirectory called `/src/components/`.
- A base filename should exactly match the name of its default export if one is
  present.
- Use PascalCase for files that export a component, class, or constructor.
- Use camelCase for files that export functions, variables, instances, etc.
- Use the `.stories.js` and `test.js` extensions for stories and tests where
  applicable.

```jsx
// bad
./MyComponent.js
./test/MyComponent.js
./stories/MyComponent.js

// good
./MyComponent.js
./MyComponent.stories.js
./MyComponent.test.js

// good
./useMyHook.js
./useMyHook.test.js
```

## Modules

- Always use modules (import/export) over a non-standard module system.
- In modules with a single export, prefer default export over named export.
- Put all imports above non-import statements.

## Metadetails

- Provide a README that lists all environment variables and their descriptions.
- Private projects should have `UNLICENSED` and `private` in their
  `package.json`.
- Public projects should have `MIT` in their `package.json` with the license in
  `LICENSE.txt`.
- Projects should specify an `engine` field with the minimum required node
  version.
  - You can use the version you started the project with.

## React

- We initialize our web applications with create-react-app.
    - Next.js and Gatsby are also acceptable.
- Use hooks instead of other state management.

## Component Driven Development

- Practice CDD with Storybook in React applications.
- All component stories should have a `Default` state.
- Put spaces in story component names for consistency with story parsing.

```graphql
// bad
export default {
  title: 'pages/BrowsePage',
}

// good
export default {
  title: 'pages/Browse Page'
}
```

## GraphQL

- Write root-level Mutations and Queries verb-first.

```graphql
// bad
Mutation {
  userCreate
  address
}

// bad
Query {
  comments
  comment
}

// good
Mutation {
  createUser
  likePost
  updateComment
}

// good
Query {
  getComments
  getUser
}
```

- Field names should use camelCase.
- Type names should use PascalCase.
- Enum values should be ALL_CAPS and enum names should use PascalCase.

## GitHub

- Use separate repositories for frontend and backend applications.
  - e.g.: repo-web, repo-mobile, and repo-api
- Projects should follow Git flow, and have both a **main** and **dev**
  branch.
  - Open source apps or small projects can use a single **main** branch.
- Both **dev** and **main** should be protected branches.
  - Status checks must pass before merging.
  - Require branches are up-to-date.

## CI / CD

- Use dependabot for dependency management.
- Configure CI / CD using Semaphore 2.0 or Github Actions.

## Testing

- Write tests.
- Use Jest for unit / integration tests.
- Use Cypress for e2e tests.
- Follow the [testing trophy] methodology.

[AirBNB]: https://github.com/airbnb/javascript
[jest-runner-eslint]: https://www.npmjs.com/package/jest-runner-eslint
[is-ci-cli]: https://www.npmjs.com/package/is-ci-cli
[testing trophy]: https://kentcdodds.com/blog/write-tests
