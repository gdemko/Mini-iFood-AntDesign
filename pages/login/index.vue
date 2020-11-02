<template>
  <div class="container">
    <div>
      <Logo />
       <a-form @submit="onSubmit">
          <a-form-item
            id="input-group-1"
            label="E-mail:"
            label-for="input-1"
          >
            <a-input
              id="input-1"
              v-model="form.email"
              type="email"
              required
              :state="validation"
            ></a-input>
          </a-form-item>

          <a-form-item id="input-group-2" label="Senha:" label-for="input-2">
            <a-input
              id="input-2"
              v-model="form.password"
              type="password"
              required
            ></a-input>
          </a-form-item>
          <a-button type="primary" html-type="submit" block>Login</a-button>
       </a-form>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'login',
  data() {
      return {
        form: {
          email: '',
          password: '',
        },
        show:true,
        validation:null
      }
    },
    methods: {
      onSubmit(e) {
        e.preventDefault()
         this.$auth
        .loginWith("local", {
          data: {
            email: this.form.email,
            password: this.form.password
          }
        })
        .then((response) => {
          this.validation = true
          document.location.reload(true);
        }).catch((data) => {
          console.log(data)
          if(status == 400)
          {
            this.validation = false
          }
        })
      },
    }
}
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
