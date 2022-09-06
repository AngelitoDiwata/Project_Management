<template>
  <div class="ml-20 m-auto w-full h-screen pr-20 flex flex-row items-start justify-between space-x-5">
    <div class="w-2/3 flex flex-col items-start justify-between space-y-5">
      <data-table
        @data-get="setDataFields"
        class="w-full overflow-scroll"
        style="max-height: 200px"
        :query-string="realtimeValue || editorValue"
      />
      <div
        ref="edit"
        id="editor"
        class="w-full m-auto"
        style="height: 500px"
      ></div>
    </div>
    <div class="w-1/3 ml-10 h-screen flex flex-col items-start justify-start space-y-5">
      <h1>Query Properties</h1>
      <div class="w-3/4 p-3 overflow-scroll border border-dashed" style="max-height: 200px">
        Dataset Fields:
        <el-tag
          class="m-2 font-bold"
          :key="datafield.key"
          v-for="datafield in dataFields"
        >
          {{ datafield }}
        </el-tag>
      </div>
    </div>
  </div>
</template>

<script>
import loader from "@monaco-editor/loader";
import DataTable from "../components/Builder/DataTable.vue";

export default {
  components: { DataTable },
  layout: "Default",
  data() {
    return {
      editor: null,
      editorValue: "",
      dataFields: [],
    };
  },
  computed: {
    realtimeValue() {
      try {
        return this.editor.getValue();
      } catch (e) {
        return e;
      }
    },
  },
  watch: {
    editor(_) {
      this.editorValue = this.editor.getValue();
    },
  },
  methods: {
    setDataFields(value) {
      this.dataFields = value;
    },
  },
  async mounted() {
    loader.init().then((monaco) => {
      const editorOptions = {
        value: "SELECT * FROM t_users;",
        language: "sql",
        roundedSelection: false,
        scrollBeyondLastLine: false,
        readOnly: false,
        theme: "vs-dark",
      };

      this.editor = monaco.editor.create(
        document.getElementById("editor"),
        editorOptions
      );
    });
  },
};
</script>

<style>
</style>