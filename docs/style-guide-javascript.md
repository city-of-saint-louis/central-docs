# JavaScript Style Guide

## Introduction

This guide recommends consistent JavaScript practices. Follow the existing
style when editing an established module; use the recommendations below for
new code.

## General JavaScript Guidelines

### Prefer straightforward control flow

Write branches and decisions so they can be read one step at a time. Keep
guard clauses and geometry or address decisions explicit instead of hiding
multiple checks in deeply nested expressions.

```javascript title="Use guard clauses for missing parcel IDs"
// Incorrect
function parseParcelId(parcelId) {
  if (!parcelId) {
    return null;
  }
  const filteredId = filterNonNumbers(parcelId);
  if (isInvalidNumberString(filteredId)) {
    return null;
  } else {
    return filteredId;
  }
}

// Correct
function parseParcelId(parcelId) {
  if (!parcelId) {
    return null;
  }

  const filteredId = filterNonNumbers(parcelId);
  if (isInvalidNumberString(filteredId)) {
    return null;
  }

  return filteredId;
}
```

The `else` branch is not needed after the invalid-ID guard. Returning directly
in the correct example keeps the success path shorter and easier to follow.

```javascript title="Use an explicit branch for address unit lookup"
// Incorrect
const startUnitSearch =
  suffixIdx !== -1 ? suffixIdx + 1 : typeIdx !== -1 ? typeIdx + 1 : idx;

// Correct
let startUnitSearch = idx;
if (suffixIdx !== -1) {
  startUnitSearch = suffixIdx + 1;
} else if (typeIdx !== -1) {
  startUnitSearch = typeIdx + 1;
}
```

The nested ternary contains two decisions in a compact expression. The explicit
branches show each address fallback in sequence, making the unit lookup easier
to understand and change.

### Use the project's configured tools

Run the project's configured lint and test commands, and follow its formatting
and enforcement rules.

## Naming Conventions

### Use descriptive names

Choose names that describe the data they contain. Descriptive names make
request parameters and helper variables easier to understand without searching
for their surrounding context.

```javascript title="Name request values after the data they contain"
// Incorrect
const p = req.params.parcelId;

// Correct
const { parcelId } = req.params;
```

The `p` name does not explain that the value is a parcel ID. The destructured
name in the correct example matches the API controller parameter and immediately
identifies the data.

```javascript title="Name parcel and neighborhood helpers clearly"
// Incorrect
const d = dateValue instanceof Date ? dateValue : new Date(dateValue);

// Correct
const date = dateValue instanceof Date ? dateValue : new Date(dateValue);
```

The `d` name does not describe the Date value clearly. `date` makes the value
easier to recognize in the same change-reporting code.

```javascript title="Name lookup accumulators clearly"
// Incorrect
if (ward) {
  dict[nhdNum].push(ward);
}

// Correct
if (ward) {
  neighborhoodLookup[nhdNum].push(ward);
}
```

The `dict` accumulator does not state which data it contains.
`neighborhoodLookup` identifies the accumulated object and makes the ward
mapping easier to follow.

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

### Use descriptive names for functions

Name a function for the action or result it represents. A specific name makes
its purpose clear when it is called elsewhere in the code.

```javascript title="Name parcel helpers clearly"
// Correct
function parseParcelId(parcelId) {
  // Filter and validate the parcel ID.
}

// Correct
async function getNeighborhoodName(neighborhoodNumber) {
  // Retrieve the neighborhood name.
}
```

`parseParcelId` clearly describes a parcel ID parsing operation, and
`getNeighborhoodName` identifies both the action and the neighborhood input.

## Variables and Constants

### Use `const` unless a binding must be reassigned

Use `const` for bindings that do not change and `let` for values that must be
reassigned. Do not use `var` in new code.

```javascript title="Choose the narrowest declaration"
// Avoid var in new code. Use const or let instead.
const parcelId = "12345";
let attempt = 0;
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

### Keep each function focused

Give each function one clear responsibility. A function may coordinate multiple
steps toward the same result, but unrelated tasks or independent reasons to
change can indicate that responsibilities should be separated into helpers.


## Objects and Arrays

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

Await a promise when its result is needed before continuing, and return a
promise when the caller should handle it.

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

### Don't mix CommonJS and ES module styles

Use the module style configured for the project consistently. In CommonJS
modules, use `require()` and `module.exports`; dynamic imports may be
appropriate for an ESM-only dependency.

```javascript title="Use CommonJS consistently"
// Incorrect: ES module syntax in a CommonJS project
import { getParcel } from "./parcels.js";

export { getParcel };

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

### Use the configured logger for operational messages

Use the configured logger for errors, warnings, and
operational messages. Include context that helps diagnose the problem, such as
IDs and the operation being performed. Don't leave `console.log` calls in
committed code for debugging. Intentional human-facing output in a command-line
script is an exception, but runtime application errors should use the
configured logger.

```javascript title="Log useful error context"
// Incorrect
console.log(error);

// Correct
logger.error("failed to generate .env.example", {
  error: error.message,
});
```

## Null, Undefined, and Optional Values

### Use optional chaining and nullish coalescing for optional values

Prefer `??` when the fallback should apply only to `null` or `undefined`; `||`
also replaces valid values such as `0` and `""`.

```javascript title="Preserve valid falsy values"
// Incorrect
const pageSizeWithOr = requestedPageSize || 10;

// Correct
const pageSizeWithNullishFallback = requestedPageSize ?? 10;
```

Check optional properties before using them. Optional chaining is appropriate
when an external response may omit an intermediate property. After safely
reading the value, handle `undefined` or `null` explicitly when missing data has
different meanings.

```javascript title="Safely handle an optional Cityworks token"
// Correct
async function getCityworksToken() {
  const result = await axios.post(tokenUrl, data);

  if (result?.data?.Value?.Token) {
    tokenStore.setToken(result.data.Value.Token);
    return;
  }

  throw new CustomError("failed to save token", "TOKEN_STORE_FAILED");
}
```

Optional chaining prevents `result`, `data`, or `Value` from causing an access
error when they are missing. The `if` block handles a missing token as a
meaningful failure. Do not return a fallback token or continue authentication
after this check fails.

```javascript title="Distinguish undefined and null API results"
// Correct
const vocabulary = await fetchData(
  req.apiResponse,
  getVocabById(id),
  ErrorMessages.VOCAB.FETCH_ERROR,
  true,
);

if (vocabulary === undefined) {
  req.apiResponse.setStatus(StatusCodes.INTERNAL_SERVER_ERROR);
} else if (vocabulary === null) {
  throw new CustomError(ErrorMessages.VOCAB.NOT_FOUND, StatusCodes.NOT_FOUND);
} else {
  req.apiResponse.setStatus(StatusCodes.OK);
}
```

Use explicit checks when `undefined` and `null` mean different things. In this
API controller, `undefined` represents a fetch failure, while `null` represents
a valid lookup with no vocabulary found.

## Code Reusability and Maintainability

### Keep responsibilities in the appropriate module

Keep request handling, business rules, and data access in focused modules;
share a helper when the same logic is needed in more than one place.

## API Integrations and HTTP Requests

### Configure HTTP requests explicitly

Keep request parameters and headers in the Axios configuration, set a finite
timeout for new requests, and let failures reach the appropriate error-handling
layer.

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

## Configuration and Environment Variables

### Validate configuration before use

Read configuration from the environment and validate required values before
creating clients.

```javascript title="Validate a required Cityworks base URL"
// Incorrect
const baseUrl = process.env.CITYWORKS_BASE_URL || "https://production.example";

// Correct
const baseUrl = process.env.CITYWORKS_BASE_URL;
if (!baseUrl) {
  throw new CustomError(
    "CITYWORKS_BASE_URL is not defined",
    "CONFIGURATION_ERROR",
  );
}

const axiosInstance = axios.create({
  baseURL: baseUrl,
});
```

The incorrect example silently supplies a production URL. The correct example
uses `CustomError` when `CITYWORKS_BASE_URL` is missing and
passes the validated value to Axios as `baseURL`.

### Validate numeric environment values

Parse numeric environment values once when configuration is created. Validate
the parsed value and its permitted range. Use `??` when a fallback should apply
only to missing values; handle empty strings explicitly when they require
different treatment.

## Testing and Test Automation

### Mock external dependencies

Keep tests deterministic by mocking network calls, and verify both successful
responses and failure behavior.

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

### Test observable behavior

Add or update focused tests for behavior changed, including error cases and
meaningful edge cases.

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
