<template>
  <div>
    <el-dialog
      :title="'添加' + titleText"
      :visible.sync="addParamsDialogVisible"
      width="50%"
      @close="addParamsDialogClosed"
    >
      <el-form
        ref="addParamsFormRef"
        :model="addParamsForm"
        :rules="addParamsFormRules"
        label-width="80px"
      >
        <el-form-item :label="titleText" prop="attr_name">
          <el-input v-model="addParamsForm.attr_name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addParamsDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addParams">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    titleText: String,
    id: Number
  },
  data() {
    return {
      addParamsDialogVisible: false,
      addParamsForm: {
        attr_name: ''
      },
      addParamsFormRules: {
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
    addParamsDialogClosed() {
      this.$refs.addParamsFormRef.resetFields()
    },
    addParams() {
      this.$refs.addParamsFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post(
          `categories/${this.id}/attributes`,
          {
            attr_name: this.addParamsForm.attr_name,
            attr_sel: this.attrSel
          }
        )
        if (res.meta.status !== 201) { return this.$message.error('添加参数失败') }
        this.$message.success('添加参数成功')
        this.addParamsDialogVisible = false
        this.$emit('refreshGetParamsData')
      })
    }
  }
}
</script>

<style lang="less" scoped>
</style>
