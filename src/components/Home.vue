<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/heima.png" alt />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 左侧菜单栏区域 -->
        <el-menu
          background-color="#323744"
          text-color="#fff"
          active-text-color="#389aff"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu :index="item.id+''" v-for="item in menulist" :key="item.id">
            <template slot="title">
              <i :class="iconsObj[item.id]" class="icons"></i>
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/'+ subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/'+ subItem.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容区域 -->
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
      menulist: [],
      // 左侧菜单icon图标
      iconsObj: {
        125: 'el-icon-s-custom',
        103: 'el-icon-coin',
        101: 'el-icon-shopping-bag-2',
        102: 'el-icon-s-order',
        145: 'el-icon-s-data'
      },
      // 左侧菜单是否缩进
      isCollapse: false,
      // 菜单激活状态
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      // console.log(res.data)
      if (res.meta.status !== 200) return this.$message.error('获取菜单列表失败')
      this.menulist = res.data
    },
    // 点击，切换左侧菜单的折叠与展开
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保持左侧二级菜单的激活状态
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.el-header {
  background-color: #373c41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 0;
  > div {
    display: flex;
    align-items: center;
    span {
      font-size: 20px;
      color: #fff;
      margin-left: 20px;
    }
  }
}

.el-aside {
  background-color: #323744;
  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #eaedf2;
}

.home-container {
  height: 100%;
}

.icons {
  margin-right: 10px;
}

.toggle-button {
  font-size: 10px;
  text-align: center;
  line-height: 24px;
  color: #fff;
  letter-spacing: 0.3em;
  cursor: pointer;
}
</style>
