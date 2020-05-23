<template>
  <el-container>
    <!-- 头部 -->
    <el-header>
      <div style="padding-left: 20px">
        <img src="../assets/logo.png" alt />
        <span>Alex Lxy</span>
      </div>
      <el-button @click="logout">退出</el-button>
    </el-header>
    <!-- 主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <!-- <div class="toggle-button" @click="togleCollapse">|||</div> -->
        <el-menu  :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath" background-color="#3E7FEE" text-color="#fff" active-text-color="#409FFF">
           <!-- :unique-opened="true"->只允许展开一个菜单 -->
           <!-- :collapse-transition="false" -> 关闭动画 -->
           <!-- router -> 导航开启路由模式 -->
          <!-- 一级菜单  -->
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id" >
            <!-- 一级菜单的模板区域 -->
            <template slot="title">
              <i :class="iconObj[item.id]"></i>
              <span>{{ item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
              <!-- 导航开启路由模式：
                将index值作为导航路由 -->
              <!-- 二级菜单的模板区域 -->
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 内容主体 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menuList: [],
      iconObj: {
        '125': 'iconfont icon-user',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      // 默认不折叠
      isCollapse: false,
      // 被激活导航地址
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      // 清空token
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取请求菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
      // console.log(res)
    },
    // 菜单的折叠与展开
    togleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保存连接的激活地址
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
    }
  }
}
</script>

<style lang="less" scoped>
.el-container {
  height: 100%;
}
.el-header {
  // background-color: #34384B;
  border-bottom:1px solid rgba(52, 56, 75, 0.25) ;
  box-shadow: 0px 2px 10px 0px rgba(52, 56, 75, 0.25);
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #3E7FEE;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    img {
      height: 40px;
    }
    span {
      margin-left: 15px;
    }
  }
}
.el-aside {
  background-color: #3E7FEE;
  padding-left: 10px;
  .el-menu {
    border: none;
    // .is-active{
    //   background-color: #FFFFFF;;
    // }
  }
  /deep/ .el-submenu.is-active .el-submenu__title{
    background-color: #FFFFFF!important;
    color: #3E7FEE!important;
    border-radius:10px 0px 0px 10px;
    height: 50px;
    .el-aside .el-submenu__title i{
       color: #3E7FEE!important;
    }
  }
  .el-submenu__title i{
    color: #FFFFFF;
  }
  .el-menu-item i{
    color: #FFFFFF;
  }
  .el-menu-item.is-active{
    color: #FFBD62!important;
  }
}
.el-main {
  background-color: #FFFFFF;
}
.iconfont{
  margin-right: 10px;
}
.toggle-button {
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  // 鼠标放上去变成小手
  cursor: pointer;
}
</style>
