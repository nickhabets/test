## PR and contribution guidelines

Code that’s easy to review is easy to debug, modify, and extend, so break PRs up into the smallest reasonable chunk possible if the feature is large. If a PR takes 45 minutes or an hour to review, then a reviewer may need an entire uninterrupted block of time to properly review it and this is not always feasible due to meetings and/or competing work priorities

### Review Process

1. Your team reviews and approves
2. If review by a My LZ team member is required (i.e. your changes affect platform code/code used by multiple apps), once your team approves request review from a My LZ team member

### General guidelines

- If not on the My LZ team and you are contributing a PR that affects platform code, please ensure your teammates review your PRs _before_ requesting a review from people on the My LZ team
- Largescale package upgrades are `chore`s that should be separate from feature work, unless they include breaking changes (or very small and localized enhancements with limited scope) that should be included
- When possible, PRs should be able to be merged all the way through to Prod if approved and should not be dependent upon other PRs that are not yet merged
- If a PR _is_ dependent on another PR but _should_ be split out to make reviewing easier, reference the other PR/story in the body of the PR. Put the `Do not merge` tag onto all PRs dependent upon other PRs until the blocking PR(s) is/are merged
- If your PR touches a lot of files simply due to formatting changes via prettier, etc., but only a few logic changes then add comments to point out where the actual changes are or what is just formatting
- Code should be readable. Function/variable names should reflect conventions in the file/project (camelCase, PascalCase, etc., as suitable) and should be declarative – i.e. `getAllItemsById` or `shouldShowOptions`
- Boolean functions must start with "is", e.g. `isPokemon(name: string)`
- Flags must use affirmative language, e.g. `enableRex`. This avoids confusion caused by double-negatives, e.g. `disableRex = false` 🤷
- Code should be easily cognizable
- Use TS! It’s a great tool, so take advantage of generics, type inference, etc., where you can
- Fill in the PR template completely
- No code reviews will be given unless all checkboxes are checked OR an explanation is given as to why something is not checked. For this, “N/A” is acceptable as an explanation where no other explanation is needed
- If viewport size dramatically affects the UX, use multiple images to reflect those differences
- Post PRs in the #my-lz-pr-review channel
- Add screenshots of any UI work from “before” and “after”

### Angular to React migration guidelines

- Add screenshots of the migrated work from Angular as “before” and the post-migration work as “after”
- Note where there are differences and why (i.e. “font slightly different per <whoeverthedesigneris>”)
- Post PRs in the #my-lz-pr-review channel
- For ALL migration related PRs, put the label “React Migration” on the PR
- Functionality should be tested with “@testing-library/react” and apis should be tested with “nock” (if not already tested elsewhere)

#### Performance

Consider performance and in the course of looking at Angular prior to/while working on the migration, consider the following:

- Are we currently over-fetching data (i.e. fetching fields/info we don’t need, or connecting to additional services that provide data we don’t need or don’t immediately need?)
- Is there some data that we could get in a subsequent client-side request that would reduce server latency and comes from a different DB/service than the basic necessary request to begin rendering the page?
- Are there expensive queries (check DataDog) that could and should be cached? If so, consider passing the `x-lz-should-cache` header with a value of “true”
- Also look at to what extent you can share data via `Context` as opposed to just storing it locally
- The dumber the component, the more reusable it is.
  - Favor component composition by creating or utilizing common components in the `ui` package when given the option of creating one-offs or generic components. An example of a dumb component is an Accordion – it doesn’t care what’s in it, it just renders the header and content and expands/collapses when part of it is clicked
  - But also avoid trying to fit a square peg in a round hole. Not every component can be generic and be wary of adding more and more props/options to one prop in order to allow it to be used in many ways. If this is occurring, you may want to bring in senior/staff devs and UX to discuss
- For text, use the typography classes we have in tailwind
