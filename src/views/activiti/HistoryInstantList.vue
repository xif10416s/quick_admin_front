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
          <a @click="getHistory(record)">历史</a>

          <!--<a-divider type="vertical"/>-->
          <!--<a-dropdown>-->
            <!--<a class="ant-dropdown-link">更多 <a-icon type="down"/></a>-->
            <!--<a-menu slot="overlay">-->
              <!--<a-menu-item>-->
                <!--<a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.deploymentId)">-->
                  <!--<a>删除</a>-->
                <!--</a-popconfirm>-->
              <!--</a-menu-item>-->
            <!--</a-menu>-->
          <!--</a-dropdown>-->
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

  </a-card>
</template>

<script>
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import JDictSelectTag from '@/components/dict/JDictSelectTag'
  import { getAction} from '@/api/manage'
  import moment from 'moment'
  export default {
    name: 'HistoryInstantList',
    mixins: [JeecgListMixin],
    components: {
      JDictSelectTag,
      moment
    },
    data() {
      return {
        description: '历史任务实例列表',
        // 表头
        columns: [
          {
            title: '实例Id',
            align: 'center',
            dataIndex: 'id'
          },
          {
            title: '实例名称',
            align: 'center',
            dataIndex: 'processDefinitionName'
          },
          {
            title: '实例定义',
            align: 'center',
            dataIndex: 'processDefinitionKey'
          },
          {
            title: '发起者',
            align: 'center',
            dataIndex: 'startUserId'
          },
          {
            title: '业务号',
            align: 'center',
            dataIndex: 'businessKey'
          },
          {
            title: '开始时间',
            align: 'center',
            dataIndex: 'startTime'
            ,
            customRender: (text) => {
              return moment(text).format('YYYY-MM-DD HH:mm:ss')
            }
          },
          {
            title: '结束时间',
            align: 'center',
            dataIndex: 'endTime'
            ,
            customRender: (text) => {
              return moment(text).format('YYYY-MM-DD HH:mm:ss')
            }
          },
          {
            title: '操作',
            dataIndex: 'action',
            align: 'center',
            scopedSlots: { customRender: 'action' },
          }
        ],
        url: {
          list: '/activiti/processHis',
          delete: '/activiti/processHis/delete',
        },
      }
    },
    methods:{

    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>