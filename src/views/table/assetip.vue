<template>
  <div>
    <el-collapse v-model="activeNames">
      <el-collapse-item name="1">
        <template slot="title"><i class="header-icon el-icon-info" />菜单栏隐藏与显示</template>
        <!-- 查询条件 -->
        <el-form ref="searchform" inline size="small" :model="searchMap">
          <!-- <el-input  v-model="searchMap.projectinfoid" placeholder="项目信息编号"  width="8"></el-input> -->

          <el-form-item prop="projectinfoid" label="项目信息">
            <el-select v-model="searchMap.projectinfoid" style="width:180px;" filterable remote allow-create default-first-option clearable placeholder="请输入" :remote-method="getProjectInfoList" :loading="searchLoading">
              <el-option v-for="item in projectinfoList" :key="item.id" :label="item.projectname" :value="item.id" /></el-select>
          </el-form-item>

          <el-form-item prop="ipaddressv4" label="ipv4">
            <el-select v-model="searchMap.ipaddressv4" style="width:150px;" filterable remote allow-create default-first-option clearable placeholder="ipv4模糊查询" :remote-method="getIpaddressv4List" :loading="searchLoading">
              <el-option v-for="item in ipaddressv4List" :key="item.id" :label="item.ipaddressv4" :value="item.ipaddressv4" /></el-select>
          </el-form-item>

          <el-form-item prop="ipv4" label="ipv4">
            <el-select v-model="searchMap.ipv4" style="width:150px;" filterable remote clearable placeholder="ipv4精准查询" :remote-method="getIpaddressv4List" :loading="searchLoading">
              <el-option v-for="item in ipaddressv4List" :key="item.id" :label="item.ipaddressv4" :value="item.ipaddressv4" /></el-select>
          </el-form-item>

          <el-form-item prop="ipaddressv6" label="ipv6">
            <el-select v-model="searchMap.ipaddressv6" style="width:180px;" filterable remote allow-create default-first-option clearable placeholder="请输入" :remote-method="getIpaddressv6List" :loading="searchLoading">
              <el-option v-for="item in ipaddressv6List" :key="item.id" :label="item.ipaddressv6" :value="item.ipaddressv6" /></el-select>
          </el-form-item>

          <el-form-item prop="checkwhitelist" label="安全检测白名单">
            <el-switch v-model="searchMap.checkwhitelist" />
          </el-form-item>
          <el-form-item prop="assetnotifywhitelist" label="资产提醒白名单">
            <el-switch v-model="searchMap.assetnotifywhitelist" />
          </el-form-item>

          <el-form-item prop="activetime" label="发现时间">
            <el-date-picker v-model="searchMap.activetime" type="datetimerange" value-format="yyyy-MM-dd HH:mm:ss" start-placeholder="开始日期" range-separator="-" end-placeholder="结束日期" :picker-options="pickerOptions" style="width:350px;" />
          </el-form-item>
          <el-form-item prop="passivetime" label="下线时间">
            <el-date-picker v-model="searchMap.passivetime" type="datetimerange" value-format="yyyy-MM-dd HH:mm:ss" start-placeholder="开始日期" range-separator="-" end-placeholder="结束日期" :picker-options="pickerOptions" style="width:350px;" />
          </el-form-item>

          <el-form-item prop="remark" label="备注">
            <el-select v-model="searchMap.remark" style="width:180px;" filterable remote allow-create default-first-option clearable placeholder="请输入" :remote-method="getRemarkList" :loading="searchLoading">
              <el-option v-for="item in remarkList" :key="item.id" :label="item.remark" :value="item.remark" /></el-select>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="fetchData()">查询</el-button>
            <el-tooltip placement="top">
              <div slot="content">如果选择白名单查询<br>需要清空查询条件<br>数据才是准确的</div>
              <el-button type="info" @click="resetForm('searchform')">重置</el-button>
            </el-tooltip>
          </el-form-item>
          <!-- </el-form> -->

          <!-- 操作 -->
          <!-- <el-form inline size="small"> -->
          <el-form-item>
            <el-input v-model="filename" placeholder="默认名字：报告" clearable style="width:180px;" prefix-icon="el-icon-document" />
            <el-button :loading="downloadLoading" type="success" icon="el-icon-document" @click="handleDownload">导出</el-button>
          </el-form-item>
          <el-form-item>
            <el-tooltip placement="top">
              <div slot="content">删除已选记录的所有相关信息<br>请谨慎操作!!!</div>
              <el-button type="danger" icon="el-icon-delete" @click="handleDeleteAll">删除</el-button>
            </el-tooltip>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="handleEdit('')">新增</el-button>
          </el-form-item>
        </el-form>
      </el-collapse-item>    </el-collapse>

    <el-drawer
      title="详情"
      :visible.sync="drawer"
      :with-header="false"
      direction="rtl"
      size="65%"
      :before-close="handleDrawerClose"
    >
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b> {{ pojo.ipaddressv4 }} 所有信息</b></span>
        </div>
        <div class="text">
          <table border="0">
            <tr>
              <td><b>部门</b></td>
              <td>{{ departmentPojo.departmentname }}</td>
              <el-divider direction="vertical" />
              <td><b>项目信息</b></td>
              <td>{{ pojo.projectname }}</td>
            </tr>
            <tr>
              <td><b>ipv4</b></td>
              <td>{{ pojo.ipaddressv4 }}</td>
              <el-divider direction="vertical" />
              <td><b>ipv6</b></td>
              <td>{{ pojo.ipaddressv6 }}</td>
            </tr>
            <tr>
              <td><b>安全检测白名单</b></td>
              <td>{{ formatBoolean(pojo.checkwhitelist) }}</td>
              <el-divider direction="vertical" />
              <td><b>资产提醒白名单</b></td>
              <td>{{ formatBoolean(pojo.assetnotifywhitelist) }}</td>
            </tr>
            <tr>
              <td><b>发现时间</b></td>
              <td>{{ pojo.activetime| dateformat() }}</td>
              <el-divider direction="vertical" />
              <td><b>下线时间</b></td>
              <td>{{ pojo.passivetime | dateformat() }}</td>
            </tr>
            <tr>
              <td><b>备注</b></td>
              <td>{{ pojo.remark }}</td>
            </tr>
          </table>

        </div>
      </el-card>

      <!-- 联系人 -->
      <el-table :data="contactList" fit>
        <el-table-column prop="name" label="联系人" />
        <el-table-column prop="email" label="邮箱" />
        <el-table-column prop="phone" label="电话" />
      </el-table>

      <!-- 主机信息 -->
      <el-table :data="hostList" fit>
        <el-table-column prop="macaddress" label="mac地址" />
        <el-table-column prop="hostname" label="主机/域名" />
        <el-table-column prop="ostype" label="OS类型" />
        <el-table-column prop="osversion" label="OS版本" />
        <el-table-column prop="type" label="类型" />
        <el-table-column prop="owner" label="所有者" />
        <el-table-column prop="activetime" label="发现时间">
          <template slot-scope="scope">
            {{ scope.row.activetime | dateformat() }}
          </template>
        </el-table-column>
        <el-table-column prop="remark" label="备注" />
      </el-table>

      <!-- 端口 -->
      <el-table :data="portList" fit>
        <el-table-column prop="port" label="端口" />
        <el-table-column prop="protocol" label="协议" />
        <el-table-column prop="state" label="状态" />
        <el-table-column prop="service" label="服务" />
        <el-table-column prop="version" label="版本" />
        <el-table-column align="center" label="安全检测白名单">
          <template slot="header">
            <span>安全检测白名单</span>
            <template slot-scope="scope">
              <span>{{ formatBoolean(scope.row.checkwhitelist) }}</span>
            </template>
          </template></el-table-column>

        <el-table-column align="center" label="资产提醒白名单">
          <template slot-scope="scope">
            <span>{{ formatBoolean(scope.row.assetnotifywhitelist) }}</span>
          </template>
        </el-table-column>

        <el-table-column prop="uptime" label="发现时间">
          <template slot-scope="scope">
            {{ scope.row.uptime | dateformat() }}
          </template>
        </el-table-column>
        <el-table-column prop="downtime" label="关闭时间">
          <template slot-scope="scope">
            {{ scope.row.downtime | dateformat() }}
          </template>
        </el-table-column>
        <el-table-column prop="changedtime" label="修改时间">
          <template slot-scope="scope">
            {{ scope.row.changedtime | dateformat() }}
          </template>
        </el-table-column>
      </el-table>

      <!-- 检测结果 -->
      <el-table :data="checkresultList" fit>
        <!-- <el-table-column  prop="vulnid" width="150" label="漏洞名称">
              <template slot-scope="scope">
                {{ getVulnName(scope.row.id) }}
              </template>
            </el-table-column> -->

        <el-table-column prop="assetportid" label="端口" />
        <el-table-column prop="name" label="插件名称" />
        <el-table-column prop="risk" label="风险" />
        <el-table-column prop="result" label="检测结果" show-overflow-tooltip />

        <el-table-column prop="activetime" label="发现时间">
          <template slot-scope="scope">
            {{ scope.row.activetime | dateformat() }}
          </template>
        </el-table-column>
        <el-table-column prop="passivetime" label="修复时间">
          <template slot-scope="scope">
            {{ scope.row.passivetime | dateformat() }}
          </template>
        </el-table-column>
        <el-table-column prop="remark" label="备注" />
      </el-table>

      <!-- web信息 -->
      <el-table :data="webinfoList" fit>
        <el-table-column prop="portid" label="端口" />
        <el-table-column prop="title" label="title" />
        <el-table-column prop="bodychildrenstextcontent" label="body内容" show-overflow-tooltip />
        <el-table-column prop="server" label="server" />
        <el-table-column prop="xpoweredby" label="xpoweredby" show-overflow-tooltip />
        <el-table-column prop="setcookie" label="setcookie" show-overflow-tooltip />
        <el-table-column prop="wwwauthenticate" label="认证方式" show-overflow-tooltip />
        <el-table-column prop="crawltime" label="抓取时间">
          <template slot-scope="scope">
            {{ scope.row.crawltime | dateformat() }}
          </template>
        </el-table-column>
      </el-table>

      <!-- url链接 -->
      <el-table :data="urlList" fit>
        <el-table-column prop="webinfoid" label="端口" />
        <el-table-column prop="name" label="名称" />

        <el-table-column prop="url" label="url">
          <template slot-scope="scope">
            <el-link :href="scope.row.url" target="_blank" :underline="false">{{ scope.row.url }}</el-link>
          </template>
        </el-table-column>
      </el-table>
      <!-- {{ urlList }} -->

    </el-drawer>

    <!-- 表格数据 -->
    <el-table
      ref="multipleTable"
      v-loading="listLoading"
      :data="list"
      border
      fit
      style="width: 100%;"
      element-loading-text="拼命加载中"
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      @selection-change="handleSelectionChange"
    >

      <el-table-column type="selection" align="center" />

      <el-table-column label="序号" type="index" :index="1" align="center" width="50" />

      <!-- <el-table-column prop="id" label="资产ip编号" /> -->

      <el-table-column sortable prop="projectname" label="项目信息" />
      <!-- <el-table-column sortable prop="projectinfoid" label="项目信息">
        <template slot-scope="scope">
          {{ getProjectname(scope.row.projectinfoid) }}
        </template>
      </el-table-column> -->

      <!-- <el-table-column sortable prop="ipaddressv4" label="ipv4" /> -->

      <el-table-column sortable width="200" prop="ipaddressv4" label="ipv4">
        <template slot="header">
          <span>ipv4</span>
          <el-tooltip placement="top">
            <div slot="content">总端口数:未关闭端口数:总漏洞数:未修复漏洞数</div>
            <i class="el-icon-info" />
          </el-tooltip>
        </template>
        <template slot-scope="scope">
          <el-link :underline="false" @click="handleDrawer(scope.row.id) ">
            {{ scope.row.ipaddressv4 }}
          </el-link>
          <!-- </template>
        <template slot-scope="scope"> -->
          <span v-if=" scope.row.statistic">
            <el-tag size="mini" type="success" effect="plain">{{ scope.row.statistic }}</el-tag>
          </span>
        </template>
      </el-table-column>

      <!-- <el-table-column prop="statistic" label="统计">
        <template slot="header">
          <span>统计</span>
          <el-tooltip placement="top">
            <div slot="content">总端口数:未关闭端口数:总漏洞数:未修复漏洞数</div>
            <i class="el-icon-info" />
          </el-tooltip>
        </template>
        <template slot-scope="scope">
          <span v-if=" scope.row.statistic">
            <el-tag size="mini" type="success" effect="plain">{{ scope.row.statistic }}</el-tag>
          </span>
        </template>
      </el-table-column> -->

      <el-table-column sortable prop="ipaddressv6" label="ipv6" />

      <el-table-column align="center" sortable label="安全检测白名单">
        <template slot="header">
          <span>安全检测白名单</span>
          <el-tooltip placement="top">
            <div slot="content">如果ip在白名单，则该ip及ip的所有端口<br>都不会进行安全检测（即使端口不在白名单）</div>
            <i class="el-icon-info" />
          </el-tooltip>
        </template>
        <template slot-scope="scope">
          <span>{{ formatBoolean(scope.row.checkwhitelist) }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" sortable label="资产提醒白名单">
        <template slot="header">
          <span>资产提醒白名单</span>
          <el-tooltip placement="top">
            <div slot="content">如果ip在白名单，则该ip及ip的所有端口<br>都不提醒负责人（即使端口不在白名单）<br>默认提醒不受限制</div>
            <i class="el-icon-info" />
          </el-tooltip>
        </template>
        <template slot-scope="scope">
          <span>{{ formatBoolean(scope.row.assetnotifywhitelist) }}</span>
        </template>
      </el-table-column>

      <el-table-column sortable prop="activetime" label="发现时间">
        <template slot-scope="scope">
          {{ scope.row.activetime | dateformat() }}
        </template>
      </el-table-column>
      <el-table-column sortable prop="passivetime" label="下线时间">
        <template slot-scope="scope">
          {{ scope.row.passivetime | dateformat() }}
        </template>
      </el-table-column>
      <el-table-column sortable prop="remark" label="备注" />

      <el-table-column
        fixed="right"
        label="操作"
        width="100"
      >
        <template slot-scope="scope">
          <el-button type="primary" size="mini" icon="el-icon-edit" circle @click="handleEdit(scope.row.id)" />
          <el-button type="danger" size="mini" icon="el-icon-delete" circle @click="handleDelete(scope.row.id)" />
        </template>
      </el-table-column>
    </el-table>

    <!-- 底部分页 -->
    <el-pagination
      :current-page.sync="currentPage"
      :page-sizes="[10,20,50,200,500,1000]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
      @size-change="handleSizeChange"
      @current-change="fetchData"
    />

    <!-- 编辑框 -->
    <el-dialog title="编辑" :visible.sync="dialogFormVisible" width="50%" center :before-close="cleanCache">
      <el-form label-width="100px">
        <el-form-item label="项目信息">
          <span>{{ projectname }}</span>
          <el-select v-model="pojo.projectinfoid" style="width:300px;" filterable remote clearable placeholder="请输入" :remote-method="getProjectInfoList" :loading="searchLoading">
            <el-option v-for="item in projectinfoList" :key="item.id" :label="item.projectname" :value="item.id" /></el-select>
        </el-form-item>

        <!-- <el-form-item label="ipv4" required><el-input v-model="pojo.ipaddressv4" style="width:300px;" clearable /></el-form-item> -->
        <el-form-item prop="ipaddressv4" label="ipv4">
          {{ pojo.ipaddressv4 }}
          <span v-if="pojo.id==null">
            <el-select v-model="pojo.ipaddressv4" style="width:300px;" filterable remote allow-create default-first-option clearable placeholder="请输入" :remote-method="getIpaddressv4List" :loading="searchLoading">
              <el-option v-for="item in ipaddressv4List" :key="item.id" :label="item.ipaddressv4" :value="item.ipaddressv4" /></el-select>
          </span>
        </el-form-item>
        <el-form-item label="ipv6"><el-input v-model="pojo.ipaddressv6" style="width:300px;" clearable /></el-form-item>
        <el-form-item label="白名单">
          <el-switch v-model="pojo.checkwhitelist" active-text="安全检测" />
          <el-switch v-model="pojo.assetnotifywhitelist" active-text="资产提醒" />
        </el-form-item>
        <el-form-item label="时间">
          <el-date-picker v-model="pojo.activetime" style="width:300px;" placeholder="发现时间" type="datetime" />
          <el-date-picker v-model="pojo.passivetime" style="width:300px;" placeholder="下线时间" type="datetime" />
        </el-form-item>
        <el-form-item label="备注"><el-input v-model="pojo.remark" :autosize="{ minRows: 2, maxRows: 10 }" type="textarea" /></el-form-item>

      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="handleSave()">保存</el-button>
        <el-button @click="closeDialogForm()">关闭</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import assetipApi from '@/api/assetip'
import projectinfoApi from '@/api/projectinfo'
import assetportApi from '@/api/assetport'
import departmentApi from '@/api/department'
import contactProjectinfoApi from '@/api/contactProjectinfo'
import contactApi from '@/api/contact'
import hostApi from '@/api/host'
import checkresultApi from '@/api/checkresult'
import webinfoApi from '@/api/webinfo'
import urlApi from '@/api/url'

import Vue from 'vue'
const dateformat = Vue.filter('dateformat')

export default {
  data() {
    return {
      list: [],
      total: 0, // 总记录数
      currentPage: 1, // 当前页
      pageSize: 10, // 每页大小
      searchMap: {}, // 查询条件
      dialogFormVisible: false, // 编辑窗口是否可见
      pojo: {}, // 编辑表单绑定的实体对象
      id: '', // 当前用户修改的ID
      filename: '',
      listLoading: true,
      multipleSelection: [],
      downloadLoading: false,
      searchLoading: false,
      remarkList: [],
      ipaddressv4List: [],
      ipaddressv6List: [],
      projectinfoList: [],

      // 多表显示
      projectinfoPojo: {},
      departmentPojo: {},
      contactProjectinfoList: [],
      contactList: [],
      hostList: [],
      checkresultList: [],
      webinfoList: [],
      urlList: [],
      webinfoids: [],
      portList: [],
      portids: [],
      activeNames: ['1'],
      projectname: '',
      drawer: false,

      pickerOptions: { // 日期选择
        disabledDate(time) {
          return time.getTime() > Date.now()
        },
        shortcuts: [{
          text: '最近一周',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近一个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近三个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近半年',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 180)
            picker.$emit('pick', [start, end])
          }
        }]
      }
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    handleDrawer(id) {
      this.id = id
      this.drawer = true
      assetipApi.findById(id).then(response => {
        if (response.flag) {
          this.pojo = response.data

          // 资产端口
          assetportApi.findAllByAssetipId(this.id).then(response => {
            this.portList = response.data
            if (this.pojo.projectinfoid && this.pojo.projectinfoid !== '') {
              projectinfoApi.findById(this.pojo.projectinfoid).then(response => {
                if (response.data) {
                  this.projectinfoPojo = response.data
                  if (this.projectinfoPojo.departmentid) {
                    const departmentid = this.projectinfoPojo.departmentid
                    departmentApi.findById(departmentid).then(response => {
                      this.departmentPojo = response.data
                    })
                  }
                }
              })

              // 联系人信息
              contactProjectinfoApi.findAllByProjectinfoid(this.pojo.projectinfoid).then(response => {
                this.contactProjectinfoList = response.data
                for (let i = 0; i < this.contactProjectinfoList.length; i++) {
                  contactApi.findById(this.contactProjectinfoList[i].contactid).then(response => {
                    this.contactList.push(response.data)
                  })
                }
              })
            }

            if (this.portList.length !== 0) {
              for (let i = 0; i < this.portList.length; i++) {
                this.portids.push(this.portList[i].id)
              }
              // web信息
              webinfoApi.findAllByAssetportIds(this.portids).then(response => {
                this.webinfoList = response.data
              }).then(() => {
                // url
                if (this.webinfoList.length !== 0) {
                  for (let i = 0; i < this.webinfoList.length; i++) {
                    this.webinfoids.push(this.webinfoList[i].id)
                  }
                  urlApi.findAllByWebinfoIds2Port(this.webinfoids).then(response => {
                    this.urlList = response.data
                  })
                }
              })
              // 检测结果
              checkresultApi.findAllByAssetportIds(this.portids).then(response => {
                this.checkresultList = response.data
              })
            }
          }).then(() => {

          })
          // 主机信息
          hostApi.findAllByAssetipId(this.id).then(response => {
            this.hostList = response.data
          })
        }
      })
    },
    handleDrawerClose() {
      this.drawer = false
      this.closeDialogForm()
    },
    cleanCache() {
      this.closeDialogForm()
    },
    closeDialogForm() {
      this.dialogFormVisible = false
      this.projectinfoPojo = {}
      this.departmentPojo = {}
      this.contactProjectinfoList = []
      this.contactList = []
      this.hostList = []
      this.checkresultList = []
      this.webinfoList = []
      this.urlList = []
      this.webinfoids = []
      this.portList = []
      this.portids = []
      this.projectinfoList = []
      this.projectname = ''
    },
    getIpaddressv6List(query) {
      if (query !== '' && query) {
        this.searchLoading = true
        setTimeout(() => {
          this.searchLoading = false
          assetipApi.search(1, 10, { 'ipaddressv6': query }).then(response => {
            this.ipaddressv6List = response.data.rows.filter(item => {
              return item.ipaddressv6.toLowerCase().indexOf(query.toLowerCase()) > -1
            })
          })
        }, 200)
      } else {
        this.ipaddressv6List = []
      }
    },
    getIpaddressv4List(query) {
      if (query !== '' && query) {
        this.searchLoading = true
        setTimeout(() => {
          this.searchLoading = false
          assetipApi.search(1, 10, { 'ipaddressv4': query }).then(response => {
            this.ipaddressv4List = response.data.rows.filter(item => {
              return item.ipaddressv4.toLowerCase().indexOf(query.toLowerCase()) > -1
            })
          })
        }, 200)
      } else {
        this.ipaddressv4List = []
      }
    },

    getRemarkList(query) {
      if (query !== '' && query) {
        this.searchLoading = true
        setTimeout(() => {
          this.searchLoading = false
          assetipApi.search(1, 10, { 'remark': query }).then(response => {
            this.remarkList = response.data.rows.filter(item => {
              return item.remark.toLowerCase().indexOf(query.toLowerCase()) > -1
            })
          })
        }, 200)
      } else {
        this.remarkList = []
      }
    },

    getProjectInfoList(query) {
      if (query !== '' && query) {
        this.searchLoading = true
        setTimeout(() => {
          this.searchLoading = false
          projectinfoApi.search(1, 10, { 'projectname': query }).then(response => {
            this.projectinfoList = response.data.rows.filter(item => {
              return item.projectname.toLowerCase().indexOf(query.toLowerCase()) > -1
            })
          })
        }, 200)
      } else {
        this.projectinfoList = []
      }
    },
    handleDeleteAll() {
      if (this.multipleSelection && this.multipleSelection.length) {
        this.$confirm('此操作将永久删除已选记录信息, 包括 [资产ip, 资产端口, 主机信息, 位置信息, 漏洞检测结果, web信息, url信息], 是否继续?', '警告', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning',
          center: true
        }).then(() => {
          const ids = []
          for (let i = 0; i < this.multipleSelection.length; i++) {
            ids.push(this.multipleSelection[i].id)
          }
          assetipApi.deleteAllByIds(ids).then(response => {
            this.$message({
              message: response.message,
              type: (response.flag ? 'success' : 'error')
            })
            if (response.flag) {
              this.fetchData() // 刷新数据
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      } else {
        this.$message({
          message: '^_^至少选择一条记录哦~',
          type: 'info'
        })
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val
    },
    handleDownload() {
      if (this.multipleSelection && this.multipleSelection.length) {
        this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['项目信息', 'ipv4', 'ipv6', '安全检测白名单', '资产提醒白名单', '发现时间', '下线时间', '备注']
          const filterVal = ['projectinfoid', 'ipaddressv4', 'ipaddressv6', 'checkwhitelist', 'assetnotifywhitelist', 'activetime', 'passivetime', 'remark']
          const list = this.multipleSelection
          for (let i = 0; i < list.length; i++) {
            // list[i].ipaddressv4 = this.list[i].ipaddressv4.split(' : ')[0]
            list[i].activetime = dateformat(list[i].activetime)
            list[i].passivetime = dateformat(list[i].passivetime)
            list[i].checkwhitelist = list[i].checkwhitelist ? '是' : ''
            list[i].assetnotifywhitelist = list[i].assetnotifywhitelist ? '是' : ''
          }
          const data = this.formatJson(filterVal, list)
          excel.export_json_to_excel({
            header: tHeader,
            data,
            filename: this.filename
          })
          this.$refs.multipleTable.clearSelection()
          this.downloadLoading = false
        })
        this.fetchData()
      } else {
        this.$message({
          message: '^_^至少选择一条记录哦~',
          type: 'info'
        })
      }
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
    },
    resetForm(formName) { // 清空搜索表单
      this.$refs[formName].resetFields()
      this.searchMap = {}
      this.remarkList = []
      this.ipaddressv4List = []
      this.ipaddressv6List = []
      this.$message({
        message: '已清空搜索条件',
        type: 'info'
      })
    },
    handleSizeChange(val) {
      this.pageSize = val
      this.fetchData()
    },
    formatBoolean(cellValue) {
      return cellValue ? '是' : ''
    },
    fetchData() {
      this.listLoading = true
      assetipApi.search(this.currentPage, this.pageSize, this.searchMap).then(response => {
        this.list = response.data.rows
        this.total = response.data.total
      })
      this.listLoading = false
    },
    handleSave() {
      assetipApi.update(this.id, this.pojo).then(response => {
        this.$message({
          message: response.message,
          type: (response.flag ? 'success' : 'error')
        })
        if (response.flag) { // 如果成功
          this.fetchData() // 刷新列表
        }
      })
      this.closeDialogForm() // 关闭窗口
    },
    handleEdit(id) {
      this.id = id
      this.dialogFormVisible = true // 打开窗口
      if (id !== '') { // 修改
        assetipApi.findById(id).then(response => {
          if (response.flag) {
            this.pojo = response.data
            this.projectname = this.pojo.projectname
            // projectinfoApi.findById(this.pojo.projectinfoid).then((response) => {
            //   if (response.flag) {
            //     this.projectname = response.data.projectname
            //   }
            // })
          }
        })
      } else {
        this.pojo = {} // 清空数据
      }
    },
    handleDelete(id) {
      this.$confirm('此操作将永久删除已选记录信息, 包括 [资产ip, 资产端口, 主机信息, 位置信息, 漏洞检测结果, web信息, url信息], 是否继续?', '警告', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
        center: true
      }).then(() => {
        assetipApi.deleteById(id).then(response => {
          this.$message({
            message: response.message,
            type: (response.flag ? 'success' : 'error')
          })
          if (response.flag) {
            this.fetchData() // 刷新数据
          }
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    }
  }
}
</script>

<style>
.el-drawer__body {
   overflow: auto;
}
 .text {
    font-size: 14px;
    line-height: 20px
  }
</style>
