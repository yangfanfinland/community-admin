<template>
  <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="handleSubmit">
    <FormItem prop="username">
      <Input v-model="form.username" placeholder="Please fill in email">
        <span slot="prepend">
          <Icon :size="16" type="ios-person"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input type="password" v-model="form.password" placeholder="Please fill in password">
        <span slot="prepend">
          <Icon :size="14" type="md-lock"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="code">
      <Input class="imooc-input" type="text" v-model="form.code" placeholder="Please fill in verification code">
        <span slot="prepend">
          <Icon :size="14" type="md-image"></Icon>
        </span>
        <span class="imooc-code" slot="append" v-html="svg" @click="_getCode()"></span>
      </Input>
    </FormItem>
    <FormItem>
      <Button :loading="loading" @click="handleSubmit" type="primary" long>Login</Button>
    </FormItem>
  </Form>
</template>
<script>
// import axios from '@/libs/request'
import { getCode } from '@/api/login'
import uuid from 'uuid/v4'
export default {
  name: 'LoginForm',
  props: {
    userNameRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: 'Email cannot be empty', trigger: 'blur' },
          { type: 'email', message: 'Wrong email formate', trigger: 'change' }
        ]
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: 'Password cannot be empty', trigger: 'blur' },
          {
            type: 'string',
            min: 6,
            message: 'Minimum password length is 6',
            trigger: 'change'
          }
        ]
      }
    },
    codeRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: 'Verification code cannot be empty', trigger: 'blur' },
          {
            type: 'string',
            len: 4,
            message: 'Wrong verification code length',
            trigger: 'change'
          }
        ]
      }
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      svg: '',
      form: {
        username: 'yangfanfinland@gmail.com',
        password: '',
        code: '',
        sid: ''
      }
    }
  },
  computed: {
    rules () {
      return {
        username: this.userNameRules,
        password: this.passwordRules,
        code: this.codeRules
      }
    }
  },
  mounted () {
    let sid = ''
    if (localStorage.getItem('sid')) {
      sid = localStorage.getItem('sid')
    } else {
      sid = uuid()
      localStorage.setItem('sid', sid)
    }
    this.$store.commit('setSid', sid)
    this.form.sid = sid
    this._getCode()
  },
  methods: {
    handleSubmit () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$emit('on-success-valid', { ...this.form })
        }
      })
    },
    _getCode () {
      getCode(this.$store.state.sid).then((res) => {
        if (res.code === 200) {
          this.svg = res.data
        }
      })
    }
  }
}
</script>
<style lang="scss">
.imooc-input {
  .ivu-input-group-append {
    padding: 0;
  }
  .ivu-input-group-prepend {
    width: 32px;
  }
}

.imooc-code {
  display: inline-block;
  padding: 0;
  height: 28px;
  overflow: hidden;
  svg {
    width: 120px;
    position: relative;
    top: -12px;
  }
}
</style>
