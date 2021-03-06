<template>
  <div
    :class="{'has-logo':showLogo}"
    :style="{ backgroundColor: settings.sideTheme === 'theme-dark' ? variables.menuBg : variables.menuLightBg }"
  >
    <logo v-if="showLogo" :collapse="isCollapse" />
    <el-scrollbar :class="settings.sideTheme" wrap-class="scrollbar-wrapper">
      <el-menu
        :default-active="activeMenu"
        :collapse="isCollapse"
        :background-color="settings.sideTheme === 'theme-dark' ? variables.menuBg : variables.menuLightBg"
        :text-color="settings.sideTheme === 'theme-dark' ? variables.menuText : 'rgba(0,0,0,.65)'"
        :unique-opened="true"
        :active-text-color="settings.theme"
        :collapse-transition="false"
        mode="vertical"
      >
        <sidebar-item
          v-for="route in sidebarRouters"
          :key="route.path"
          :item="route"
          :base-path="route.path"
        />
        <!-- <sidebar-item
          v-for="(route, index) in sidebarRouters"
          :key="route.path  + index"
          :item="route"
          :base-path="route.path"
        /> -->
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";
import Logo from "./Logo";
import SidebarItem from "./SidebarItem";
import variables from "@/assets/styles/variables.scss";

export default {
  data() {
    return {
      sidebarRouters: [
        {
          name: "System",
          path: "/system",
          hidden: false,
          redirect: "noRedirect",
          component: "Layout",
          alwaysShow: true,
          meta: { title: "系统管理", icon: "system", noCache: false },
          children: [
            {
              name: "User",
              path: "user",
              hidden: false,
              component: "system/user/index",
              meta: { title: "用户管理", icon: "user", noCache: false }
            },
            {
              name: "Role",
              path: "role",
              hidden: false,
              component: "system/role/index",
              meta: { title: "角色管理", icon: "peoples", noCache: false }
            },
            {
              name: "Menu",
              path: "menu",
              hidden: false,
              component: "system/menu/index",
              meta: { title: "菜单管理", icon: "tree-table", noCache: false }
            },
            {
              name: "Dept",
              path: "dept",
              hidden: false,
              component: "system/dept/index",
              meta: { title: "部门管理", icon: "tree", noCache: false }
            },
            {
              name: "Post",
              path: "post",
              hidden: false,
              component: "system/post/index",
              meta: { title: "岗位管理", icon: "post", noCache: false }
            },
            {
              name: "Dict",
              path: "dict",
              hidden: false,
              component: "system/dict/index",
              meta: { title: "字典管理", icon: "dict", noCache: false }
            },
            {
              name: "Config",
              path: "config",
              hidden: false,
              component: "system/config/index",
              meta: { title: "参数设置", icon: "edit", noCache: false }
            },
            {
              name: "Notice",
              path: "notice",
              hidden: false,
              component: "system/notice/index",
              meta: { title: "通知公告", icon: "message", noCache: false }
            },
            {
              name: "Log",
              path: "log",
              hidden: false,
              redirect: "noRedirect",
              component: "ParentView",
              alwaysShow: true,
              meta: { title: "日志管理", icon: "log", noCache: false },
              children: [
                {
                  name: "Operlog",
                  path: "operlog",
                  hidden: false,
                  component: "monitor/operlog/index",
                  meta: { title: "操作日志", icon: "form", noCache: false }
                },
                {
                  name: "Logininfor",
                  path: "logininfor",
                  hidden: false,
                  component: "monitor/logininfor/index",
                  meta: {
                    title: "登录日志",
                    icon: "logininfor",
                    noCache: false
                  }
                }
              ]
            }
          ]
        },
        {
          name: "Monitor",
          path: "/monitor",
          hidden: false,
          redirect: "noRedirect",
          component: "Layout",
          alwaysShow: true,
          meta: { title: "系统监控", icon: "monitor", noCache: false },
          children: [
            {
              name: "Online",
              path: "online",
              hidden: false,
              component: "monitor/online/index",
              meta: { title: "在线用户", icon: "online", noCache: false }
            },
            {
              name: "Job",
              path: "job",
              hidden: false,
              component: "monitor/job/index",
              meta: { title: "定时任务", icon: "job", noCache: false }
            },
            {
              name: "Druid",
              path: "druid",
              hidden: false,
              component: "monitor/druid/index",
              meta: { title: "数据监控", icon: "druid", noCache: false }
            },
            {
              name: "Server",
              path: "server",
              hidden: false,
              component: "monitor/server/index",
              meta: { title: "服务监控", icon: "server", noCache: false }
            },
            {
              name: "Cache",
              path: "cache",
              hidden: false,
              component: "monitor/cache/index",
              meta: { title: "缓存监控", icon: "redis", noCache: false }
            }
          ]
        },
        {
          name: "Tool",
          path: "/tool",
          hidden: false,
          redirect: "noRedirect",
          component: "Layout",
          alwaysShow: true,
          meta: { title: "系统工具", icon: "tool", noCache: false },
          children: [
            {
              name: "Build",
              path: "build",
              hidden: false,
              component: "tool/build/index",
              meta: { title: "表单构建", icon: "build", noCache: false }
            },
            {
              name: "Gen",
              path: "gen",
              hidden: false,
              component: "tool/gen/index",
              meta: { title: "代码生成", icon: "code", noCache: false }
            },
            {
              name: "Swagger",
              path: "swagger",
              hidden: false,
              component: "tool/swagger/index",
              meta: { title: "系统接口", icon: "swagger", noCache: false }
            }
          ]
        },
        {
          name: "Http://ruoyi.vip",
          path: "http://ruoyi.vip",
          hidden: false,
          component: "Layout",
          meta: { title: "若依官网", icon: "guide", noCache: false }
        }
      ]
    };
  },
  components: { SidebarItem, Logo },
  computed: {
    ...mapState(["settings"]),
    ...mapGetters(["sidebarRouters", "sidebar"]),
    activeMenu() {
      const route = this.$route;
      const { meta, path } = route;
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu;
      }
      return path;
    },
    showLogo() {
      return this.$store.state.settings.sidebarLogo;
    },
    variables() {
      return variables;
    },
    isCollapse() {
      return !this.sidebar.opened;
    }
  }
};
</script>
