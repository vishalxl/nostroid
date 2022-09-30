<script type="ts">
  import { goto } from '$app/navigation'
  import { base } from '$app/paths'
  import type { IEvent } from "../db"
  import TagList from './TagList.svelte'
  
  export let event: IEvent
  export let name = "unknown"

  const select = (id: string) => {
    goto(`${base}/event/${id}`)
  }
</script>

<div class="container">
  <div class="comment__container opened" id="first-comment" >
    <div class="comment__card" on:click={() => select(event.id)}>
      <p><b> {name} </b>
      </p>
      <p>
        {event?.content}
      </p>
      <div class="comment__card-footer">

        <div>Likes 123</div>
        <div class="show-replies">Reply 2</div>
      </div>
    </div>
  </div>
</div>


<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 16px;
  min-height: 100%;
}

body {
  min-height: 100vh;
}

.container {
  width: min(90%, 1140px);
  margin: 3rem auto;
}

.comment__container {
  display: none;
  position: relative;
}

.comment__container.opened {
  display: block;
}

.comment__container::before {
  content: "";
  background-color: rgb(170, 170, 170);
  position: absolute;
  min-height: 100%;
  width: 1px;
  left: -10px;
}

.comment__container:not(:first-child) {
  margin-left: 3rem;
  margin-top: 1rem;
}

.comment__card {
  padding: 20px;
  background-color: white;
  border: 1px solid rgba(0, 0, 0, 0.3);
  border-radius: 0.5rem;
  min-width: 100%;
  box-shadow: 0 0 50px rgba(0, 0, 0, 0.1);
}

.comment__card h3,
.comment__card p {
  margin-bottom: 1rem;
}

.comment__card-footer {
  display: flex;
  font-size: 0.85rem;
  opacity: 0.6;
  gap: 30px;
  justify-content: flex-end;
  align-items: center;
}

.show-replies {
  cursor: pointer;
}

</style>
