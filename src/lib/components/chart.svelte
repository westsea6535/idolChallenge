<script>
  import { afterUpdate } from 'svelte';
  import Chart from 'svelte-frappe-charts';

  export let statisticsData;

  let data;

  afterUpdate(() => {
    console.log('on component');
    console.log(statisticsData);

    data = {
      labels: new Array(50).fill(`video`),
      datasets: [
        {
          values: statisticsData.items.map((e, i) => e.statistics.viewCount)
        }
      ]
    }
    console.log(data);
  })

</script>

<div id="chartWrap">
  {#if statisticsData}
    <Chart data={data} type="line" />
  {/if}
</div>

<style>
  #chartWrap {
    width: 50vw;
  }
</style>