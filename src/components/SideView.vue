<template>
  <el-menu v-model="activeIndex" class="el-menu-vertical-demo" style="width: 120px;">
    <el-menu-item index="1" @click="route" style="" class="item">
      <el-icon class="large-icon">
        <Notebook />
      </el-icon>
      <el-button link :type="'primary'" class="large-button">图书信息</el-button>
    </el-menu-item>

    <el-menu-item
        v-if="!adminStore.isAdmin"
        index="2"
        @click="router.push('/borrow')"
        class="item"
    >
      <el-icon class="large-icon">
        <Search />
      </el-icon>
      <el-button link :type="'primary'" class="large-button">借阅记录</el-button>
    </el-menu-item>

    <el-menu-item index="3" @click="router.push('/user')" class="item">
      <el-icon class="large-icon">
        <User />
      </el-icon>
      <el-button link :type="'primary'" class="large-button">个人信息</el-button>
    </el-menu-item>

    <el-menu-item
        v-if="adminStore.isAdmin"
        index="4"
        @click="router.push('/reader')"
    >
      <el-icon class="large-icon">
        <Setting />
      </el-icon>
      <el-button link :type="'primary'" class="large-button">用户管理</el-button>
    </el-menu-item>

    <el-menu-item index="5" @click="logout">
      <el-icon class="large-icon">
        <SwitchButton />
      </el-icon>
      <el-button link :type="'danger'" class="large-button">退出登录</el-button>
    </el-menu-item>
  </el-menu>
</template>

<style scoped>
.large-icon {
  width: 120px;
  height: 120px;
  font-size: 40px;
}

.large-button {
  font-size: 20px;
  padding: 10px;
}
.item{
margin-bottom:30px
}
</style>

<script lang="ts" setup>
import {Notebook, Search, Setting, SwitchButton, User} from "@element-plus/icons-vue";

import {ref} from "vue";

const activeIndex = ref("1");

import {useRouter} from "vue-router";
import {useTokenStore} from "@/stores/token";
import {useReaderStore} from "@/stores/reader";
import {useAdminStore} from "@/stores/admin";
import {logoutService} from "@/methods/logout.js";
import {ElMessage} from "element-plus";


const adminStore = useAdminStore();
const tokenStore = useTokenStore();
const readerStore = useReaderStore();

const router = useRouter();

const logout = async () => {
  await logoutService();
  ElMessage.success("已退出登录!")
  // 退出后清除token、reader和admin信息
  tokenStore.setToken(null);
  readerStore.clearReader();
  adminStore.clearAdmin();
  await router.push("/login");
};

const route = () => {
  if (adminStore.isAdmin) {
    router.push("/admin/book");
  } else {
    router.push("/book");
  }
};
</script>
