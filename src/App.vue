<template>
  <el-container>
    <el-header>
      <div class="el-header__container">
        <div class="header-logo">
          <img src="./assets/logo.png" alt="logo" />
          <div>Orbiter NetState</div>
        </div>
        <el-menu :default-active="navActive" class="header-navs" mode="horizontal" :router="true">
          <template v-for="(item, index) in navs" :key="index">
            <el-menu-item v-if="item.meta.navShow" :index="item.path">
              {{ item.name }}
            </el-menu-item>
          </template>
        </el-menu>
      </div>
    </el-header>

    <el-container>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default defineComponent({


  setup() {
    const route = useRoute()
    const router = useRouter()

    const state = reactive({
      navs: router.getRoutes(),
      navActive: '/',
    })
    watch(
      () => route.path,
      (nv) => {
        state.navActive = nv
      }
    )

    return {
      ...toRefs(state),
    }
  },
})
</script>

<style lang="scss" scoped>
$header-height: 60px;
$aside-width: 200px;
$max-width: 1600px;

.el-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: $header-height;
  background: white;
  border-bottom: 1px solid #{var(--el-border-color-base)};
  padding: 0;
  z-index: var(--el-index-popper);

  .el-header__container {
    max-width: $max-width;
    display: flex;
    flex-direction: row;
    align-items: center;
    height: $header-height;
    width: 100%;
    margin: 0 auto;
    padding: 0 20px;
    box-sizing: border-box;
  }

  .header-logo {
    display: flex;
    flex-direction: row;
    align-items: center;
    // justify-content: center;
    margin: 12px 0;
    font-size: 22px;
    font-weight: bold;

    img {
      width: 40px;
      margin-right: 12px;
    }
  }

  .header-navs {
    flex: 1;
    margin-left: 36px;
    background-color: transparent;
  }
}
</style>
