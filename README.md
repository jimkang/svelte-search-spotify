# svelte-search-spotify

A thing to put in your Svelte app to allow users to search via the Spotify API.

Note: Only tested for tracks so far.

## Installation

    npm install @jimkang/svelete-search-spotify

## Usage

    <script>
    import SearchSpotify from 'svelte-search-spotify';

    let error;

    function setError(anError) {
      error = anError;
    }

    function onResultClick(result) {
      console.log('The user picked', result);
      // Do something with the result.
    }

    // Do stuff to get yourself a token (https://developer.spotify.com/documentation/general/guides/authorization-guide/)
    </script>

    <div>
      <SearchSpotify spotifyToken={spotifyToken} handleError={setError} targetEntity={'track'} onResultClick={onResultClick} />

      {#if error}
      <div class="error-message">
        <div>{error.message}</div>
        <pre>{error.stack}</pre>
      </div>
      {/if}
    </div>

## Properties

- `spotifyToken`: [A token from the Spotify API](https://developer.spotify.com/documentation/general/guides/authorization-guide/)
- `setError`: A callback that takes an error as an argument. Called if there is an error while searching.
- `targetEntity`: `track`, `album`. Theoretically, any of the entities accepted by the [Spotify /v1/search endpoint](https://developer.spotify.com/documentation/web-api/reference/search/search/), though I've only tried track and album so far.
- `onResultClick`: An event handler that will be passed a result when the user clicks its representative in the DOM. Usually an object that has an `id`, `uri`, and many other properties. See the Response Format in the [search endpoint documentation](https://developer.spotify.com/documentation/web-api/reference/search/search/) and look at the elements in the `items` arrays in the response examples.

## License

The MIT License (MIT)

Copyright (c) 2020 Jim Kang

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the 'Software'), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
