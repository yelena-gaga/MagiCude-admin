<template>
  <div style="padding:100px 200px 0">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span><b>魔方-MagiCude </b></span>
        <el-link :underline="false" @click="handleDrawer()">
          <b>v2.0.28</b>
        </el-link>
        <el-link style="float: right;" href="https://github.com/er10yi/MagiCude" target="_blank">GitHub</el-link>
      </div>
      <div class="text">
        <ol>
          <li>高效获取服务器IP, 端口信息, web信息, url链接, 漏洞等资产信息</li>
          <li>对资产进行有效的安全管理</li>
          <li>建立漏洞wiki并自动闭环漏洞, 减少安全部门与业务部门的沟通</li>
          <li>即时漏洞提醒</li>
          <li>定时发送资产报告和漏洞报告</li>
          <li>及时发送资产信息和处理结果给负责人进行整改并自动确认整改完成</li>
          <li>有效获取DHCP的办公机IP与用户关联</li>
          <li>丰富的图表统计</li>
          <li>POC框架</li>
        </ol>
      </div>
      <div class="item">
        Copyrights © 2018-{{ getYear() }}
        <el-link type="info" href="https://github.com/er10yi" target="_blank">贰拾壹.</el-link>All Rights Reserved.
      </div>
    </el-card>

    <!--更新日志显示 -->
    <el-drawer title="ChangeLog" :visible.sync="drawer" direction="rtl" size="60%">

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>v2.0.28 - 20201123</b></span>
          <div class="text">
            <ol>
              <li>前端主机名改成：主机/域名</li>
              <li>nmap域名扫描，域名会直接新增到主机/域名中</li>
              <li>任务，[cron任务删除]移到类型列</li>
              <li>修复部分bug，并优化部分前端和后端代码</li>
              <li>移除v2.0.20中[启动任务前，发一次agent心跳包监控，防止agent掉线漏任务]更新</li>
              <li>增加最佳实践文档</li>
              <li>侧边栏增加Logo显示，统一侧边栏菜单图标样式</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>v2.0.20 - 20201028</b></span>
          <div class="text">
            <ol>
              <li>优化多表数据显示逻辑</li>
              <li>优化任务和子任务显示，默认不显示子任务，无子任务不能打开子任务的Drawer，子任务数量移到任务名称前显示</li>
              <li>任务，cron任务删除移到类型列</li>
              <li>任务及web信息增加 增加显示，可以按需选择显示的列</li>
              <li>任务新增渐变色的进度百分比显示</li>
              <li>任务编辑，新增cron表达式模拟解析</li>
              <li>优化侧边栏，部分菜单二级收纳</li>
              <li>项目信息添加负责人显示，可直接新增部门和负责人</li>
              <li>检测结果新增ip、端口、漏洞名称方式搜索（三者是或的关系）</li>
              <li>web信息新增ip、端口方式搜索（两者是或的关系）</li>
              <li>任务端口，新增任务名称方式搜索，用于导出单次任务的端口扫描结果（暂缓方式，后续优化）</li>
              <li>资产ip，新增统计列，方便查看当前ip的漏洞数量和端口数量</li>
              <li>资产ip，新增漏洞数目</li>
              <li>批量导入-项目信息ip，移除如果ip不存在，会新增</li>
              <li>批量导入-ip端口，增加无端口信息的ip导入（避免使用新增C段甚至B段的格式，会导致整个C段或B段的ip都导入数据库）</li>
              <li>批量导入-ip主机名，修复主机名重复时，导入失败问题</li>
              <li>nmap模式增加域名扫描、-sn(ping)结果解析</li>
              <li>启动任务前，发一次agent心跳包监控，防止agent掉线漏任务</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>v2.0.2 - 20201011</b></span>
          <div class="text">
            <ol>
              部署脚本：
              <li>日志提示增加字体颜色</li>
              <li>增加卸载脚本</li>
              <li>center部署无需本地手动修改前端api地址和agent.yml中的地址，只需部署时选择即可</li>
              <li>agent分布式部署无需本地手动修改agent.yml中的地址，只需部署时输入部署center的ip地址即可</li>
              <li>优化判断依赖是否存在的方式</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>v2.0.1 - 20201009</b></span>
          <div class="text">
            <ol>
              部署脚本：
              <li>增加部署脚本执行目录判断（issues #3 建议判断或约束一下安装脚本执行目录）</li>
              <li>增加agent.yml内容是否已经修改判断</li>
              <li>增加center和agent所需文件判断</li>
              <li>增加前端api地址是否修改判断</li>
              <li>增加操作系统判断</li>
              <li>修复多次重新部署的时候，环境变量会写入多次</li>
              <li>优化脚本日志输出，屏蔽非脚本错误输出，优化所有脚本</li>
              <li>Python升级到3.9.0</li>
              <li>部署脚本运行结束时，增加信息和错误提示</li>
              <li>移除agentDependency，nmap更新到7.90</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>正式版 v2.0 - 20200925</b></span>
          <div class="text">
            <ol>
              正式版发布，同时开源
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>公测版 v1.3.25 - 20200913</b></span>
          <div class="text">
            <ol>
              前端：
              <li>修复 子任务Drawer，调整分页无效的问题</li>
              <li>优化 用code标签显示代码</li>
              <li>新增 插件页面新增插件代码高亮，python3的selfd插件可前端新增(已知bug，第一次进入插件页面，新增或编辑需要打开两次代码高亮插件才会正常显示，第二次保存才能正常添加代码)</li>
              <li>新增 设置-批量导入，新增python3的selfd插件配置导入，主要用于python3的selfd插件配置及代码导入</li>
              <li>新增 新增各个类型任务的demo</li>
              后端：
              <li>修复 nse类型任务，获取状态时不显示PID信息</li>
              <li>修复 nse、selfd、httpp任务，资产库无对应的信息时，任务也会发给agent</li>
              <li>修复 无资产ip时，目标ip为ipNoPort，unknownPortSerVer，ipAllPort，任务也会发给agent</li>
              <li>修复 修复插件未设置服务和版本时，启动任务会开启所有插件 </li>
              <li>优化 优化新增自定义插件，插件核心增加Jep方式调用python代码，可前端直接新增python3的selfd自定义类型的插件（重要更新，TDL的一半，优化新增自定义插件并丰富插件）</li>
              <li>优化 优化全部批量删除</li>
              <li>新增 新增python3的selfd插件配置导入接口</li>
              <li>新增 agent新增python插件import检测，如模块未安装，则会自动安装</li>
              <li>新增 插件配置新增http、dns辅助验证，用于插件辅助验证无回显漏洞（TDL 增加无回显插件检测）</li>
              <li>新增 增加HTTP json插件解析</li>
              <li>修改 随机获取ua的方式修改，存入redis进行随机</li>
              数据库：
              <li>新增 插件配置新增plugincode字段，用于保存插件代码</li>
              <li>新增 插件配置新增validatetype字段，用于开启插件http辅助验证或dns辅助验证</li>
              配置文件：
              <li>新增 agent.yml新增jep节点，用于保存JEP本地库的绝对路径</li>
              <li>新增 agent.yml新增center节点，用于插件辅助验证时传递http接口和dns的ip地址</li>
              部署脚本:
              <li>增加 新增Python3.8编译安装</li>
              <li>移除 移除jre14，修改成jdk15（JEP编译需要）</li>
              文档：
              <li>优化 优化快速上手及说明文档</li>
              <li>增加 新增Readme文档</li>
              <li>增加 新增插件编写说明</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>公测版 v1.2.2 - 20200722</b></span>
          <div class="text">
            <ol>
              <li>优化center部署脚本，增加判断是否已经运行过部署脚本，避免误操作二次运行部署脚本（如果已经部署，则需要用户交互），优化docker服务运行状态检测</li>
              <li>优化initDataAndStart.sh，增加脚本复制和执行成功的判断</li>
              <li>修改center.yml，token过期时间设置成一小时</li>
              <li>优化任务插件显示，将插件启用状态体现到操作中，优化插件启用禁用</li>
              <li>新增子任务展开Drawer</li>
              <li>about页面版本上增加changelog的Drawer，增加更新日志及TDL</li>
              <li>前端依赖更新到最新</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>公测版 v1.2.1 - 20200715</b></span>
          <div class="text">
            <ol>
              前端：
              <li>增加about页面</li>
              <li>增加菜单栏显示与隐藏的Collapse，涉及的页面：任务、资产ip、资产端口、主机、检测结果、web信息、web信息和url</li>
              <li>优化资产ip，去除编辑页面显示ip所有信息，修改成Drawer方式显示，并增加url超链接</li>
              <li>优化资产端口，去除编辑页面显示端口所有信息，修改成Drawer方式显示，并增加http和https链接打开，增加url超链接</li>
              <li>优化新建任务的数量前端样式，将input修改成number模式</li>
              <li>优化检测类型任务，将input修改成单选模式</li>
              <li>优化web信息中ip的显示，增加打开http和https链接的Popover</li>
              <li>首页四个折线图修改成显示最新数据的10%</li>
              <li>修改设置-通知设置中的风险等级编辑样式，从input改成单选模式</li>
              <li>修改cron表达式解析结果的样式，改成表格</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>公测版 v1.2.0 - 20200706</b></span>
          <div class="text">
            <ol>
              公开公测开启
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>内测版 v1.1 - 20200615</b></span>
          <div class="text">
            <ol>
              <li>优化部署步骤，修改部署脚本成一键部署</li>
              <li>部署脚本增加系统源修改判断和docker镜像源修改判断（需要用户交互）</li>
              <li>部署脚本增加判断，提高安装部署成功率</li>
              <li>前端增加web信息功能，后端优化部分代码</li>
              <li>文档增加Q/A章节、微信公众号、项目的目录结构说明，为公测做准备</li>
              <li>增加公测宣传文档</li>
              <li>增加magicude执行脚本，便于查看系统状态及操作系统（0624）</li>
            </ol>
          </div>
        </div>
      </el-card>

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>内测版 v1.0 - 20200528</b></span>
          <div class="text">
            <ol>
              公开内测开启
            </ol>
          </div>
        </div>
      </el-card>
    </el-drawer>
  </div>
</template>
<script>
export default {
  data() {
    return {
      drawer: false
    }
  },
  methods: {
    handleDrawer() {
      this.drawer = true
    },
    handleDrawerClose() {
      this.drawer = false
    },
    getYear() {
      var now = new Date()
      this.year = now.getFullYear()
      return this.year
    }
  }
}
</script>
<style>
.text {
  font-size: 15px;
  line-height: 28px;
}
.item {
  font-size: 13px;
  text-align: center;
}
.el-drawer__body {
  overflow: auto;
}
</style>
