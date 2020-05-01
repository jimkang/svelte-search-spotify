<script>
import throttle from 'lodash.throttle';
import request from 'basic-browser-request';

const baseSearchURL = 'https://api.spotify.com/v1/search/?';

export let spotifyToken;
export let handleError;
export let targetEntity;
export let onResultClick;

let searchTerm;
$: searchResults = [];

var activeRequest;

function onUpdateSearchTerm() {
  if (activeRequest) {
    activeRequest.cancelRequest();
  }
  var opts = {
    method: 'GET',
    mimeType: 'application/json',
    url: `${baseSearchURL}q=${encodeURIComponent(
      searchTerm
    )}&type=track&limit=10`,
    headers: {
      authorization: 'Bearer ' + spotifyToken
    }
  };
  activeRequest = request(opts, processSearchResults);
}

var throttledOnUpdateSearchTerm = throttle(onUpdateSearchTerm, 400);

function processSearchResults(error, response, result) {
  const targetEntityPlural = targetEntity + 's';
  if (error) {
    handleError(error);
  } else if (!response || !result) {
    //   console.log('Search request cancelled.');
  } else if (!result || (!result[targetEntityPlural])) {
    // TODO: Message
  } else {
    // We only care about the first page of results.
    searchResults = result[targetEntityPlural].items;
  }
}

</script>

<div class="search-label">Search for a {targetEntity} on Spotify:</div>
<input type="text" class="search-field" bind:value={searchTerm} on:keyup={throttledOnUpdateSearchTerm}>

<ul class="search-result-list">
{#each searchResults as result }
  <li class="search-result" on:click={onResultClick(result)}>{result.name} by {result.artists.map(artist => artist.name).join(', ')}</li>
{/each}
</ul>

<style>
.search-result {
  cursor: pointer;
}
</style>
