<template>
  <base-chart-pie class="flex-1" ref="chart" @reload="reloadChart"/>
</template>

<script>
import baseChartPie from "../../base/base-chart-pie";
import { transObjFromArray } from "@/utils/dragReport";
import {
  eventProcessingReportPieOption,
  eventDealName
} from "../../js/variable";

export default {
  name: "custom-report-component-chart-event-processing-report",
  props: {
    reportData: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  watch: {
    reportData: {
      handler(newVal, oldVal) {
        // 父组件数据更新触发 echart 更新
        this.renderChart(newVal);
      }
    }
  },
  components: { baseChartPie },
  methods: {
    // 开发模式下，热加载组件触发 echart 更新
    reloadChart() {
      let { chart } = this.$refs;
      chart.setOption(eventProcessingReportPieOption);
      this.reportData && this.renderChart(this.reportData);
    },
    // 虽然这个地方的配置也能设置图表的 title、legend 等其他信息，但是建议将这些信息提取到 variable 中方便统一管理
    renderChart(option) {
      let { chart } = this.$refs;
      chart.setOption({
        series: { data: transObjFromArray(option, eventDealName) }
      });
    }
  },
  mounted() {
    this.reloadChart();
  }
};
</script>