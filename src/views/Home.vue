<template>
  <div class="home">
    <TableData />
  </div>
</template>

<script>
// @ is an alias to /src
import TableData from "@/components/TableData";

export default {
  name: "Home",
  components: {
    TableData,
  },
};
</script>
