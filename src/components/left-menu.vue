<template>
  <div class="menu" :class="{ 'menu-open': ShowMenu }">
    <h4>Calendar</h4>
    <img @click="ShowMenu = !ShowMenu" src="../assets/img/svg/menu.svg" />
    <hr />
    <transition name="fade">
      <tableMenu class="tableMenu" v-show="ShowMenu" />
    </transition>
  </div>
</template>

<script>
// import components
import tableMenu from "./tableMenu";

export default {
  name: "leftMenu",
  data() {
    return {
      ShowMenu: true,
    };
  },
  components: {
    tableMenu,
  },
};
</script>

<style lang="scss" scoped>
.menu {
  width: 50px;
  min-width: 50px;
  height: 100%;
  background-color: #2f2f2f;
  transition: 0.15s all ease-in-out;
  will-change: width;
  overflow: hidden;
  h4 {
    padding: 5px 0 2px 10px;
    color: #fff;
    font-size: 14px;
    transition: 0.2s;
    opacity: 0;
  }
  hr {
    background-color: #40e0d085;
    margin: 0 5px;
    height: 1px;
    border: none;
  }
  img[src*="menu"] {
    width: 40px;
    padding: 5px;
    transition: 0.15s;
    user-select: none;
    &:hover {
      cursor: pointer;
      opacity: 0.7;
    }
  }
}
.menu-open {
  width: 250px;
  min-width: 250px;
  will-change: width;
  h4 {
    padding: 5px 0 2px 10px;
    color: #fff;
    font-size: 14px;
    opacity: 1;
  }
}
.tableMenu {
  height: 270px;
  margin: 8px;
  overflow: hidden;
}

.fade-enter-active,
.fade-leave-active {
  transition: 0.25s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}
</style>