<script lang="ts">
	import { page } from '$app/stores'
	import { db } from "../../../db"
	import Event from "../../../components/Event.svelte"
	import type { IEvent } from "../../../db"
  import { onMount } from 'svelte'
  import { liveQuery } from "dexie"
	import type { IProfile } from "../../../db"


  let id = ''
  let event:IEvent|undefined
  let profile:IProfile|undefined
  
  onMount(async () => {
    id = $page.params.id
    event = await db.events.get(id)

    profile = await db.profiles.get(event?.pubkey)

    
  })
</script>

<svelte:head>
  <title>Event</title>
</svelte:head>

<h1>Event {id}</h1>

{#if event}
<Event {event} name={profile?.name} />
{:else}
Event not found.
{/if}
