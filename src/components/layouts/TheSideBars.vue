<template>
  <div id="app">
    <div id="view" :class="[{'collapsed' : collapsed}]">
      <!-- <router-view/> -->
    </div>
    <sidebar-menu
      class="sidebar bg-primary"
      :menu="menu"
      :collapsed="collapsed"
      @item-click="onItemClick"
      @collapse="onCollapse"
    />
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      menu: [
        {
          header: true,
          // title: "Getting Started" <i class="fa-thin fa-paper-plane"></i>
        },
        {
          href: "/customer",
          title: "Customer",
          icon: "fa fa-list-ul"
        },
        {
          href: "/employee",
          title: "Employee",
          icon: "fa fa-list-ul"
        },
        {
          href: "/admin",
          title: "Admin",
          icon: "fa fa-list-ul"
        },
        

        
      ],
      collapsed: true
    };
  },
  methods: {
    onItemClick(e, i) {
      console.log("onItemClick");
    },
    onCollapse(c) {
      console.log("onCollapse");
      this.collapsed = c;
    }
  }
};
</script>

<style>
#view {
  padding-left: 350px;
}
#view.collapsed {
  padding-left: 50px;
}

.sidebar.v-sidebar-menu .vsm-arrow:after {
  content: "\f105";
  font-family: "FontAwesome";
}
.sidebar.v-sidebar-menu .collapse-btn:after {
  content: "\f07e";
  font-family: "FontAwesome";
}
</style>