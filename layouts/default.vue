<template>
  <div>
    <a-menu v-model="current" mode="horizontal">
      <a-menu-item key="mail"> Ant Design Vue</a-menu-item>
      <a-sub-menu>
        <span slot="title" class="submenu-title-wrapper"
          ><a-icon type="setting" />Menu</span
        >
        <a-menu-item-group title="Opções">
          <a-menu-item key="setting:1">
            Profile
          </a-menu-item>
          <a-menu-item @click="logout()" key="setting:2">
            Logout
          </a-menu-item>
        </a-menu-item-group>
      </a-sub-menu>
    </a-menu>
    <nuxt />
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: ['home'],
      user: this.$auth.user
    }
  },
  methods:{
    async logout(){
      console.log('logout')
      await this.$auth.logout(/* .... */)
      await document.location.reload(true);
    }
  }
}
</script>
<style>
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}
</style>
