<template>
  <div>
    <el-dialog
      title="修改地址"
      :visible.sync="editAddressDialogVisible"
      width="50%"
      @close="editAddressDialogClosed"
    >
      <el-form ref="editAddressFormRef" :model="editAddressForm" :rules="editAddressFormRules" label-width="100px">
        <el-form-item label="省市区/县" prop="address1">
          <el-cascader style="width: 100%"
            v-model="editAddressForm.address1"
            :options="cityDate"
            :props="{ expandTrigger: 'hover'}"
            ></el-cascader>
        </el-form-item>
        <el-form-item label="详细地址" prop="address2">
          <el-input v-model="editAddressForm.address2"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editAddressDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editAddressDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import cityDate from './cityData.js'

export default {
  data() {
    return {
      editAddressDialogVisible: false,
      editAddressForm: {
        address1: [],
        address2: ''
      },
      editAddressFormRules: {
        address1: [
          { required: true, message: '请选择省市区/县', trigger: 'blur' }
        ],
        address2: [
          { required: true, message: '请输入详细信息', trigger: 'blur' }
        ]
      },
      cityDate
    }
  },
  methods: {
    editAddressDialogClosed() {
      this.$refs.editAddressFormRef.resetFields()
    }
  }
}
</script>

<style lang="less" scoped>
</style>
