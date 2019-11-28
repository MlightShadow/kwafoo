<template>
  <el-container>
    <el-main style="padding:0px 10px 10px 10px">
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="单页下载" name="single-page">
          <el-row>
            <el-col :span="24">
              <el-input placeholder="请输入地址" v-model="url" class="input-with-select" size="mini">
                <el-button
                  :loading="loading"
                  slot="append"
                  icon="el-icon-search"
                  @click="test"
                >获取资源列表</el-button>
              </el-input>
            </el-col>
          </el-row>
          <br />
          <el-row>
            <el-col :span="12">
              <el-checkbox label="全选" v-model="select_all_status" @change="select_all"></el-checkbox>
              <el-button size="mini" @click="download">下载</el-button>
            </el-col>
            <el-col :span="12">
              <el-input placeholder="请输入链接正则" v-model="regex_link" size="mini" clearable></el-input>
            </el-col>
          </el-row>
          <br />
          <el-row>
            <el-col :span="24">
              <div v-for="(item, index) in list" :key="index">
                <el-checkbox
                  v-model="checked[index]"
                  ref="link"
                  style="overflow: hidden; text-overflow:ellipsis; white-space: nowrap;"
                >{{item}}</el-checkbox>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="多页下载" name="muti-page">多页下载</el-tab-pane>
        <el-tab-pane label="配置管理" name="config">配置管理</el-tab-pane>
        <el-tab-pane label="文件管理" name="file-sys">文件管理</el-tab-pane>
        <el-tab-pane label="关于系统" name="about">
          <system-information></system-information>
        </el-tab-pane>
      </el-tabs>
    </el-main>
    <el-footer></el-footer>
  </el-container>
</template>

<script>
import SystemInformation from './LandingPage/SystemInformation'
import { scout } from './../../core/scout'
export default {
  name: 'landing-page',
  components: { SystemInformation },
  data () {
    return {
      request_info: '',
      checked: [],
      list: [],
      url: '',
      activeName: 'single-page',
      regex_url: '',
      regex_link: '\\S+',
      loading: false,
      select_all_status: false,
      download_list: []
    }
  },
  methods: {
    download () {
      this.download_list = []
      this.checked.map((v, i) => {
        if (v) {
          this.download_list.push(this.list[i])
        }
      })
      console.log(this.download_list)
    },
    select_all () {
      this.checked = this.$refs.link.map(() => this.select_all_status)
    },
    handleClick (tab, event) {
      console.log(tab, event)
    },
    open (link) {
      this.$electron.shell.openExternal(link)
    },
    async test () {
      if (!this.url) {
        this.$message.error({
          showClose: true,
          type: 'error',
          message: '请输入url'
        })
        return
      }

      this.list = []
      this.checked = []
      this.loading = true
      await scout.get(this.url).then(
        response => {
          this.request_info = response.body
        },
        error => {
          this.$message.error({
            showClose: true,
            type: 'error',
            message: error
          })
        }
      )

      if (this.request_info) {
        let match_list = this.request_info.match(
          new RegExp(eval('/(src|href)="' + this.regex_link + '"/'), 'g')
        )

        match_list = match_list.map(s =>
          s
            .replace(new RegExp(/(src|href)=/, 'g'), '')
            .replace(new RegExp(/\"/, 'g'), '')
        )
        if (match_list) {
          this.list.push(...match_list)
        }
      }

      this.loading = false
    }
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}

.box-card {
  width: 480px;
}

.input-with-select .el-input-group__prepend {
  background-color: #fff;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
  font-size: 12px;
}
</style>
