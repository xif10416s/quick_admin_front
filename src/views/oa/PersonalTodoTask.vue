<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <!--<div class="table-page-search-wrapper">-->
      <!--<a-form layout="inline" @keyup.enter.native="searchQuery">-->
        <!--<a-row :gutter="24">-->

          <!--<a-col :md="6" :sm="8">-->
            <!--<a-form-item label="职务编码">-->
              <!--<a-input placeholder="请输入职务编码" v-model="queryParam.code"></a-input>-->
            <!--</a-form-item>-->
          <!--</a-col>-->
          <!--<a-col :md="6" :sm="8">-->
            <!--<a-form-item label="职务名称">-->
              <!--<a-input placeholder="请输入职务名称" v-model="queryParam.name"></a-input>-->
            <!--</a-form-item>-->
          <!--</a-col>-->
          <!--<template v-if="toggleSearchStatus">-->
            <!--<a-col :md="6" :sm="8">-->
              <!--<a-form-item label="职级">-->
                <!--<j-dict-select-tag v-model="queryParam.postRank" placeholder="请选择职级" dictCode="position_rank"/>-->
              <!--</a-form-item>-->
            <!--</a-col>-->

          <!--</template>-->
          <!--<a-col :md="6" :sm="8">-->
            <!--<span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">-->
              <!--<a-button type="primary" @click="searchQuery" icon="search">查询</a-button>-->
              <!--<a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>-->
              <!--<a @click="handleToggleSearch" style="margin-left: 8px">-->
                <!--{{ toggleSearchStatus ? '收起' : '展开' }}-->
                <!--<a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>-->
              <!--</a>-->
            <!--</span>-->
          <!--</a-col>-->

        <!--</a-row>-->
      <!--</a-form>-->
    <!--</div>-->

    <!-- 操作按钮区域 -->
    <!--<div class="table-operator">-->
      <!--&lt;!&ndash;<a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>&ndash;&gt;-->
      <!--<a-button type="primary" icon="download" @click="handleExportXls('职务表')">导出</a-button>-->
      <!--<a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">-->
        <!--<a-button type="primary" icon="import">导入</a-button>-->
      <!--</a-upload>-->
      <!--<a-dropdown v-if="selectedRowKeys.length > 0">-->
        <!--<a-menu slot="overlay">-->
          <!--<a-menu-item key="1" @click="batchDel">-->
            <!--<a-icon type="delete"/>-->
            <!--删除-->
          <!--</a-menu-item>-->
        <!--</a-menu>-->
        <!--<a-button style="margin-left: 8px"> 批量操作-->
          <!--<a-icon type="down"/>-->
        <!--</a-button>-->
      <!--</a-dropdown>-->
    <!--</div>-->

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handlerAudit(record)">审核</a>

          <a-divider type="vertical"/>
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down"/></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.deploymentId)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <leave-apply-modal ref="modalForm" @ok="modalFormOk"></leave-apply-modal>

  </a-card>
</template>

<script>
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import JDictSelectTag from '@/components/dict/JDictSelectTag'
  import LeaveApplyModal from './modules/LeaveApplyModal'
  import { getAction} from '@/api/manage'

  export default {
    name: 'PersonalTodoTask',
    mixins: [JeecgListMixin],
    components: {
      JDictSelectTag,
      LeaveApplyModal
    },
    data() {
      return {
        description: '个人代办任务页面',
        // 表头
        columns: [
          {
            title: '任务Id',
            align: 'center',
            dataIndex: 'id'
          },
          {
            title: '任务名称',
            align: 'center',
            dataIndex: 'name'
          },
          {
            title: '任务分类',
            align: 'center',
            dataIndex: 'category'
          },
          {
            title: '流程实例id',
            align: 'center',
            dataIndex: 'processInstanceId'
          },
          {
            title: '任务定义',
            align: 'center',
            dataIndex: 'taskDefinitionKey'
          },
          {
            title: '指定处理人',
            align: 'center',
            dataIndex: 'assignee'
          },
          {
            title: '创建时间',
            align: 'center',
            dataIndex: 'createTime'
          },
          {
            title: '截止时间',
            align: 'center',
            dataIndex: 'dueDate'
          },
          {
            title: '启动中',
            align: 'center',
            dataIndex: 'suspended'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align: 'center',
            scopedSlots: { customRender: 'action' },
          }
        ],
        url: {
          list: '/activiti/todoTaskList',
          delete: '/activiti/position/delete',
        },
      }
    },
    computed: {
      importExcelUrl: function () {
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`
      }
    },
    methods:{
      handlerAudit: function(record){
        if(record.taskDefinitionKey != "report"){
          getAction("/oa/leaveApply/getLeaveApplyModel",
            { processInstanceId: record.processInstanceId }).then(response => {
            if (response.success) {

              const result = response.result
              this.$refs.modalForm.audit(result, record.id);
              this.$refs.modalForm.title = "编辑";
              this.$refs.modalForm.disableSubmit = false;
            }
          })
        } else {
          getAction("/oa/leaveApply/reportBack",
            { taskId: record.id }).then(response => {
            if (response.success) {
              this.modalFormOk();
            }
          })
        }
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>