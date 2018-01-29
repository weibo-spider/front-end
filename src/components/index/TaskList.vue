<template>
  <div id="task-list">
    <div class="table">
      <Table border :columns="table.columns" :data="table.tasks"></Table>
    </div>
    <div class="page">
      <Page :total="page.total" :page-size="page.pageSize" :current="page.page" show-total show-elevator show-sizer @on-change="changePage" @on-page-size-change="changePageSize"></Page>
    </div>
    <div class="refresh">
      <Button type="success" size="large" long @click="refresh">刷新</Button>
    </div>

    <Modal
        v-model="deleteDialog.show"
        title="删除抓取任务">
        <p>
          如果要删除，请发送您的微博登录账户成功后的截图至 867597730@qq.com
        </p>
    </Modal>
  </div>
</template>

<style scoped>
  .table {
    margin-bottom: 20px;
  }
  .page {
    width: 600px;
    margin: 20px auto;
  }
</style>

<script>
  export default {
    name: 'TaskList',
    data () {
      return {
        table: {
          columns: [
          {
            title: 'Uid',
            key: 'uid',
            render: (h, params) => {
              return h('strong', ' ' + params.row.uid)
            }
          },
          {
            title: '用户名',
            key: 'name',
            render: (h, params) => {
              return h('div', [
                h('Icon', {
                  props: {
                    type: 'person'
                  }
                }),
                h('strong', ' ' + params.row.name)
                ])
            }
          },
          {
            title: '抓取页数',
            key: 'page_limit'
          },
          {
            title: '抓取深度',
            key: 'depth_limit'
          },
          {
            title: '任务创建时间',
            key: 'create_time'
          },
          {
            title: '任务开始时间',
            key: 'start_time'
          },
          {
            title: '任务完成时间',
            key: 'end_time'
          },
          {
            title: '任务状态',
            key: 'status'
          },
          {
            title: '操作',
            key: 'action',
            width: 150,
            align: 'center',
            render: (h, params) => {
              return h('div', [
                h('Button', {
                  props: {
                    type: 'primary',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.show(params.index)
                    }
                  }
                }, '查看'),
                h('Button', {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      this.remove(params.index)
                    }
                  }
                }, '删除')
                ]);
            }
          }
          ],
          tasks: [],
        },
        page: {
          total: 0,
          pageSize: 10,
          page: 1,
          last: -1,
        },
        deleteDialog: {
          show: false
          // loading: true
        },
      }
  },
  methods: {
    /*showError(response) {
      let errorNo = response.data.errorno
      if (errorNo) {
        let errorMsg = response.data.errormsg
        this.$Message.error({
          content: errorMsg,
          duration: 3,
        })
      }
    },*/
    remove(index) {
      this.deleteDialog.show = true
      // setTimeout(() => {
      //     this.deleteDialog.show = false
      // }, 2000)
    },
    fetchTasks(pageNum=1) {
      let vue = this
      console.log('fetchTasks')
      // TODO:fetchTasks
      this.$http.get('http://127.0.0.1:8888/api/v1/task/list', {
        params: {
          page_size: vue.page.pageSize,
          page: pageNum,
          last: vue.page.last,
        }
      })
      .then(function (response) {
        console.log(response)
        vue.page.page = pageNum
        vue.$helper.showErrorMsg(vue, response)
        vue.table.tasks = response.data.data.tasks
        vue.page.total = response.data.data.count
      })
      .catch(function (response) {
        console.log(response)
        vue.$helper.showServerError(vue, response)
      })
    },
    refresh() {
      this.fetchTasks()
    },
    changePage(newPage) {
      console.log(newPage)
      this.fetchTasks(newPage)
    },
    changePageSize(newSize) {
      console.log(newSize)
      this.page.pageSize = newSize
      this.fetchTasks(this.page.newPage)
    },
  },
  mounted() {
      this.fetchTasks()
  },
}
</script>
