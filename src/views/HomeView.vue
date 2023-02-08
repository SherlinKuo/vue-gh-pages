<!-- eslint-disable max-len -->
<template>
      <div id="app">
        <div class="container">
            <div class="col py-3">
                <div class="col-md-8">
                    <h2>產品列表</h2>
                    <table class="table table-hover mt-4">
                        <thead>
                            <tr>
                                <th width="150">產品名稱</th>
                                <th width="120">
                                    原價
                                </th>
                                <th width="120">
                                    售價
                                </th>
                                <th width="150">
                                    是否啟用
                                </th>
                                <th width="120">
                                    查看細節
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in products" :key="index">
                                <td width="150">{{item.title}}</td>
                                <td width="120">
                                    {{item.origin_price}}
                                </td>
                                <td width="120">
                                    {{item.price}}
                                </td>
                                <td width="150">
                                    <span class="text-success" v-if="item.is_enabled == 1">啟用</span>
                                    <span v-if="item.is_enabled != 1">未啟用</span>
                                </td>
                                <td width="120">
                                    <button type="button" class="btn btn-primary" v-on:click="data = item"
                                        v-bind:disabled="item.is_enabled != 1">查看細節</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    <p>目前有 <span>{{products.length}}</span> 項產品</p>
                </div>
                <div class="col-md-10">
                    <h2>單一產品細節</h2>

                    <div v-if="data" class="row">
                        <div class="card mb-3 col-md-8">
                            <img class="card-img-top primary-image" v-bind:src="data.imageUrl" v-bind:alt="data.title">
                            <div class="card-body">
                                <h5 class="card-title">
                                    {{data.title }}
                                    <span class="badge bg-primary ms-2">{{ data.category }}</span>
                                </h5>
                                <p class="card-text">商品描述：{{ data.description }}</p>
                                <!--                 <p class="card-text">商品內容：</p> -->
                                <p class="card-text">{{ data.content }}</p>
                                <div class="d-flex">
                                    <p class="card-text me-2">{{ data.price }}</p>
                                    <p class="card-text text-secondary"><del>{{ data.origin_price }}</del></p>
                                    元 / {{ data.unit }}
                                </div>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <template v-for="(url, index) in data.imagesUrl" :key="index">
                                <img v-bind:src="url" v-bind:alt="url" class="images m-2">
                            </template>
                        </div>

                    </div>

                    <p class="text-secondary" v-else>請選擇一個商品查看</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  data() { // data 是個 function
    return {
      products: [],
      data: undefined,
      url: 'https://vue3-course-api.hexschool.io/v2', // 請加入站點
      path: 'sesame-store', // 請加入個人 API Path
    };
  },
  methods: {
    getData() {
      // console.log(token);
      // header 夾帶 token

      this.axios.get(`${this.url}/api/${this.path}/admin/products`)
        .then((res) => {
          if (res.data.products.length === 0) {
            this.alert('無商品');
            window.location = 'index.html';
          }
          console.log(res);
          this.products = res.data.products;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    checkLogin() {
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
      this.axios.defaults.headers.common.Authorization = token;
      // https://vue3-course-api.hexschool.io/v2/api/user/check
      this.axios.post(`${this.url}/api/user/check`)
        .then(() => {
          this.getData();
        })
        .catch(() => {
          this.alert('未登入');
          window.location = 'index.html';
        });
    },
  },
  mounted() {
    this.checkLogin();
  },
};
</script>
