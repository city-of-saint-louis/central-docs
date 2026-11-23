# JavaScript Style Guide

## Introduction

This guide is based on the JavaScript in the City of St. Louis
[`lco-integrations-and-automations`](https://github.com/city-of-saint-louis/lco-integrations-and-automations)
and [`api`](https://github.com/city-of-saint-louis/api) repositories. It describes
common practices and recommends a consistent standard where the repositories
differ. Follow the existing style when editing an established module; use the
recommendations below for new code.

## General JavaScript Guidelines

### Use descriptive names

Choose names that explain what a value or function represents. Clear names help
readers understand code without guessing from surrounding context.

```javascript title="Choose names that explain the purpose"
// Incorrect
const x = 1200;
function f(value) {
  return value * x;
}

// Correct
const parcelArea = 1200;
function calculateParcelValue(pricePerSquareFoot) {
  return pricePerSquareFoot * parcelArea;
}
```

In the incorrect example, `x`, `f`, and `value` do not tell the reader what
they represent. The correct names describe the area, the calculation, and its
input.

```javascript title="Name values after the data they contain"
// Incorrect
const data = await get(id);

// Correct
const parcel = await getParcelById(parcelId);
```

In the incorrect example, `data`, `get`, and `id` are too general. The correct
example makes the kind of result and lookup clear.

### Prefer clear, straightforward control flow

Write conditions and branches so readers can follow each decision. Use early
returns and separate checks instead of deeply nested or chained expressions.

```javascript title="Use guard clauses instead of nested conditions"
// Incorrect
function getParcelOwner(parcel) {
  if (parcel) {
    if (parcel.owner) {
      return parcel.owner;
    } else {
      return "Unknown";
    }
  } else {
    return "Missing";
  }
}

// Correct
function getParcelOwner(parcel) {
  if (!parcel) {
    return "Missing";
  }

  if (!parcel.owner) {
    return "Unknown";
  }

  return parcel.owner;
}
```

The incorrect example nests checks, making the possible outcomes harder to
scan. The correct example handles missing cases first, then returns the owner.

```javascript title="Prefer explicit branches to chained ternaries"
// Incorrect
const status = !parcel ? "Missing" : parcel.active ? "Active" : "Inactive";

// Correct
let status;
if (!parcel) {
  status = "Missing";
} else if (parcel.active) {
  status = "Active";
} else {
  status = "Inactive";
}
```

The chained ternary in the incorrect example hides multiple decisions in one
line. The correct example gives each outcome its own readable branch.

### Use the project's configured tools

Both repositories include ESLint and Prettier scripts, but their configurations
and enforcement differ. Run the lint and test commands for the repository you
are changing; do not assume one repository's rules apply to the other.

## Naming Conventions

### Use camelCase for variables and functions

Use `camelCase` for variables, parameters, and function names. Use `PascalCase`
for class names and `UPPER_SNAKE_CASE` for fixed constants shared as
configuration.

```javascript title="Name identifiers consistently"
// Incorrect
const ParcelID = "12345";
function GetParcel() {}
class customError extends Error {}

// Correct
const parcelId = "12345";
function getParcel() {}
class CustomError extends Error {}
```

Preserve the capitalization of fields received from external services when
their names are part of that service's contract.

## Variables and Constants

### Use `const` unless a binding must be reassigned

The repositories mainly use `const`, with `let` for values that change. A few
`var` declarations remain in the integrations code; use `let` or `const`
instead.

```javascript title="Choose the narrowest declaration"
// Incorrect
var previousAttempt = 0;
previousAttempt += 1;

// Correct
let attempt = 0;
attempt += 1;
```

For values that do not change, use `const`:

```javascript title="Keep fixed values constant"
// Incorrect
let requestTimeout = 120000;

// Correct
const REQUEST_TIMEOUT = 120000;
```

Use uppercase names for fixed configuration constants; use `camelCase` for
ordinary local values.

## Functions and Methods

### Use descriptive names and keep each function focused

Name functions for the action or result they represent. Extract repeated or
distinct work into a helper rather than growing a function with unrelated
responsibilities.

```javascript title="Name a function for its purpose"
// Incorrect
function process(value) {
  return value;
}

// Correct
function getParcelById(parcelId) {
  return parcelRepository.findById(parcelId);
}
```

### Use one named-function style consistently within a module

The integrations code and many API services use function declarations; API
controllers also commonly use `const`-assigned arrow functions. For new
standalone helpers, prefer named function declarations and use arrow functions
for callbacks. When editing an existing module, follow its established pattern.

```javascript title="Use arrow functions for callbacks"
// Incorrect
const parcelIdsWithFunctionCallback = parcels.map(function (parcel) {
  return parcel.id;
});

// Correct
const parcelIds = parcels.map((parcel) => parcel.id);
```

## Formatting and Indentation

### Use two spaces, semicolons, and double quotes

These are common in both repositories and are specified in the integrations
repository's Prettier configuration. Keep statements readable and use the
configured formatter where available.

```javascript title="Format statements consistently"
// Incorrect
const unformattedParcel = {id: '12345'}

// Correct
const parcel = { id: "12345" };
```

### Break long expressions across lines

Put each item on its own line when an object, argument list, or expression is
long enough to be difficult to scan.

```javascript title="Wrap long calls"
// Incorrect
async function loadParcelWithoutLineBreaks() {
  const parcelResult = await getParcel(parcelId, includeAssessments, includeImages, includeNeighborhood);
}

// Correct
async function loadParcelWithLineBreaks() {
  const parcelResult = await getParcel(
    parcelId,
    includeAssessments,
    includeImages,
    includeNeighborhood,
  );
}
```

## Objects and Arrays

### Use readable multiline objects and arrays

Use a compact layout for short collections and one item per line for longer
ones. Add a trailing comma in multiline collections.

```javascript title="Lay out collections clearly"
// Incorrect
const longParcel = {
parcelId: "12345", ward: 6, neighborhood: "Example", city: "St. Louis", state: "MO", postalCode: "63101"
};
const longParcelIds = ["12345", "67890", "54321", "09876", "11223", "44556", "77889"];

// Correct
const parcel = {
  parcelId: "12345",
  ward: 6,
  neighborhood: "Example",
  city: "St. Louis",
  state: "MO",
  postalCode: "63101",
};

const parcelIds = [
  "12345",
  "67890",
  "54321",
  "09876",
  "11223",
  "44556",
  "77889",
];
```

### Destructure values when it improves clarity

Destructure commonly used properties rather than repeating long property paths.

```javascript title="Destructure commonly used properties"
// Incorrect
const parcelIdFromParams = req.params.parcelId;
const wardFromParams = req.params.ward;

// Correct
const { parcelId, ward } = req.params;
```

## Conditional Statements and Loops

### Use braces for conditional branches

Braces make branch boundaries clear and reduce mistakes when code changes.
Use guard clauses to handle invalid or exceptional cases early.

```javascript title="Make conditional branches explicit"
// Incorrect
function getParcelOrNullWithoutBraces() {
  if (!parcelId) return null;
}

// Correct
function getParcelOrNullWithBraces() {
  if (!parcelId) {
    return null;
  }
}
```

### Choose the loop that matches the task

Use `for...of` when processing each item in a collection. Use an indexed loop
when the index itself is needed, such as when processing fixed-size batches.

```javascript title="Iterate over collection values"
// Incorrect
for (let index = 0; index < parcelIds.length; index += 1) {
  loadParcel(parcelIds[index]);
}

// Correct
for (const parcelId of parcelIds) {
  loadParcel(parcelId);
}
```

## Asynchronous JavaScript

### Use `async`/`await` for sequential asynchronous work

Both repositories use `async`/`await` for API and database operations. Await a
promise when its result is needed before continuing, and return a promise when
the caller should handle it.

```javascript title="Wait for the asynchronous result"
// Incorrect
async function getParcelWithoutAwait(parcelId) {
  const parcel = parcelRepository.findById(parcelId);
  return parcel.id;
}

// Correct
async function getParcelWithAwait(parcelId) {
  const parcel = await parcelRepository.findById(parcelId);
  return parcel.id;
}
```

### Run independent asynchronous work together

When multiple requests do not depend on one another, use `Promise.all` rather
than waiting for each request in sequence.

```javascript title="Run independent requests in parallel"
// Incorrect
async function loadSequentially() {
  const parcel = await getParcel(parcelId);
  const neighborhood = await getNeighborhood(neighborhoodId);
}

// Correct
async function loadInParallel() {
  const [parcel, neighborhood] = await Promise.all([
    getParcel(parcelId),
    getNeighborhood(neighborhoodId),
  ]);
}
```

## Error Handling

### Do not silently discard errors

Propagate errors to the established error handler unless the current layer can
recover or provide a meaningful fallback. API route handlers commonly forward
errors with `next(error)`.

```javascript title="Forward errors to the handler"
// Incorrect
async function getParcelHandlerWithoutErrorHandling(req, res) {
  try {
    const parcel = await getParcel(req.params.parcelId);
    res.json(parcel);
  } catch (error) {
    res.json(null);
  }
}

// Correct
async function getParcelHandlerWithErrorHandling(req, res, next) {
  try {
    const parcel = await getParcel(req.params.parcelId);
    res.json(parcel);
  } catch (error) {
    next(error);
  }
}
```

Catch an error only when handling it, adding useful context, or translating it
at a layer with that responsibility. Avoid logging and rethrowing the same
error at multiple layers.

## Imports and Exports

### Use the existing CommonJS module pattern

The application code in both reviewed repositories uses CommonJS
(`require` and `module.exports`). Keep a module's imports and exports consistent
with that pattern.

```javascript title="Use CommonJS consistently"
// Incorrect
async function loadParcelFromESModule() {
  const { getParcel } = await import("./parcels.js");
  return getParcel;
}

// Correct
const { getParcel } = require("./parcels");

module.exports = { getParcel };
```

Destructure named exports when importing only a few functions, and export only
the functions or values that other modules need.

## Comments and Documentation

### Explain intent, not the obvious

Comments should capture non-obvious reasoning, constraints, or side effects.
Use JSDoc when a public or complex function needs its parameters or return value
documented.

```javascript title="Write comments that add context"
// Incorrect
// Increment retryCount.
retryCount += 1;

// Correct
// This count limits retries after a transient Cityworks response.
retryCount += 1;
```

## Logging and Debugging

### Use the repository's logger for operational messages

The API uses a Winston-backed logger; the integrations code uses Winston and
also prints some command-line status messages. Use the existing logger for
application errors and structured context. Keep `console` output for
intentional command-line output, not temporary debugging.

```javascript title="Log useful error context"
// Incorrect
console.log(error);

// Correct
logger.error("Failed to load parcel", { parcelId, error });
```

## Null, Undefined, and Optional Values

### Use optional chaining and nullish coalescing for optional values

Both repositories use optional chaining. Prefer `??` when the fallback should
apply only to `null` or `undefined`; `||` also replaces valid values such as
`0` and `""`.

```javascript title="Preserve valid falsy values"
// Incorrect
const pageSizeWithOr = requestedPageSize || 10;

// Correct
const pageSizeWithNullishFallback = requestedPageSize ?? 10;
```

Check optional properties before using them:

```javascript title="Safely access nested optional properties"
// Incorrect
const statusWithoutOptionalChecks = response.data.Value.Status;

// Correct
const statusWithOptionalChecks = response.data?.Value?.Status;
```

Use explicit `null` or `undefined` checks when the distinction matters, and do
not use a fallback that hides an unexpected missing value.

## Code Reusability and Maintainability

### Keep responsibilities in the appropriate module

The API separates routes, controllers, services, repositories, and utilities.
Keep request handling, business rules, and data access in focused modules;
share a helper when the same logic is needed in more than one place.

```javascript title="Separate data access from request handling"
// Incorrect
async function getParcelHandlerWithInlineQuery(req, res, next) {
  try {
    const parcel = await citydataDb.query(
      "SELECT * FROM parcels WHERE parcel_id = ?",
      { replacements: [req.params.parcelId] },
    );
    res.json(parcel);
  } catch (error) {
    next(error);
  }
}

// Correct
async function getParcelHandlerWithRepository(req, res, next) {
  try {
    const parcel = await parcelRepository.getById(req.params.parcelId);
    res.json(parcel);
  } catch (error) {
    next(error);
  }
}
```

### Test observable behavior

Both repositories use Jest. Add or update focused tests for behavior changed,
including error cases and meaningful edge cases.

```javascript title="Assert the behavior of an asynchronous operation"
// Incorrect
it("gets a parcel", async () => {
  await getParcel("12345");
});

// Correct
it("returns the parcel with the requested ID", async () => {
  await expect(getParcel("12345")).resolves.toMatchObject({
    parcelId: "12345",
  });
});
```

## Repository Review and Team Confirmation

### Summary of the review

- `lco-integrations-and-automations`: reviewed the runtime modules
  `app.js` and `gchat.js`, the Jest tests, and the package, ESLint, Prettier,
  and EditorConfig configuration.
- `api`: reviewed `app.js`, `server.js`, modules under `src/` (including
  controllers, routes, services, repositories, middleware, utilities, and
  configuration), Jest tests, and package and lint configuration.
- Both repositories use CommonJS, Jest, two-space indentation, semicolons,
  double-quoted strings, and async functions. The API also uses centralized
  error handling and a Winston logger.

### Conventions to confirm with the team

- **Function declarations:** the integrations code and many API services use
  declarations, while API controllers often use `const`-assigned arrow
  functions. This guide recommends declarations for new standalone functions
  and arrows for callbacks; confirm whether to standardize existing modules.
- **Formatting and lint enforcement:** the integrations repository has an
  explicit Prettier configuration and `format:check` script. The API has
  formatter and linter scripts but different ESLint settings and no checked-in
  Prettier configuration was found during this review. Confirm whether to align
  these settings.
- **Logging:** the API favors its Winston service, while the integrations
  application has some intentional console output. Confirm the expected
  approach for new command-line and application logging.
- **File names:** the API contains kebab-case, camelCase, and PascalCase
  filenames. Confirm a preferred naming convention for new files.

## API Integrations and HTTP Requests

Both repositories use Axios for HTTP integrations, including Cityworks and GIS
services. Keep request parameters and headers in the Axios configuration, set
a finite timeout for new requests, and let failures reach the appropriate
error-handling layer.

```javascript title="Pass request data through Axios options"
// Incorrect
const response = await axios.get(`${baseUrl}/parcels?parcelId=${parcelId}`);

// Correct
const response = await axios.get(`${baseUrl}/parcels`, {
  params: { parcelId },
  timeout: 10000,
});
```

For authenticated requests, use the authorization header or configured client
pattern already used by the integration. Do not put credentials or tokens in
URLs.

## Security and Sensitive Data

The repositories read credentials and tokens from environment variables, and
their `.gitignore` files exclude local environment files. Keep secrets out of
source code, committed environment files, error messages, and logs. Log
non-sensitive context that helps diagnose a failure instead.

```javascript title="Do not log credentials"
// Incorrect
logger.error("Authentication failed", { password, token });

// Correct
logger.error("Authentication failed", { service: "Cityworks" });
```

## Configuration and Environment Variables

Both repositories use `process.env`; the integrations repository also provides
an `.env.example` and checks environment-variable coverage in its tests. Read
configuration from the environment rather than embedding deployment-specific
URLs or credentials. Validate required values before using them, and parse
numeric values once at the configuration boundary.

```javascript title="Validate required configuration"
// Incorrect
const baseURL = process.env.CITYWORKS_BASE_URL || "https://production.example";

// Correct
const baseURL = process.env.CITYWORKS_BASE_URL;
if (!baseURL) {
  throw new Error("CITYWORKS_BASE_URL is not defined");
}
```

## Testing and Test Automation

Both repositories use Jest. API endpoint tests use Supertest, and integration
tests mock Axios and other external dependencies. Keep tests deterministic by
mocking network calls, and verify both successful responses and failure
behavior.

```javascript title="Test an HTTP integration with a mock"
// Incorrect
it("loads a parcel", async () => {
  const parcel = await getParcel("12345");
  expect(parcel.parcelId).toBe("12345");
});

// Correct
it("returns the parcel from the HTTP response", async () => {
  axios.get.mockResolvedValueOnce({
    data: { parcelId: "12345" },
  });

  await expect(getParcel("12345")).resolves.toEqual({
    parcelId: "12345",
  });
  expect(axios.get).toHaveBeenCalledTimes(1);
});
```
