<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">

          <a-col :md="6" :sm="12">
            <a-form-item label="发起人">
              <a-input placeholder="输入发起人名称" v-model="queryParam.involvedUser"></a-input>
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
            <a-form-item label="启用状态">
              <a-select v-model="queryParam.suspended" placeholder="启用状态">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option value="1">激活</a-select-option>
                <a-select-option value="2">暂停</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
            <a-form-item label="工作流分类">
              <a-select v-model="queryParam.category" placeholder="工作流分类">
                <a-select-option value="">请选择</a-select-option>
                <a-select-option value="OA办公">OA办公</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <!--<div class="table-operator" style="border-top: 5px">-->
      <!--<a-upload name="uploadfile" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="uploadProcess" @change="handleImportExcel">-->
        <!--<a-button type="primary" icon="import">上传流程</a-button>-->
      <!--</a-upload>-->
      <!--<a-dropdown v-if="selectedRowKeys.length > 0">-->
        <!--<a-menu slot="overlay" @click="handleMenuClick">-->
          <!--<a-menu-item key="1">-->
            <!--<a-icon type="delete" @click="batchDel"/>-->
            <!--删除-->
          <!--</a-menu-item>-->
        <!--</a-menu>-->
        <!--<a-button style="margin-left: 8px">-->
          <!--批量操作-->
          <!--<a-icon type="down"/>-->
        <!--</a-button>-->
      <!--</a-dropdown>-->
    <!--</div>-->

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i>已选择&nbsp;<a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项&nbsp;&nbsp;
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        bordered
        size="middle"
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange">

        <!--<template slot="avatarslot" slot-scope="text, record, index">-->
          <!--<div class="anty-img-wrap">-->
            <!--<a-avatar shape="square" :src="getAvatarView(record.avatar)" icon="user"/>-->
          <!--</div>-->
        <!--</template>-->
        <!--<template slot="diagramResource" slot-scope="text, record, index">-->
          <!--<a target="_blank" :href="getDiagramUrl(record)">流程图</a>-->
        <!--</template>-->

        <span slot="action" slot-scope="text, record">
         <!-- <a @click="handleEdit(record)" v-has="'user:edit'">编辑</a>-->
          <a target="_blank" :href="getDiagramUrl(record)">查看</a>
          <!--<a-divider type="vertical"/>-->
           <!--<a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">-->
                  <!--<a>删除</a>-->
           <!--</a-popconfirm>-->
          <!--<a-divider type="vertical"/>-->
          <!--<a-dropdown>-->
            <!--<a class="ant-dropdown-link">-->
              <!--更多 <a-icon type="down"/>-->
            <!--</a>-->
            <!--<a-menu slot="overlay">-->
              <!--<a-menu-item>-->
                <!--<a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">-->
                  <!--<a>删除</a>-->
                <!--</a-popconfirm>-->
              <!--</a-menu-item>-->

              <!--&lt;!&ndash;<a-menu-item>&ndash;&gt;-->
                <!--&lt;!&ndash;<a href="javascript:;" @click="handleAgentSettings(record.username)">代理人</a>&ndash;&gt;-->
              <!--&lt;!&ndash;</a-menu-item>&ndash;&gt;-->

            <!--</a-menu>-->
          <!--</a-dropdown>-->
        </span>


      </a-table>
    </div>

  </a-card>
</template>

<script>
  import {putAction,getFileAccessHttpUrl} from '@/api/manage';
  import {frozenBatch} from '@/api/api'
  import {JeecgListMixin} from '@/mixins/JeecgListMixin'
  import JInput from '@/components/jeecg/JInput'
  import { getAction } from '@/api/manage'

  export default {
    name: "ProcessInstantList",
    mixins: [JeecgListMixin],
    components: {
      // SysUserAgentModal,
      // UserModal,
      // PasswordModal,
      JInput,
      // UserRecycleBinModal
    },
    data() {
      return {
        description: '流程实例列表',
        queryParam: {},
        recycleBinVisible: false,
        columns: [
          {
            title: '业务号',
            align: "center",
            dataIndex: 'businessKey',
          },
          {
            title: '流程名称',
            align: "center",
            dataIndex: 'name',
          },
          {
            title: '流程定义id',
            align: "center",
            dataIndex: 'processDefinitionId',
          },
          {
            title: '流程实例Id',
            align: "center",
            dataIndex: 'processInstanceId',
          },
          {
            title: '开始时间',
            align: "center",
            dataIndex: 'startTime',
          },
          {
            title: '发起人',
            align: "center",
            dataIndex: 'startUserId'
          },

          {
            title: '启用状态',
            align: "center",
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
          list: "/activiti/processInstList",
          delete: "/activiti/deleteDeploy",
        },
      }
    },
    computed: {
      uploadProcess: function(){
        return `${window._CONFIG['domianURL']}/${this.url.uploadProcess}`;
      }
    },
    methods: {
      processTrace: function(record) {
        getAction().then((res) => {
          if (res.success && res.result) {
            this.dataSource = res.result.records;
            this.ipagination.total = res.result.total;
          }
        })
      },
      getDiagramUrl: function (record) {
        return window._CONFIG['domianURL']+"/activiti/traceProcess?processDefinitionId="+record.processDefinitionId+"&processInstanceId="+record.processInstanceId;
      },
      batchFrozen: function (status) {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return false;
        } else {
          let ids = "";
          let that = this;
          let isAdmin = false;
          that.selectionRows.forEach(function (row) {
            if (row.username == 'admin') {
              isAdmin = true;
            }
          });
          if (isAdmin) {
            that.$message.warning('管理员账号不允许此操作,请重新选择！');
            return;
          }
          that.selectedRowKeys.forEach(function (val) {
            ids += val + ",";
          });
          that.$confirm({
            title: "确认操作",
            content: "是否" + (status == 1 ? "解冻" : "冻结") + "选中账号?",
            onOk: function () {
              frozenBatch({ids: ids, status: status}).then((res) => {
                if (res.success) {
                  that.$message.success(res.message);
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              });
            }
          });
        }
      },
      handleMenuClick(e) {
        if (e.key == 1) {
          this.batchDel();
        } else if (e.key == 2) {
          this.batchFrozen(2);
        } else if (e.key == 3) {
          this.batchFrozen(1);
        }
      },
      handleFrozen: function (id, status, username) {
        let that = this;
        //TODO 后台校验管理员角色
        if ('admin' == username) {
          that.$message.warning('管理员账号不允许此操作！');
          return;
        }
        frozenBatch({ids: id, status: status}).then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            that.loadData();
          } else {
            that.$message.warning(res.message);
          }
        });
      },
      handleChangePassword(username) {
        this.$refs.passwordmodal.show(username);
      },
      handleAgentSettings(username){
        this.$refs.sysUserAgentModal.agentSettings(username);
        this.$refs.sysUserAgentModal.title = "用户代理人设置";
      },
      passwordModalOk() {
        //TODO 密码修改完成 不需要刷新页面，可以把datasource中的数据更新一下
      }
    }

  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>