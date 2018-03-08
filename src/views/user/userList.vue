<template>
  <div class="app-container calendar-list-container">
    <div class="filter-container">
      <el-input @keyup.enter.native="handleFilter" style="width: 200px;" class="filter-item" :placeholder="$t('table.user_name')+'/'+$t('table.id')" v-model="listQuery.title">
      </el-input>
      <el-button class="filter-item" type="primary" v-waves icon="el-icon-search" @click="handleFilter">{{$t('table.search')}}</el-button>
      <el-button class="filter-item" style="margin-left: 10px;" @click="handleCreate" type="primary" icon="el-icon-edit">{{$t('table.add')}}</el-button>
      <el-button class="filter-item" type="primary" :loading="downloadLoading" v-waves icon="el-icon-download" @click="handleDownload">{{$t('table.export')}}</el-button>
    </div>

    <el-table :key='tableKey' :data="tableData" v-loading="listLoading" element-loading-text="给我一点时间" border fit highlight-current-row
      style="width: 100%">
      <el-table-column width="95" align="center" :label="$t('table.id')">
        <template slot-scope="scope">
          <span>{{scope.row.id}}</span>
        </template>
      </el-table-column>
      <el-table-column width="150" align="center" :label="$t('table.header_img')">
        <template slot-scope="scope">
          <img class="user-avatar" :src="scope.row.header_img" alt="">
        </template>
      </el-table-column>
      <el-table-column width="150" align="center" :label="$t('table.date')">
        <template slot-scope="scope">
          <span>{{scope.row.date}}</span>
        </template>
      </el-table-column>
      <el-table-column width="110" align="center" :label="$t('table.user_name')">
        <template slot-scope="scope">
          <span>{{scope.row.user_name}}</span>
        </template>
      </el-table-column>
      <el-table-column width="110" align="center" :label="$t('table.account')">
        <template slot-scope="scope">
          <span>{{scope.row.account}}</span>
        </template>
      </el-table-column>
      <el-table-column width="65" align="center" :label="$t('table.sex')">
        <template slot-scope="scope">
          <span>{{scope.row.sex}}</span>
        </template>
      </el-table-column>
      <el-table-column width="110" align="center" :label="$t('table.role')">
        <template slot-scope="scope">
          <span>{{scope.row.role}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" :label="$t('table.actions')" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleUpdate(scope.row)">{{$t('table.edit')}}</el-button>
          <el-button size="mini" @click="handleGoDetails(scope.row)">{{$t('table.details')}}
          </el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row,'deleted')">{{$t('table.delete')}}
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <div class="pagination-container">
      <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="listQuery.page" :page-sizes="[10,20,30, 50]" :page-size="listQuery.limit" layout="total, sizes, prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>

  </div>
</template>

<script>
import waves from '@/directive/waves' // 水波纹指令
import { parseTime } from '@/utils' // 解析时间

export default {
  name: 'userList',
  directives: {
    waves
  },
  data() {
    return {
      tableData: null, // 表格数据
      tableKey: 0,
      total: null, // 总页数
      listLoading: false, // loading动画
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: '+id'
      },
      downloadLoading: false
    }
  },
  created() {
    this.getList()
  },
  methods: {
    // 获取用户列表数据
    getList(type) {
      // this.listLoading = true
      if (type) {
        this.tableData = [{
          id: '2',
          sex: '女',
          account: '1234510',
          header_img: 'http://dspx.tstmobile.com/uploads/touxiang/20180205/1e9865474a2c5b9aa7e68a9cff3f72a6.png',
          date: '2016-05-03',
          user_name: '王小',
          role: 'admin'
        }]
      } else {
        this.tableData = [{
          id: '1',
          sex: '男',
          account: '1234511',
          header_img: 'http://dspx.tstmobile.com/uploads/touxiang/20180205/1e9865474a2c5b9aa7e68a9cff3f72a6.png',
          date: '2016-05-03',
          user_name: '王萨的',
          role: 'edit'
        }, {
          id: '2',
          sex: '女',
          account: '1234510',
          header_img: 'http://dspx.tstmobile.com/uploads/touxiang/20180205/1e9865474a2c5b9aa7e68a9cff3f72a6.png',
          date: '2016-05-03',
          user_name: '王小',
          role: 'admin'
        }]
      }
    },
    // 搜索
    handleFilter() {
      this.listQuery.page = 1
      this.getList(1)
    },
    // 设置每页显示多少条数据
    handleSizeChange(val) {
      this.listQuery.limit = val
      this.getList()
    },
    handleCurrentChange(val) {
      this.listQuery.page = val
      this.getList()
    },
    handleModifyStatus(row, status) {
      this.$message({
        message: '操作成功',
        type: 'success'
      })
      row.status = status
    },
    // 详情
    handleGoDetails(row) {
      this.$router.push({
        path: 'userDetails'
      })
    },
    // 编辑
    handleUpdate(row) {
      this.$router.push({
        path: 'userDetails'
      })
    },
    // 删除
    handleDelete(row) {
      this.$notify({
        title: '成功',
        message: '删除成功',
        type: 'success',
        duration: 2000
      })
      const index = this.tableData.indexOf(row)
      this.tableData.splice(index, 1)
    },
    // 添加
    handleCreate() {
      this.$router.push({
        path: 'userDetails'
      })
    },
    // 导出
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['ID', '头像', '时间', '用户名', '账户', '性别', '角色']
        const filterVal = ['id', 'header_img', 'date', 'user_name', 'account', 'sex', 'role']
        const data = this.formatJson(filterVal, this.tableData)
        excel.export_json_to_excel(tHeader, data, 'user-list') // 设置表头，表数据，表名称
        this.downloadLoading = false
      })
    },
    // 遍历表数据并返回
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'ID') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .user-avatar{
    width: 40px;
    height: 40px;
    border-radius: 10px;
  }
</style>
