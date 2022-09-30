<script lang="ts">
	import { page } from '$app/stores'
	import { db } from "../../db"
	import type { IProfile } from "../../db"
    import { liveQuery } from "dexie"
	import Event from '../../components/Event.svelte'
	import { onMount } from "svelte";

	const numEventsToLoad = 500;
	const numEventsToShowAtStart = 25;
	const numEventsToLoadMore = 25;
	let	  numEventsToPrint = numEventsToShowAtStart;

	const pubkey = $page.params.pubkey
	let p = liveQuery(() => db.profiles.get(pubkey))
    $: profile = $p as IProfile

	console.time("Time taken to load events for pubkey " + pubkey.slice(0, 4));

	let events = liveQuery(() => db.events
		.orderBy('created_at').reverse()
		.filter((it) => it.pubkey == pubkey && it.kind == 1)
		.limit(numEventsToLoad)
		.toArray());

	console.timeEnd("Time taken to load events for pubkey " + pubkey.slice(0, 4));
  
</script>

<svelte:head>
  <title>Profile</title>
</svelte:head>

<h1>A profile and its stuff ...</h1>
<ul>
<li><strong>Name: </strong>{profile?.name}</li>
<li><strong>Public Key: </strong>{profile?.pubkey}</li>
<li><strong>Private Key: </strong> {#if profile?.privkey}yes{:else}no{/if}</li>
<li><strong>Avatar: </strong>{#if profile?.avatar}<img src={profile?.avatar} alt='The Avatar'>{:else}no{/if}  </li>
</ul>

{#if $events instanceof Array }
{#each $events as event, i}
	{#if i < numEventsToPrint }
		<Event  {event} name={profile?.name}/>
	{/if}
{/each}

{/if}

{#if $events instanceof Array }

{#if numEventsToPrint < $events.length}
 <button
	on:click={() => numEventsToPrint = numEventsToPrint + numEventsToLoadMore}
    id="loadmore"
    type="button"
    class="btn btn-secondary">
    Show 25 more posts
  </button>
{/if}
{/if}


{#if $events instanceof Array }
{#if numEventsToPrint < $events.length}
Showing {numEventsToPrint} of {$events.length} events.
{:else}
Showing {$events.length} events.
{/if}
{/if}

<style>
img {
  width: 200px;
  height: auto;
}
</style>