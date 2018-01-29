<style scoped>
   .layout{
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
    border-radius: 4px;
    overflow: hidden;
  }
  .layout-logo{
    width: 100px;
    height: 30px;
    background: #5b6270;
    border-radius: 3px;
    float: left;
    position: relative;
    top: 15px;
    left: 20px;
  }
  .layout-nav{
    width: 420px;
    margin: 0 auto;
    margin-right: 20px;
  }
  .layout-footer-center{
    text-align: center;
  }

  .search {
    margin-top: 10px;
    margin-bottom: 10px;
  }
  .add {
    margin-bottom: 10px;
  }
</style>
<template>
  <div class="layout">
    <Layout>
      <Header>
        <Menu mode="horizontal" theme="dark" active-name="1">
          <div class="layout-logo">
            <p>
              微博关系网
            </p>
          </div>
          <div class="layout-nav">
            <MenuItem name="1">
              <Icon type="ios-navigate"></Icon>
              首页
            </MenuItem>
            <MenuItem name="2">
              <Icon type="ios-keypad"></Icon>
              任务队列
            </MenuItem>
            <MenuItem name="3">
              <Icon type="ios-analytics"></Icon>
              统计图表
            </MenuItem>
            <MenuItem name="4">
              <Icon type="ios-paper"></Icon>
              关于本站
            </MenuItem>
          </div>
        </Menu>
      </Header>
      <Content :style="{padding: '0 50px'}">
        <Breadcrumb :style="{margin: '20px 0'}">
          <BreadcrumbItem>Home</BreadcrumbItem>
          <BreadcrumbItem>Components</BreadcrumbItem>
          <BreadcrumbItem>Layout</BreadcrumbItem>
        </Breadcrumb>
        <Card>
          <div style="min-height: 200px;">
            <Search class="search"></Search>
            <AddTask class="add"></AddTask>
            <TaskList></TaskList>
          </div>
        </Card>
      </Content>
      <Footer class="layout-footer-center">2017-2018 &copy; cw1997
        <a href="https://github.com/cw1997" title="github repo"> https://github.com/cw1997</a>
      </Footer>
    </Layout>
  </div>
</template>

<script>
import Search from './components/index/Search'
import AddTask from './components/index/AddTask'
import TaskList from './components/index/TaskList'
// import HelloWorld from './components/HelloWorld'
export default {
  name: 'App',
  components: {
    Search,
    TaskList,
    AddTask
  }
}
</script>
