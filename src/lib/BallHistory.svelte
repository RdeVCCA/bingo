<script>
  let { unpicked } = $props();
</script>

<style>
  table {
    border-collapse: collapse;
    table-layout: fixed;
    width: 15%;
  }
  
  table * {
    border: 2px solid black;
    text-align: center;
  }

  th {
    font-weight: bold;
    background-color: #eee;
  }

</style>

<table>
  <thead>
    <tr>
      <th>B</th>
      <th>I</th>
      <th>N</th>
      <th>G</th>
      <th>O</th>
    </tr>
  </thead>
  <tbody>
    <!--
      Bingo balls each have unique numbers from 1-75.
      1-15 are labelled B
      16-30 are I
      31-45 are N
      46-60 are G
      61-75 are O
      thus we just need a bit of math to see which cell to check.
    -->
    {#each Array(15).fill() as _, i}
      <!-- 15 rows -->
      <tr>
        {#each Array(5).fill() as _, j}
          <!-- 5 columns -->
          <td>
            {#if !unpicked.includes(j * 15 + i + 1)}
              {j * 15 + i + 1}
            {:else}
              <!-- HACK: not sure why the table columns are shrinking so fill empty cells with &emsp to pad them up -->
              &emsp;
            {/if}
          </td>
        {/each}
      </tr>
    {/each}
  </tbody>
</table>
