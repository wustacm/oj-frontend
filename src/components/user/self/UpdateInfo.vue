<template>
<div style="height: 100%; display: flex; align-items: center; ">
    <div style="width: 80%; max-width: 480px; margin: 0 auto;">
      <Form ref="formData" :label-width="120" :rules="formRule" :model="formData">
        <FormItem label="学校" prop="schoolName">
          <Select v-model="formData.schoolName">
            <Option :key="item" :value="item" v-for="item in formData.schoolNameList">
              {{ item }}
            </Option>
          </Select>
        </FormItem>
        <FormItem label="ID" prop="studentID">
          <Input type="text" v-model="formData.studentID" />
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
          <Button type="primary" @click="changeInfo('formData')">确认修改</Button>
        </FormItem>
      </Form>
    </div>
  </div>
</template>

<script>
import {mapActions, mapGetters} from 'vuex'
export default {
  name: "UpdateInfo",
  data() {
    return {
      formData: {
        captcha: '',
        studentID: '',
        schoolName: 'Inviald',
        schoolNameList: ['Inviald'],
      },
      formRule: {
        studentID: [
          {
            required: true, 
            message: '新密码不能为空', 
            trigger: 'blur'
          },
          {
            pattern: /^[a-zA-Z0-9]{1,40}$/,
            message: '用户名只能包含字母和数字且不超过40字符',
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
    this.formData.schoolNameList = [
      'load', '456'
    ];
    this.formData.schoolName = '456';
  },
  methods: {
    ...mapActions(['getProfile', 'randomCaptcha']),
    changeInfo(name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          // TODO
          null;
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
