<script setup>
import { duck } from "../init-duck";
const insertTx = async () => {
  const c = await duck.connect();

  const jsonColContent = {
    col1: [1, 2],
    col2: ["foo", "bar"],
  };
  await duck.registerFileText("columns.json", JSON.stringify(jsonColContent));
  const error = await c.insertJSONFromPath("columns.json", { name: "columns" });

  console.log(error)

  await c.close();
};
</script>
<template>
  <button @click="insertTx">Insert example data</button>
</template>
