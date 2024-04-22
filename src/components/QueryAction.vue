<script setup>
import { duck } from "../init-duck";
import { ref } from 'vue'

const result = ref("");

const queryTx = async () => {
  const c = await duck.connect();

  const query = await c.query(`
    SELECT * FROM 'columns.json'
`);
  const resultJSON = query.toArray().map((row) => {
    const columns = Object.entries(row.toJSON()).map(([, col]) => {
      console.log(col)
      return col.toJSON()
    })
    return columns
  });
  result.value = resultJSON.toString();

  await c.close();
};
</script>
<template>
  <button @click="queryTx">Query <code>columns.json</code></button>
  <pre>
    <code>{{ result }}</code>
  </pre>
</template>
