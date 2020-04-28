<template>
  <div>
    <el-dialog
      :title="'修改' + titleText"
      :visible.sync="editParamsDialogVisible"
      width="50%"
      @close="editParamsDialogClosed"
    >
      <el-form
        ref="editParamsFormRef"
        :model="editParamsForm"
        :rules="editParamsFormRules"
        label-width="80px"
      >
        <el-form-item :label="titleText" prop="attr_name">
          <el-input v-model="editParamsForm.attr_name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editParamsDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editParams">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    editParamsForm: {},
    titleText: String,
    id: Number
  },
  data() {
    return {
      editParamsDialogVisible: false,
      editParamsFormRules: {
        attr_name: [
          { required: true, message: '请输入参数名称', trigger: 'blur' }
        ]
      }
    }
  },
  computed: {
    attrSel() {
      if (this.titleText === '动态参数') {
        return 'many'
      }
      return 'only'
    }
  },
  methods: {
    editParamsDialogClosed() {
      this.$refs.editParamsFormRef.resetFields()
    },
    editParams() {
      this.$refs.editParamsFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.put(
          `categories/${this.id}/attributes/${this.editParamsForm.attr_id}`,
          {
            attr_name: this.editParamsForm.attr_name,
            attr_sel: this.attrSel
          }
        )
        if (res.meta.status !== 200) { return this.$message.error('修改参数失败') }
        this.$message.success('修改参数成功')
        this.$emit('refreshGetParamsData')
        this.editParamsDialogVisible = false
      })
    }
  }
}
</script>

<style lang="less" scoped>
</style>
