<template>
  <div id="navSidebar" class="nav navsidebar">
    <div class="vstack height-full justify-space-between height-full">
      <div class="vstack height-auto">
        <div class="width-full text-align-left padding-s">
          <div class="vstack margin-none">
            <router-link v-for="item in this.pages" v-bind:key="item.id" class="tabbar-vertical-item" v-bind:to="item.route">{{ item.label }}</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NavSidebar',
  data() {
    return {
      pages: [
        {
          id: 'home',
          label: 'Home',
          route: '/'
        },
        {
          id: 'about',
          label: 'About',
          route: '/about'
        },
        {
          id: 'projects',
          label: 'Projects',
          route: '/projects'
        }
      ]
    };
  },
  methods: {
    isMobile() {
      if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        return true
      } else {
        return false
      }
    },
    revealNavMenuButton() {
      document.getElementById("navbarMenuButton").style.visibility = "visible";
      document.getElementById("navbarMenuButton").style.display = "inline-block";
    },
    hideNavMenuButton() {
      document.getElementById("navbarMenuButton").style.visibility = "hidden";
      document.getElementById("navbarMenuButton").style.display = "none";
    },
    revealNavMenu() {
      document.getElementById("navbarMenu").style.visibility = "visible";
      document.getElementById("navbarMenu").style.display = "inline-block";
    },
    hideNavMenu() {
      document.getElementById("navbarMenu").style.visibility = "hidden";
      document.getElementById("navbarMenu").style.display = "none";
    },
    toggleNavMenu() {
      let navbarMenu = document.getElementById("navbarMenu");
      if (navbarMenu != null) {
        if (navbarMenu.style.visibility === "hidden") {
          this.revealNavMenu();
        } else {
          this.hideNavMenu();
        }
      }
    },
    revealNavLinks() {
      document.getElementById("navbarLinks").style.visibility = "visible";
      document.getElementById("navbarLinks").style.display = "inline-block";
    },
    hideNavLinks() {
      document.getElementById("navbarLinks").style.visibility = "hidden";
      document.getElementById("navbarLinks").style.display = "none";
    }
  }
}
</script>

<style scoped>
.navsidebar {
  border-right: var(--CARD-BORDER-WIDTH) solid var(--CARD-BORDER-COLOR);
}

#navSidebar {
  visibility: visible;
  display: inline-flex;
}
@media only screen and (max-width: 1024px) {
  #navSidebar {
    visibility: hidden;
    display: none;
  }
}
</style>