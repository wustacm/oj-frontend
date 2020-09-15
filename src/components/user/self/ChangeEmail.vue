<template>
  <div style="height: 100%; display: flex; align-items: center; ">
      <div style="width: 80%; max-width: 480px; margin: 0 auto;">
        <Form ref="formData" :label-width="120" :rules="formRule" :model="formData">
          <FormItem label="E-Mail" prop="newEMail">
            <Input type="email" placeholder="user@host.com" v-model="formData.newEMail" size="large"/>
          </FormItem>
          <FormItem label="验证码" prop="captcha">
            <Row>
              <Col span="14">
                <Input type="text" v-model="formData.captcha" size="large">
                  <Icon type="md-finger-print" slot="prepend"></Icon>
                </Input>
              </Col>
              <Col span="9" offset="1">
                <img alt="captcha" :src="this.captcha_url" @click="randomCaptcha">
              </Col>
            </Row>
          </FormItem>
          <FormItem>
            <Button type="primary" @click="changeEMail('formData')">确认修改</Button>
          </FormItem>
        </Form>
      </div>
  </div>
</template>

<script>
import {mapActions, mapGetters} from 'vuex'
export default {
  name: "ChangeEmail",
  data() {
    return {
      formData: {
        newEMail: '',
        captcha: ''
      },
      formRule: {
        newEMail: [
          {
            required: true,
            message: '邮箱不能为空', 
            trigger: 'blur'
          },
          {
            type: 'email', 
            message: '邮箱格式不正确', 
            trigger: 'blur'
          }
        ],
        captcha: [
          {
            required: true,
            type: 'string',
            trigger: 'blur',
            message: '需要填写验证码'
          }
        ]
      }
    }
  },
  mounted() {
  },
  methods: {
    ...mapActions(['getProfile', 'randomCaptcha']),
    changeEMail(name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          4;
        } else {
          this.$Message.error('表单中部分字段不满足条件');
          this.randomCaptcha();
        }
      })
    }
  },
  computed: {
    ...mapGetters(['captcha_url'])
  }
}
</script>

<style scoped>

</style>
