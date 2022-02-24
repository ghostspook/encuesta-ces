<template>
  <div class="navbar-container d-flex content align-items-center">

    <!-- Nav Menu Toggler -->
    <ul class="nav navbar-nav d-xl-none">
      <li class="nav-item">
        <b-link
          class="nav-link"
          @click="toggleVerticalMenuActive"
        >
          <feather-icon
            icon="MenuIcon"
            size="21"
          />
        </b-link>
      </li>
    </ul>

    <!-- Left Col -->
    <div class="bookmark-wrapper align-items-center flex-grow-1 d-none d-lg-flex">
      <dark-Toggler class="d-none d-lg-block" />
    </div>

    <b-navbar-nav class="nav align-items-center ml-auto" v-if="currentUser">
      <b-nav-item-dropdown
        right
        toggle-class="d-flex align-items-center dropdown-user-link"
        class="dropdown-user"
      >
        <template #button-content>
          <div class="d-sm-flex d-none user-nav">
            <p class="user-name font-weight-bolder mb-0">
              {{ currentUser.displayName }}
            </p>
            <span class="user-status">{{ currentUser.email }}</span>
          </div>
          <b-avatar
            size="40"
            variant="light-primary"
            badge
            :src="currentUser.photoURL"
            class="badge-minimal"
            badge-variant="success"
          />
        </template>

        <b-dropdown-item
          link-class="d-flex align-items-center"
          @click="onLogOutClick"
        >
          <feather-icon
            size="16"
            icon="LogOutIcon"
            class="mr-50"
          />
          <span>Cerrar sesión</span>
        </b-dropdown-item>
      </b-nav-item-dropdown>
    </b-navbar-nav>
  </div>
</template>

<script>
import {
  BLink, BNavbarNav, BNavItemDropdown, BDropdownItem, BDropdownDivider, BAvatar,
} from 'bootstrap-vue'
import DarkToggler from '@core/layouts/components/app-navbar/components/DarkToggler.vue'

import { getAuth, signOut } from 'firebase/auth'

export default {
  components: {
    BLink,
    BNavbarNav,
    BNavItemDropdown,
    BDropdownItem,
    BDropdownDivider,
    BAvatar,

    // Navbar Components
    DarkToggler,
  },
  props: {
    toggleVerticalMenuActive: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      currentUser: null,
    }
  },
  mounted() {
    const auth = getAuth()
    this.currentUser =  auth.currentUser
  },
  methods: {
    async onLogOutClick() {
      const auth = getAuth()
      try {
        await signOut(auth)
        this.$router.push('/login')
      } catch (err) {
        this.showNotification('Error al cerrar sesión',
          'Por favor, vuelva a intentarlo',
          'AlertCircleIcon',
          'danger')
      }
    },
    showNotification(sTitle, sText, sIcon, sVariant) {
      this.$toast({
        component: ToastificationContent,
        props: {
          title: sTitle,
          text: sText,
          icon: sIcon,
          variant: sVariant,
        },
      })
    },
  },
}
</script>
