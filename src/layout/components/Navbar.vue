<template>
  <div class="navbar">
    <hamburger id="hamburger-container" :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />

    <div class="hamburger-container reload_icon" @click="Reload">
			<i class="el-icon-refresh-right" style="font-size: 20px; position: relative; top: -12px; font-weight: 600;"></i>
		</div>

    <breadcrumb id="breadcrumb-container" class="breadcrumb-container" />

    <div class="right-menu">
      <template v-if="device!=='mobile'">
        <search id="header-search" class="right-menu-item" />

        <el-tooltip content="全屏缩放" effect="dark" placement="bottom">
          <screenfull id="screenfull" class="right-menu-item hover-effect" />
        </el-tooltip>

        <el-tooltip content="布局设置" effect="dark" placement="bottom">
          <size-select id="size-select" class="right-menu-item hover-effect" />
        </el-tooltip>

      </template>

      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="user.avatar ? baseApi + '/avatar/' + user.avatar : Avatar" class="user-avatar">
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <span style="display:block;" @click="show = true">
            <el-dropdown-item>
              布局设置
            </el-dropdown-item>
          </span>
          <router-link to="/user/center">
            <el-dropdown-item>
              个人中心
            </el-dropdown-item>
          </router-link>
          <span style="display:block;" @click="open">
            <el-dropdown-item divided>
              退出登录
            </el-dropdown-item>
          </span>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'
import Github from '@/components/Github'
import Screenfull from '@/components/Screenfull'
import SizeSelect from '@/components/SizeSelect'
import Search from '@/components/HeaderSearch'
import Avatar from '@/assets/images/avatar.png'

export default {
  components: {
    Breadcrumb,
    Hamburger,
    Screenfull,
    SizeSelect,
    Search,
    Github
  },
  data() {
    return {
      Avatar: Avatar,
      dialogVisible: false
    }
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'device',
      'user',
      'baseApi'
    ]),
    show: {
      get() {
        return this.$store.state.settings.showSettings
      },
      set(val) {
        this.$store.dispatch('settings/changeSetting', {
          key: 'showSettings',
          value: val
        })
      }
    }
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar')
    },
    open() {
      this.$confirm('确定注销并退出系统吗？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.logout()
      })
    },
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        location.reload()
      })
    },
    Reload() {
				let currentView = this.$store.state.tagsView.visitedViews.filter(x => x.path == this.$route.path)[0];
				this.$store.dispatch('tagsView/delView', currentView).then(({
					visitedViews
				}) => {
					// 借助中间页实现不刷新页面的页面刷新
					this.$router.push({
						name: '/middle',
						query: {
							url: currentView.path
						}
					})
				});
			}
  }
}
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0,21,41,.08);

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background .3s;
    -webkit-tap-highlight-color:transparent;

    &:hover {
      background: rgba(0, 0, 0, .025)
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        margin-top: 5px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
.reload_icon{
  padding:15px 10px 0 0; 
  vertical-align: middle; 
  display: inline-block; 
}
</style>
