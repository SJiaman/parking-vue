<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="订单号" prop="orderId">
      <el-input v-model="dataForm.orderId" placeholder="订单号"></el-input>
    </el-form-item>
    <el-form-item label="收费金额" prop="money">
      <el-input v-model="dataForm.money" placeholder="收费金额"></el-input>
    </el-form-item>
    <el-form-item label="车牌号" prop="licencePlate">
      <el-input v-model="dataForm.licencePlate" placeholder="车牌号"></el-input>
    </el-form-item>
    <el-form-item label="停车时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="停车时间"></el-input>
    </el-form-item>
    <el-form-item label="停车时间" prop="expirationTime">
      <el-input v-model="dataForm.expirationTime" placeholder="停车时间"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          orderId: '',
          money: '',
          licencePlate: '',
          createTime: '',
          expirationTime: ''
        },
        dataRule: {
          orderId: [
            { required: true, message: '订单号不能为空', trigger: 'blur' }
          ],
          money: [
            { required: true, message: '收费金额不能为空', trigger: 'blur' }
          ],
          licencePlate: [
            { required: true, message: '车牌号不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '停车时间不能为空', trigger: 'blur' }
          ],
          expirationTime: [
            { required: true, message: '停车时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/pms/order/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.orderId = data.order.orderId
                this.dataForm.money = data.order.money
                this.dataForm.licencePlate = data.order.licencePlate
                this.dataForm.createTime = data.order.createTime
                this.dataForm.expirationTime = data.order.expirationTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/pms/order/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'orderId': this.dataForm.orderId,
                'money': this.dataForm.money,
                'licencePlate': this.dataForm.licencePlate,
                'createTime': this.dataForm.createTime,
                'expirationTime': this.dataForm.expirationTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
