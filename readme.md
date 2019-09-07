# Plant Browser Demo

This is a single-file demo web application. Its' purpose is to show how to develop fully functional web server with test coverage without Node.js using only browser and code editor.

## Usage

1. Save [page](doc/index.html) on disk.
2. Open it in code editor.
3. Update `#Server` and `#Spec` scripts code.
4. Refresh the page. DevTools console should contain complete output of your test.

## Dependencies

* [Plant](https://github.com/rumkin/plant).
* [TestUp](https://github.com/rumkin/testup).
* [TinyAssert](https://github.com/rumkin/tiny-assert).


## Structure overview

Code of the example is structured into several scripts. Each
script contains code related to specific task.

```html
<script id="Server"></script>
<script id="Spec"></script>
<script id="TestupHelpers"></script>
<script id="PlantHelpers"></script>
<script id="Main"></script>
```

Where:

* `#Server`. Your http API code.
* `#Spec`. Your tests for the API.
* `#TestupHelpers`. Definition of custom TestUp reporter. required by this script.
* `#PlantHelpers`. Definition for running Plant requests in browser.
* `#Main`. UI bindings and code which is tieing up all previous scripts and run app tests.

## Server with comments

Server code is located in script `#Server` and looks like this:

```js
function createServer() {
  // Instantiate Plant server
  const plant = new Plant()

  // Create API router. It's not necessary, but
  // extremely useful for modular API creation.
  const router = new PlantRouter()

  // Define simple hello world route
  router.get('/hello-world', ({res}) => {
    res.body = 'Hello, World!'
  })

  // Mount router. Pay attention to asterisk in the route
  // without it router will not be reachable.
  plant.use('/api/v1/*', router)

  // Return configured server
  return plant
}
```

## Spec with comments

Tests code is located in script `#Spec` and looks like this:

```js
// describe and it are TestUp's framework methods
// which builds test scenario
function createSpec({describe, it}) {
  describe('Server', () => {
    describe('Router', () => {

      // Define test:
      // Require `request` from test scope.
      it('Should handle request', async ({request}) => {
        // Send request to server, using `request`.
        const res = await request('/api/v1/hello-world')

        // Validate response with TinyAssert accessible globally as `assert`.
        assert.equal(res.body, 'Hello, World', 'expect res.body "Hello, World!"')
      })
    })
  })
}
```
