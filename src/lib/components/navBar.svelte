<script>
  import { page } from '$app/stores';
  import { Sidebar, SidebarGroup, SidebarItem, SidebarWrapper, Badge } from 'flowbite-svelte';
  import { selectedList } from '$lib/stores';

  let spanClass = 'flex-1 ml-3 whitespace-nowrap';

  let inputValue = "";

  $: activeUrl = $page.url.pathname
</script>

<Sidebar>
  <SidebarWrapper>
    <input type="text" bind:value={inputValue}>
    <button on:click={() => {
      if (selectedList) {
        $selectedList.push(inputValue);
        $selectedList = $selectedList;
        inputValue = "";
      }
    }}>btn</button>
    <SidebarGroup>
      <div id="sidebarBtnGroup">
        {#each $selectedList as btnTag}
          <Badge dismissable large color='red'>{btnTag}</Badge>
        {/each}
      </div>
    </SidebarGroup>
  </SidebarWrapper>
</Sidebar>


<style>
  #sidebarBtnGroup {
    height: calc(100vh - 100px);
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
</style>