<template>
  <div class="view-login">
    <div class="container">
      <div class="row">
        <div class="col-md-4 col-md-offset-4">
           <div class="panel panel-default">
            <div class="panel-body">
              <form class="form form-login" @submit.prevent="login(account)">
                <div class="form-group" :class="{'has-error': errors.has('email')}">
                  <label for="">email</label>
                  <input type="text" name="email" class="form-control" placeholder="email" v-model="account.email" v-validate data-rules="required|email" />
                  <p class="help-block">{{errors.first('email')}}</p>
                </div>
                <div class="form-group" :class="{'has-error': errors.has('password')}">
                  <label for="">password</label>
                  <input type="password" class="form-control" name="password" placeholder="password" v-model="account.password" v-validate data-rules="required" />
                  <p class="help-block">{{errors.first('password')}}</p>
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary btn-block">login</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div> 
  </div>
</template>

<script>
import {mapActions} from 'vuex'

export default {
  data () {
    return {
      account: {}
    }
  },
  computed: {
    redirect () {
      return this.$route.query.redirect || '/'
    }
  },
  methods: {
    login () {
      this.$validator.validateAll()
      if (this.errors.any()) return
      this.callLogin(this.account)
      .then((response) => {
        return this.getAccount({id: 'me'})
      })
      .then((response) => {
        this.$router.push(this.redirect)
      })
      .catch(() => {})
    },
    ...mapActions({
      'callLogin': 'login',
      'getAccount': 'getAccount'
    })
  }
}
</script>

<style></style>