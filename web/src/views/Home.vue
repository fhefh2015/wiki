<template>
  <a-layout>
    <a-layout-sider width="200" style="background: #fff">
      <a-menu
          mode="inline"
          :style="{ height: '100%', borderRight: 0 }"
      >
        <a-sub-menu key="sub1">
          <template #title>
              <span>
                <user-outlined />
                subnav 1
              </span>
          </template>
          <a-menu-item key="1">option1</a-menu-item>
          <a-menu-item key="2">option2</a-menu-item>
          <a-menu-item key="3">option3</a-menu-item>
          <a-menu-item key="4">option4</a-menu-item>
        </a-sub-menu>
        <a-sub-menu key="sub2">
          <template #title>
              <span>
                <laptop-outlined />
                subnav 2
              </span>
          </template>
          <a-menu-item key="5">option5</a-menu-item>
          <a-menu-item key="6">option6</a-menu-item>
          <a-menu-item key="7">option7</a-menu-item>
          <a-menu-item key="8">option8</a-menu-item>
        </a-sub-menu>
        <a-sub-menu key="sub3">
          <template #title>
              <span>
                <notification-outlined />
                subnav 3
              </span>
          </template>
          <a-menu-item key="9">option9</a-menu-item>
          <a-menu-item key="10">option10</a-menu-item>
          <a-menu-item key="11">option11</a-menu-item>
          <a-menu-item key="12">option12</a-menu-item>
        </a-sub-menu>
      </a-menu>
    </a-layout-sider>
    <a-layout-content
        :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }"
    >
      <a-list item-layout="vertical" size="large" :grid="{ gutter: 20, column: 3 }" :data-source="ebooks">
        <template #footer>
          <div>
            <b>Roc design vue</b>
            footer part
          </div>
        </template>
        <template #renderItem="{ item }">
          <a-list-item key="item.name">
            <template #actions> <!--这里是三个小图标-->
              <span v-for="{ type, text } in actions" :key="type">
                <component :is="type" style="margin-right: 8px" />
                {{ text }}
              </span>
            </template>

            <a-list-item-meta :description="item.description">
              <template #title>
                <a :href="item.href">{{ item.name }}</a>
              </template>
              <template #avatar><a-avatar :src="item.cover" /></template>
            </a-list-item-meta>
          </a-list-item>
        </template>
      </a-list>

    </a-layout-content>
  </a-layout>
</template>

<script>
// @ is an alias to /src
import {onMounted, ref} from "vue";
import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'
import { message } from 'ant-design-vue';



export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  setup() {
    console.log('setup')
    const ebooks = ref()
    // 在这里写函数会在页面渲染完之后再执行。可能会拿到数据比较晚会出问题。比如操作数据会出错，因为数据还没有拿到。
    onMounted(() => {
      axios.get("/ebook/list", {
        params: {
          page: 1,
          size: 100
        }
      }).then((response) => {
        const data = response.data
        if (data.success) {
          ebooks.value = response.data.content.list
        } else {
          message.error(data.message)
        }
      });
    })
    return {
      ebooks,
      pagination: {
        onChange: page => {
          console.log(page)
        },
        pageSize: 3
      },
      actions: [
        {type: 'StarOutlined', text: '156'},
        {type: 'LikeOutlined', text: '156' },
        {type: 'MessageOutlined', text: '2' },
      ]
    }
  }
}
</script>

<style scoped lang="less">
  .ant-avatar {
    width: 50px;
    height: 50px;
    line-height: 50px;
    border-radius: 8%;
    margin: 5px 0;
  }
</style>