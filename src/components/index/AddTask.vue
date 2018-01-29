<style scoped>
  .add-task {
    padding: 20px;
  }
</style>

<template>
  <div id="search">
    <Button type="primary" icon="person-add" @click="show = true">添加任务</Button>
    <Modal
    title="添加抓取任务"
    v-model="show"
    :mask-closable="false"
    @on-ok="ok"
    @on-cancel="cancel">
      <p>在此处添加抓取任务之后，等待服务器执行完抓取任务之后即可查看该用户的微博关系网图。</p>
      <Form :model="formItem" label-position="top" class="add-task"> <!-- :label-width="80" -->
        <FormItem label="Uid（微博用户ID）">
          <Input v-model="formItem.uid" placeholder="请输入微博用户Uid..."></Input>
        </FormItem>
        <FormItem label="Username（微博用户名）">
          <Input v-model="formItem.username" placeholder="请输入微博用户名..."></Input>
        </FormItem>
        <FormItem label="抓取粉丝页面数量(每页20个，请根据实际情况确定范围，过大将会影响抓取速度)">
          <Slider v-model="formItem.pageLimit" show-stops show-input :min="1" :max="50"></Slider>
        </FormItem>
        <FormItem label="抓取深度(由于服务器配置限制，当前只允许3层深度)">
          <Slider v-model="formItem.depthLimit" show-stops show-input :min="1" :max="3"></Slider>
        </FormItem>
        <!-- <FormItem>
          <Button type="primary">Submit</Button>
          <Button type="ghost" style="margin-left: 8px">Cancel</Button>
        </FormItem> -->
      </Form>
    </Modal>
  </div>
</template>

<script>
  export default {
    name: 'AddTask',
    data () {
      return {
        show: false,
        formItem: {
          uid: '1934383474',
          username: '1644114654',
          pageLimit: 1,
          depthLimit: 1,
        },
      }
    },
    methods: {
      ok() {
        console.log('handleSubmit')
        let vue = this
        let formItem = this.formItem
        // TODO:fetchTasks
        this.$http.get('http://127.0.0.1:8888/api/v1/task/add', {
          params: {
            uid: formItem.uid,
            username: formItem.username,
            page_limit: formItem.pageLimit,
            depth_limit: formItem.depthLimit,
          }
        })
        .then(function (response) {
          console.log(response)
          vue.$helper.showErrorMsg(vue, response)
          vue.$Message.success({
            content: '添加任务成功',
            duration: 3,
          })
          vue.formItem.uid = ''
          vue.formItem.username = ''
        })
        .catch(function (response) {
          console.log(response)
          vue.$helper.showServerError(vue, response)
        })
      },
      cancel() {

      },
    }
  }
</script>
