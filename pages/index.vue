<template>
  <div class="w-11/12 md:w-5/6 m-auto h-full p-1 md:p-10 lg:p-0">
    <div class="w-full py-5">
      <span class="text-5xl font-bold text-white"> Dashboard </span>
    </div>
    <div
      class="
        w-full
        flex flex-col
        lg:flex-row
        items-center
        justify-start
        space-x-0
        md:space-x-3
      "
    >
      <pie-subtask-widget />
      <percentages />
    </div>
    <div class="py-10 w-full m-auto">
      <project-percentage :perc-data="GET_ALL_PLANS" />
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import ChartWidget from "../components/Widgets/ChartWidget.vue";
import ProjectPercentage from "../components/Widgets/ProjectPercentage.vue";
import Percentages from "../components/Widgets/Percentages.vue";
import PieSubtaskWidget from "../components/Widgets/PieSubtaskWidget.vue";
export default {
  components: { ChartWidget, ProjectPercentage, Percentages, PieSubtaskWidget },
  name: "IndexPage",
  layout: "Default",
  computed: {
    ...mapGetters(["GET_SELECTED_PLANS", "GET_ALL_PROJECTS", "GET_ALL_PLANS"]),
  },
  methods: {
    async getProjectPlan(project_id) {
      await this.$store.dispatch("getPlans", project_id);
    },
  },
  async created() {
    await this.$store.dispatch("getProjects");
    await this.$store.dispatch("getPlans");
    await this.$store.dispatch("getPlanDashboard");
  },
  data() {
    return {
      drawer: false,
    };
  },
};
</script>
<style>
@import url("~/assets/output.css");
</style>