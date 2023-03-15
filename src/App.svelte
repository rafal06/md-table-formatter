<script lang="ts">
  let input = '';
  let output = '';

  function formatTable() {
    output = '';
    const inputRows = input.split('\n');

    /// Array of rows
    const tableArr: string[][] = [];
    inputRows.forEach(row => {
      if (row.trim() == '') return;
      const trimmedRow = row.trim().replace(/^\|+/, '').replace(/\|+$/, '');
      tableArr.push(trimmedRow.split('|'));
    })

    // console.table(tableArr);

    /// Array of collumns
    let formattedArr: string[][] = [];
    for (const colIndex in tableArr[0]) {
      let col: string[] = [];
      tableArr.forEach(row => {
        col.push(row[colIndex]);
      })
      // console.table(col);

      const longest = col.reduce((a, b) => {
        if (b == undefined) b = '';
        return a.length > b.length ? a : b;
      }, '').length;
      // console.log(longest);

      let formattedCol: string[] = [];
      col.forEach(str => {
        if (str == undefined) str = '';
        formattedCol.push(str + ' '.repeat(longest - str.length));
      });
      // console.table(formattedCol);
      formattedArr.push(formattedCol);
    }

    // console.table(formattedArr);
    for (const rowIndex in formattedArr[0]) {
      formattedArr.forEach(col => {
        output += '|' + col[rowIndex];
      })
      output += '|\n';
    }
  }
</script>

<main>
  <h1>Markdown table formatter</h1>
  <div class="flex">
    <div>
        <h2>Input</h2>
        <textarea bind:value={input} on:input={formatTable}></textarea>
    </div>
    <div>
        <h2>Output</h2>
        <textarea bind:value={output} disabled></textarea>
    </div>
  </div>
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  h1 {
    font-size: 2rem;
  }
  h2 {
    font-size: 1.3rem;
  }

  .flex {
    display: flex;
  }
  /* .flex > div {
    flex-shrink: 0;
  } */
  .flex textarea {
    height: 70vh;
    width: 40vw;
    padding: 10px;
    /* border-radius: 5px;
    border: 1px solid gray; */
  }

  .flex textarea:disabled {
    color: black;
    overflow: scroll;
  }

  @media (prefers-color-scheme: dark) {
    .flex textarea:disabled {
      color: white;
    }
  }
</style>
