<template>
  <div class="main-view">
    <Row :gutter="16">
      <Col span="6">
        <Card dis-hover>
          <Row :gutter="8">
            <Col span="10">
              <div style="width: 100%; max-width: 100px; text-align: center">
                <img class="img-icon" alt="icon" :src="emailHashURL" />
              </div>
            </Col>
            <Col span="14">
              <div class="infodiv">用户名: {{profile.username}}</div>
              <div class="infodiv">提交量: {{profile.total_submitted ? profile.total_submitted : '-'}}</div>
              <div class="infodiv">通过量: {{profile.total_passed ? profile.total_passed : '-'}}</div>
              <div class="infodiv">ＡＣ量: {{profile.total_accepted? profile.total_accepted : '-'}}</div>
            </Col>
          </Row>
        </Card>
      </Col>
      <Col span="18">
        <div class="exdatabk">
          <div class="blockhead">尝试过的题目</div>
          <div class="problemItemBlock">
            <div style="max-width: 60%">
              <!--
                TODO 在这里输出列表
                格式: <a href="地址">题目标号</a>
              -->
              <a href="#">P1024</a>
            </div>
            <div class="problemItemBlockComment">数据并非实时更新</div>
            <div class="cutline"></div>
          </div>
          <div class="blockhead">已通过的题目</div>
          <div class="problemItemBlock">
            <div style="max-width: 60%">
              <!--
                TODO 在这里输出列表
                格式: 同上
              -->
              <a href="#">P8192</a>
            </div>
            <div class="problemItemBlockComment">数据并非实时更新</div>
          </div>
        </div>
      </Col>
    </Row>

  </div>

</template>

<script>
import {mapGetters} from 'vuex'


export default {
  name: "UserInfo",
  methods: {

  },
  computed: {
    ...mapGetters(['emailHashURL', 'profile']),
  }
}
</script>

<style scoped>
.main-view {
  width: 95%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 10px;
}

.img-icon {
  border-radius: 50%;
  width: 100%;
}

.infodiv {
  padding-top: 0.2em;
  padding-bottom: 0.2em;
}

.exdatabk {
  border: 1px solid #e8eaec;
  border-radius: 4px;
  padding: 5px 5px;
}

.blockhead {
  margin-top: 1em;
  margin-left: 0.5em;
  margin-bottom: 1em;
  font-weight: 700;
  font-size: 1.1em;
}

.problemItemBlock {
  margin-left: 1.5em;
  margin-right: 1.5em;
  margin-top: 1em;
  line-height: 1.5em;

  word-wrap: break-word;
  word-break: normal;
}

  .problemItemBlock a {
    color: #2277ff;
    padding-right: 2em;
    white-space: nowarp;
    display: inline-block;
  }

  .problemItemBlock a:hover {
    color: #44aaff;
  }

.problemItemBlockComment {
  color: #aaaaaa;
  font-size: 0.8em;
  margin-top: 1em;
  margin-bottom: 1em;
}

.cutline {
  width: 100%;
  height: 2px;
  margin-top: 10px;
  margin-bottom: 10px;
  background-color: #e8eaec;
}
</style>
