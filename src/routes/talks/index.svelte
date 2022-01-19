<script>
	import { setClient, operationStore, query } from '@urql/svelte';
	import client from '../../client';

	setClient(client);

	const talks = operationStore(
		`
    query {
      talks {
        data {
          _id
          description
          title
          video_url
          speaker {
            firstname
          }
        }
      }
    }
    `
	);

	query(talks);
</script>

<h1>All Talks</h1>

{#if $talks.fetching}
	<p>Loading...</p>
{:else if $talks.error}
	<p>{$talks.error.message}</p>
{:else}
	<ul>
		{#each $talks.data.talks.data as talk}
			{console.log(talk)}
			<li>
				<h2>{talk.title}</h2>
				<p>{talk.description}</p>
				<iframe
					title="Integrating FaunaDB with SvelteKit"
					width="420"
					height="315"
					src={`https://www.youtube.com/embed/${talk.video_url}`}
				/>
			</li>
		{/each}
	</ul>
{/if}
