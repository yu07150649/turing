<template>
  <div class='hello'>
    <p>
      图灵，请提问:
      <input v-model='question'/>
    </p>
    <p>
      answer:{{ answer }}
    </p>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: 'hello',
  data () {
    return {
      question: '',
      answer: '有问题才有答案'
    }
  },
  watch: {
    question: function (newQuestion) {
      this.answer = '等待输入提问'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(function () {
      var vm = this
      if (this.question.indexOf('?') === -1) {
        vm.answer = '问题请用?结束'
        return
      }
      vm.answer = '思考中'
      let baseUrl = 'api/robot/index?'
      let info = encodeURI(this.question)
      let userId = '13268125661'
      let appKey = 'a2166353dd8efdb5028be11fa4a6c0d9'
      let targetUrl = baseUrl + 'info=' + info + '&userid=' + userId + '&key=' + appKey
      this.$http.get(targetUrl).then((response) => {
        console.log(response)
        vm.answer = response.data.result.text
      }).catch(function (error) {
        vm.answer = '出错了' + error
      }, 1000)
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
