<script>
  import { afterUpdate, onMount } from "svelte";
  import NavBar from "$lib/components/navBar.svelte";
  import Chart from "../lib/components/chart.svelte";
  import { selectedList } from '$lib/stores';

  const apiKey = 'AIzaSyB7hiJ2o-nO0m8C3npBkVfH40p1yhbYgZA';
  let videoData = [];
  let statisticsData;
  $: statisticsData;
  $: $selectedList, (async () => {
      for (let i = 0; i < $selectedList.length; i++) {
        console.log($selectedList[i]);
        videoData[i] = await fetchData($selectedList[i]);
        console.log(videoData[i]);
    }})();

  const fetchStatistics = async (videoId) => {
    const url = new URL('https://www.googleapis.com/youtube/v3/videos');
    const params = {
      part: 'statistics',
      id: videoId,
    };

    for (const key in params) {
      if (params.hasOwnProperty(key)) {
        url.searchParams.append(key, params[key]);
      }
    }
    url.searchParams.append('key', apiKey);

    const response = await fetch(url.toString());
    const data = await response.json();
    return data;
  }

  const fetchData = async (keyword) => {
    const url = new URL('https://www.googleapis.com/youtube/v3/search');
    const params = {
      part: 'snippet',
      q: encodeURIComponent(keyword),
      maxResults: 50,
      type: 'video',
      order: 'viewCount',
      regionCode: 'KR',
    };

    for (const key in params) {
      if (params.hasOwnProperty(key)) {
        url.searchParams.append(key, params[key]);
      }
    }
    url.searchParams.append('key', apiKey);

    const response = await fetch(url.toString());
    const data = await response.json();
    console.log(data);

    const urlJoin = data.items.reduce((accumulator, curr) => `${accumulator}${accumulator ? ',' : ''}${curr.id.videoId}`, '');

    statisticsData = await fetchStatistics(urlJoin);
    return data;
  }

  onMount( async () => {
    for (let i = 0; i < $selectedList.length; i++) {
      console.log($selectedList[i]);
      videoData[i] = await fetchData($selectedList[i]);
      console.log(videoData[i]);
    }
  })
  

</script>

<header on:click={() => console.log(videoData)}>
  <div id="title">
    아이돌 챌린지 유튜브 쇼츠
  </div>
  <div id="name">20210771 소프트웨어학부 김서해</div>
</header>

<div id="wrap">
  <NavBar />

  <Chart {statisticsData}/>
  
  <div id="videoWrap">
    {#if videoData.length}
      {#each videoData as singleKeyData}
        {#each singleKeyData?.items as videoData}
          <div class="test">
            <a href={`https://www.youtube.com/watch?v=${videoData.id.videoId}`} target="_blank">
              <img src={videoData.snippet.thumbnails.high.url} alt="">
            </a>
            {videoData.snippet.title}
          </div>
        {/each}
      {/each}
    {/if}
  </div>
</div>



<style>
  header {
    background-color: #FF6D60;
    height: 100px;
    font-family: Pretendard;
    padding: 10px;
  }
  header #title {
    font-size: 30px;
    font-weight: 800;
    color: #222;
  }
  header #name {
    font-size: 15px;
    font-weight: 600;
    color: white;
  }
  #wrap {
    display: flex;
    gap: 10px;
  }
  .test {
    height: 100px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .test img {
    height: 100px;
  }
  #videoWrap {
    height: calc(100vh - 100px);
    overflow-y: scroll;
  }
</style>

