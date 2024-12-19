<script setup>
import { duck } from "../init-duck.mjs";
import { ref } from "vue";

const result = ref("");

const queryTx = async () => {
    const c = await duck.connect();

    //     const query = await c.query(`
    //     SELECT * FROM 'columns.json';
    // `);
    const query = await c.query(`
      CREATE TABLE IF NOT EXISTS columns (
        col1 INTEGER,
        col2 TEXT
      );
      INSERT INTO columns (col1, col2) VALUES (1, 'foo');
      INSERT INTO columns (col1, col2) VALUES (2, 'bar');
      INSERT INTO columns (col1, col2) VALUES (3, 'baz');
      SELECT * FROM columns;
    `);
    const resultJSON = query.toArray().map((row) => {
        const columns = Object.entries(row.toJSON()).map(([key, col]) => {
            console.log(key);
            return col;
        });
        return columns;
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
