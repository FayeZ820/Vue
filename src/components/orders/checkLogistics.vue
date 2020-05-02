<template>
  <div>
    <el-dialog title="物流进度" :visible.sync="checkLogisticsDialogVisible" width="50%">
      <el-timeline>
        <el-timeline-item
          v-for="(activity, index) in logisticsInfo"
          :key="index"
          :timestamp="activity.time"
        >{{activity.context}}</el-timeline-item>
      </el-timeline>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    logisticsInfo: []
  },
  data() {
    return {
      checkLogisticsDialogVisible: false
    }
  }
}
</script>

<style lang="less" scoped>
</style>
