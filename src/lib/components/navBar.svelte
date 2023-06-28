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
    <div id="title">키워드 추가하기</div>
    <input type="text" bind:value={inputValue}>
    <button on:click={() => {
      if (selectedList) {
        $selectedList.push(inputValue);
        $selectedList = $selectedList;
        inputValue = "";
        console.log($selectedList);
      }
    }}>추가하기</button>
    <SidebarGroup>
      <div id="sidebarBtnGroup">
        {#each $selectedList as btnTag}
          <div class="sidebarBtnWrap">
            <div class="sidebarBtn">{btnTag}</div>
            <div class="sidebarBtnRmv">X</div>
          </div>
        {/each}
      </div>
    </SidebarGroup>
  </SidebarWrapper>
</Sidebar>


<style>
  * {
    font-family: Pretendard;
  }
  #title {
    display: flex;
    justify-content: center;
    margin-bottom: 10px;
    font-size: 20px;
  }
  input {
    height: 25px;
  }
  button {
    width: 100%;
    height: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #771D1D;
    color: white;
    border-radius: 10px;
    margin: 10px 0 15px;
  }
  #sidebarBtnGroup {
    height: calc(100vh - 100px);
    display: flex;
    flex-direction: column;
    gap: 15px;
    background-color: white;
    padding: 10px;
  }
  .sidebarBtnWrap {
    display: flex;
    justify-content: space-between;
  }
</style>