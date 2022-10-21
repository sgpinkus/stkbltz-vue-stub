<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  data(): any { return {
      x: [
        { _id: "1", value: "A" },
        { _id: "2", value: "B" },
        { _id: "3", value: "C" }
      ],
    }
  }
});
</script>

<template>
  <main>
    <table>
      <tr v-for='row in x' :key="row._id">
        <template v-for='(v, k) in row' :key="v">
          <td v-if='(k as unknown) !== "_id"'>{{ v }}</td>
        </template>
      </tr>
    </table>
  </main>
</template>

<style scoped>
  table, td { padding: 1em; border: dashed salmon }
</style>
