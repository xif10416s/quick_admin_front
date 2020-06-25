<template>
  <a-card title="工单申请">
    <a-card-grid @click="handleAdd" style="width:25%;text-align:center">
      <a-icon type="alert"/>
      <label icon="plus" style=""> 请假申请</label>
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center" :hoverable="false">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>
    <a-card-grid style="width:25%;text-align:center">
      Content
    </a-card-grid>

    <!-- 表单区域 -->
    <leave-apply-modal ref="modalForm" @ok="modalFormOk"></leave-apply-modal>

  </a-card>
</template>
<script>
  //import md5 from "md5"
  import LeaveApplyModal from './modules/LeaveApplyModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'

  export default {
    name: 'WorkSheet',
    mixins: [JeecgListMixin],
    components: {
      LeaveApplyModal
    },
    data () {
      return {
        url: {
          list: ''
        }
      }
    },
    methods: {

    }
  }
</script>