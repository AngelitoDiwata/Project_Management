<template>
  <el-table :key="tableKey" :data="tableArray" stripe>
    <el-table-column
      v-for="field in fields"
      :key="field.key"
      :prop="field.toLowerCase()"
      :label="field"
    >
    </el-table-column>
  </el-table>
</template>

<script>
export default {
  props: {
    queryString: {
      type: String,
      default: 'SELECT * FROM t_users',
    },
    tableKey: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      tableArray: [{ data: "" }]
    };
  },
  watch: {
    async queryString(value) {
      if (value.includes(';')) {
        this.tableArray = await this.$store.dispatch('customQuery', {query: value})
        this.$emit('data-get', this.fields)
      }
    }
  },
  computed: {
    fields() {
      try {
      return Object.keys(this.tableArray[0]);
      } catch (e) {
        return ['data']
      }
    },
  },
};
</script>

<style>
</style>