<template>
  <a-modal
    :title="title"
    :width="800"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭"
    wrapClassName="ant-modal-cust-warp"
    style="top:5%;height: 85%;overflow-y: hidden">

    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="申请人" >
          {{userRealName}}
        </a-form-item>
        <a-form-item label="请假类型" :labelCol="labelCol" :wrapperCol="wrapperCol" :disabled="true">
          <a-select :disabled="isAudit" v-decorator="[ 'leaveType', {}]" placeholder="请选择" :getPopupContainer= "(target) => target.parentNode">
            <a-select-option :value="1">事假</a-select-option>
            <a-select-option :value="2">病假</a-select-option>
            <a-select-option :value="3">年假</a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="开始时间">
          <j-date :disabled="isAudit" style="width: 100%" :getCalendarContainer="node => node.parentNode" v-decorator="[ 'startTime', validatorRules.startTime]" placeholder="请选择开始时间" showTime dateFormat="YYYY-MM-DD HH:mm:ss" ></j-date>
          <!--<a-date-picker showTime format='YYYY-MM-DD HH:mm:ss' v-decorator="[ 'startTime',{}, validatorRules.startTime]"/>-->
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="结束时间">
          <j-date :disabled="isAudit" style="width: 100%" :getCalendarContainer="node => node.parentNode" v-decorator="[ 'endTime', validatorRules.endTime]" placeholder="请选择开始时间" showTime dateFormat="YYYY-MM-DD HH:mm:ss" ></j-date>
          <!--<a-date-picker showTime format='YYYY-MM-DD HH:mm:ss' v-decorator="[ 'endTime',{}, validatorRules.endTime ]"/>-->
        </a-form-item>

        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="原因">
          <a-textarea :disabled="isAudit" :rows="5" placeholder="..." v-decorator="[ 'leaveReason', validatorRules.leaveReason ]" />
        </a-form-item>

        <a-form-item v-if="isAudit" label="审核结果" :labelCol="labelCol" :wrapperCol="wrapperCol" :disabled="true">
          <a-select   v-decorator="[ 'approval', {}]" placeholder="请选择" :getPopupContainer= "(target) => target.parentNode">
            <a-select-option :value="1">同意</a-select-option>
            <a-select-option :value="2">拒绝</a-select-option>
          </a-select>
        </a-form-item>
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
  import pick from 'lodash.pick'
  import {startLeaveApply,auditLeaveApply } from '@/api/api'
  import { USER_INFO } from '@/store/mutation-types'
  import Vue from 'vue'
  import JDate from '@/components/jeecg/JDate'

  export default {
    name: "LeaveApplyModal",
    components: { JDate},
    data () {
      return {
        title:"操作",
        userRealName: Vue.ls.get(USER_INFO).realname,
        visible: false,
        isAudit: false,
        model: {},
        taskId: null,
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        form: this.$form.createForm(this),
        validatorRules:{
          startTime:{
            rules: [
              { required: true, message: '请选择开始时间!' }
            ]},
          endTime:{
            rules: [
              { required: true, message: '请选择结束时间!' }
            ]},
          leaveReason:{
            rules: [
              { min: 0, max: 250, message: '长度不超过 250 个字符', trigger: 'blur' }
            ]}
        },
      }
    },
    created () {
      this.model =  {"username": Vue.ls.get(USER_INFO).username,
        "userRealName": Vue.ls.get(USER_INFO).realname};
    },
    methods: {
      add () {
        this.edit();
      },
      edit (record) {
        // this.model =  {"username": Vue.ls.get(USER_INFO).username,
        //   "userRealName": Vue.ls.get(USER_INFO).realname};
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;

        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'username', 'leaveType','leaveReason','startTime','endTime','createTime'))
        });

      },
      audit(record,taskId){
        this.isAudit = true;
        this.taskId = taskId;
        this.edit(record)
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        if(!this.isAudit){
          this.form.validateFields((err, values) => {
            if (!err) {
              that.confirmLoading = true;
              let formData = Object.assign(this.model, values);
              let obj;
              console.log(formData)
              obj = startLeaveApply(formData)
              obj.then((res)=>{
                if(res.success){
                  that.$message.success(res.message);
                  that.$emit('ok');
                }else{
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.confirmLoading = false;
                that.close();
              })
            }
          })
        } else {
          var param = {}
          param.approval =this.form.getFieldValue('approval');
          param.taskId= this.taskId;
          let obj = auditLeaveApply(param);
          obj.then((res)=>{
            if(res.success){
              that.$message.success(res.message);
              that.$emit('ok');
            }else{
              that.$message.warning(res.message);
            }
          }).finally(() => {
            that.confirmLoading = false;
            that.close();
          })
        }

      },
      handleCancel () {
        this.close()
      }

    }
  }
</script>

<style scoped>

</style>