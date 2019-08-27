<template>
  <el-container class="my-container">

    <el-aside :width="isCollapse?'64px':'200px'">
      <!-- logo -->
      <div class="logo" :class="{miniLogo:isCollapse}"></div>
      <!-- 导航菜单 -->
      <el-menu
        :default-active="$route.path"
        class="el-menu-vertical-demo"
        background-color="#002033"
        text-color="#fff"
        active-text-color="#ffd04b"
        :collapse="isCollapse"
       :collapse-transition="false"
       router
      >
        <el-menu-item index="/">
          <i class="el-icon-s-home"></i>
          <span slot="title">首页</span>
        </el-menu-item>
        <el-menu-item index="/article">
          <i class="el-icon-document"></i>
          <span slot="title">内容管理</span>
        </el-menu-item>
        <el-menu-item index="/image">
          <i class="el-icon-picture"></i>
          <span slot="title">素材管理</span>
        </el-menu-item>
        <el-menu-item index="publish">
          <i class="el-icon-s-promotion"></i>
          <span slot="title">发布文章</span>
        </el-menu-item>
        <el-menu-item index="/comment">
          <i class="el-icon-chat-dot-round"></i>
          <span slot="title">评论管理</span>
        </el-menu-item>
        <el-menu-item index="/fans">
          <i class="el-icon-present"></i>
          <span slot="title">粉丝管理</span>
        </el-menu-item>
        <el-menu-item index="/setting">
          <i class="el-icon-setting"></i>
          <span slot="title">个人设置</span>
        </el-menu-item>
      </el-menu>
    </el-aside>
    <el-container>
      <el-header>
        <span class="icon el-icon-s-fold"  @click="toggleAside()"></span>
        <span class="text">江苏传智播客科技教育有限公司</span>
        <!-- 下拉菜单组件 -->
        <el-dropdown class="my-dropdown" @command="clickItem">
          <span class="el-dropdown-link">
            <img class="avatar" :src="photo" alt="">
            <span class="name"> {{name}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <!-- vue基础知识  插槽 -->
          <el-dropdown-menu slot="dropdown" >
            <el-dropdown-item icon="el-icon-setting" command="setting">个人设置</el-dropdown-item>
            <el-dropdown-item icon="el-icon-unlock" command="logout">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-header>
      <el-main>
          <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
import store from '../../store/store'
// import router from '../../router/index'
export default {

  data () {
    return {
      isCollapse: false,
      name: '',
      photo: ''
    }
  },
  created () {
    const uers = store.getUser()
    this.name = uers.name
    this.photo = uers.photo
  },
  methods: {
    toggleAside () {
      this.isCollapse = !this.isCollapse
    },
    setting () {
      this.$router.push('/setting')
    },
    logout () {
      store.delUser()
      this.$router.push('/login')
    },
    clickItem (command) {
      this[command]()
    }

  }
}
</script>

<style lang="less" scoped>
.my-container{
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
   .el-aside{
       background: #002033;
        .logo{
            width: 100%;
            height: 60px;
            background: #002244 url('../../assets/imgs/logo_admin.png') no-repeat center/140px auto;
        }
        .miniLogo{
            width: 100%;
            height: 60px;
            background: #002244 url('../../assets/imgs/logo_admin_01.png') no-repeat center/40px auto;
        }
        .el-menu{
          border: 0px;
        }
   }
   .el-header{
       border-bottom: 1px solid #dddddd;
       line-height: 60px;
       .icon{
           font-size: 24px;
           vertical-align: middle;
       }
       .text{
           vertical-align: middle;
       }
       .my-dropdown{
           float: right;
           .avatar{
               width: 30px;
               height: 30px;
               vertical-align: middle;
           }
           .name{
               color:#333;
               font-weight: bold;
              vertical-align: middle;
           }
       }
   }
   .el-main{
    padding: 0;
   }
}
</style>
