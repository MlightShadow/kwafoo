<template>
  <el-container>
    <el-main style="padding:0px 10px 10px 10px">
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="页面扫描" name="scanner">
          <el-row>
            <el-col :span="12">
              <el-input placeholder="请输入地址" v-model="url" class="input-with-select" size="mini">
                <el-button slot="append" icon="el-icon-search" @click="test">获取资源列表</el-button>
              </el-input>
            </el-col>
            <el-col :span="12"></el-col>
          </el-row>
          <br />
          <el-row>
            <el-col :span="24">
              <el-input placeholder="url匹配" v-model="regex_url" size="mini" clearable></el-input>
            </el-col>
          </el-row>
          <br />
          <el-row>
            <el-col :span="24">
              <el-input placeholder="链接正则" v-model="regex_link" size="mini" clearable></el-input>
            </el-col>
          </el-row>
          <br />
          <el-row>
            <el-col :span="24">
              <div v-for="(item, index) in list" :key="index">
                <el-checkbox
                  v-model="checked"
                  ref="link"
                  style="overflow: hidden; text-overflow:ellipsis; white-space: nowrap;"
                >{{item}}</el-checkbox>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
        <el-tab-pane label="文件管理" name="third">文件管理</el-tab-pane>
        <el-tab-pane label="关于系统" name="fourth">
          <system-information></system-information>
        </el-tab-pane>
      </el-tabs>
    </el-main>
    <el-footer></el-footer>
  </el-container>
</template>

<script>
import SystemInformation from "./LandingPage/SystemInformation";

export default {
  name: "landing-page",
  components: { SystemInformation },
  data() {
    return {
      list: [],
      url: "",
      activeName: "scanner",
      regex_url: "",
      regex_link: ""
    };
  },
  methods: {
    open(link) {
      this.$electron.shell.openExternal(link);
    },
    handleClick(tab, event) {
      console.log(tab, event);
    },
    test() {
      const request = require("request");
      request.get({ url: this.url }, (error, response, body) => {
        if (!error && response.statusCode === 200) {
          this.list = [];
          console.log(response.body);

          this.list.push(
            ...response.body.match(
              new RegExp(eval('/(src|href)="' + this.regex_link + '"/'), "g")
            )
          );
        }
      });
    }
  }
};
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
}
</style>
