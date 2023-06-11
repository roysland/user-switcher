<script>
    import { onMount } from "svelte";
    import UserList from "./UserList.svelte";

    let ready = false;

  onMount(() => {
    const existing = window.localStorage.getItem("users");
    if (!existing) {
      window.localStorage.setItem("users", JSON.stringify([]));
    }
    window.localStorage.removeItem("currentUser");
    ready = true;
  });
</script>

<main>
  {#if ready}
  <UserList></UserList>
  {/if}
</main>
