<script>
	import { setClient, operationStore, query } from '@urql/svelte';
	import client from '../client';

	setClient(client);

	const speakers = operationStore(
		`
    query {
      speakers {
        data {
          _id
          firstname
          lastname
          bio
        }
      }
    }
    `
	);

	query(speakers);
</script>

<h1>All Speakers</h1>

{#if $speakers.fetching}
	<p>Loading...</p>
{:else if $speakers.error}
	<p>{$speakers.error.message}</p>
{:else}
	<ul>
		{#each $speakers.data.speakers.data as speaker}
			<li>
				<h2>{speaker.firstname} {speaker.lastname}</h2>
			</li>
		{/each}
	</ul>
{/if}
