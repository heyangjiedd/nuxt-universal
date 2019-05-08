<template>
  <section class="container">
    <div>
      <a-list
        itemLayout="horizontal"
        :dataSource="posts"
      >
        <a-list-item slot="renderItem" slot-scope="item, index">
          <a-list-item-meta
            :description="item.loginName"
          >
            <a slot="title" href="https://vue.ant.design/">{{item.name}}</a>
          </a-list-item-meta>
        </a-list-item>
      </a-list>
      <a-form :form="form" @submit="handleSubmit">
        <a-form-item label="name" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }">
          <a-input v-decorator="['name',
          {rules: [{ required: true, message: 'Please input your note!' }]}]"/>
        </a-form-item>
        <a-form-item label="code" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }">
          <a-input v-decorator="['code',
          {rules: [{ required: true, message: 'Please input your note!' }]}]"/>
        </a-form-item>
        <a-form-item label="phone" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }">
          <a-input v-decorator="['phone',
          {rules: [{ required: true, message: 'Please input your note!' }]}]"/>
        </a-form-item>
        <a-form-item label="loginName" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }">
          <a-input v-decorator="['loginName',
          {rules: [{ required: true, message: 'Please input your note!' }]}]"/>
        </a-form-item>
        <a-form-item label="password" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }">
          <a-input v-decorator="['password',
          {rules: [{ required: true, message: 'Please input your note!' }]}]"/>
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
          <a-button type="primary" html-type="submit">提交</a-button>
          <a-button type="primary" @click="refresh">刷新</a-button>
          <a-button type="primary" @click="submit">submit</a-button>
        </a-form-item>
      </a-form>
    </div>
  </section>
</template>

<script>
  import Logo from '~/components/Logo.vue'
  import axios from 'axios'

  export default {
    components: {
      Logo
    },
    data() {
      return {
        form: this.$form.createForm(this),
        posts:[],
      }
    },
    async asyncData() {
      const posts = await axios.get('http://localhost:4444');
      return {posts: posts.data}
    },
    methods: {
      async refresh(){
        this.posts = []
        const posts = await axios.get('http://localhost:4444');
        this.posts = posts.data
      },
      async handleSubmit() {
        this.form.validateFields((err, values) => {
          if (!err) {
           axios.post('http://localhost:4444/add',values);
          }
        });
      },
      submit(){
        axios.post('http://localhost:4444/add',{loginName:1});
      }
    }
  }
</script>

<style>
  .container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .title {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
  }

  .subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
  }

  .links {
    padding-top: 15px;
  }
</style>
