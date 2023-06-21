<script>
    import { onMount } from "svelte";

  const apiKey = 'AIzaSyB7hiJ2o-nO0m8C3npBkVfH40p1yhbYgZA';
  let videoData;

  const fetchData = async () => {
    const url = new URL('https://www.googleapis.com/youtube/v3/search');
    const params = {
      part: 'snippet',
      // q: encodeURIComponent('queencardchallenge'),
      // q: encodeURIComponent('teddybear_challenge'),
      // q: encodeURIComponent('#love_me_like_this_challenge'),
      q: 'love_me_like_this_challenge',
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

    console.log(url);

    const response = await fetch(url.toString());
    const data = await response.json();

    console.log(data)

    return data;
  }

  onMount( async () => {
    videoData = await fetchData();
    console.log(videoData);
  })

</script>

<div id="wrap">
  {#if videoData}
    {#each videoData?.items as videoData}
      <div class="test">
        <a href={`https://www.youtube.com/watch?v=${videoData.id.videoId}`} target="_blank">
          <img src={videoData.snippet.thumbnails.high.url} alt="">
        </a>
        {videoData.snippet.title}
      </div>
    {/each}
  {/if}
</div>

<style>
  #wrap {
    display: flex;
    flex-direction: column;
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
</style>

