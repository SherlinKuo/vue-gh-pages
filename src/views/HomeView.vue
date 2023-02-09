<!-- eslint-disable vuejs-accessibility/label-has-for -->
<!-- eslint-disable max-len -->
<template>
  <div class="container-product">
    <div class="col py-3">
      <div class="col-md-12">
        <div class=" row ">
          <h2 class="col-md-2">產品列表</h2>
          <button type="button" class="btn btn-primary offset-md-8 col-md-2" data-bs-toggle="modal"
            data-bs-target="#productDetailModal" data-bs-title="新增產品">新增產品</button>

        </div>

        <table class="table table-hover mt-4">
          <thead>
            <tr>
              <th v-for="(item, index) in thSetting" :key="index" v-bind:width="item.width">
                {{ item.name }}</th>

            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in products" :key="index">
              <td>{{ item.title }}</td>
              <td>
                {{ item.origin_price }}
              </td>
              <td>
                {{ item.price }}
              </td>
              <td>
                <span class="text-success" v-if="item.is_enabled == 1">啟用</span>
                <span v-if="item.is_enabled != 1">未啟用</span>
              </td>
              <td>
                <button type="button" class="btn btn-primary" v-on:click="data = item"
                  v-bind:disabled="item.is_enabled != 1">查看細節</button>
              </td>
              <td class="action-td">
                <button type="button" class="btn btn-outline-primary" v-on:click="data = item"
                  v-bind:disabled="item.is_enabled != 1" data-bs-toggle="modal" data-bs-target="#productDetailModal"
                  data-bs-title="編輯產品">編輯</button>
                <button type="button" class="btn btn-outline-warning" v-on:click="data = item"
                  v-bind:disabled="item.is_enabled != 1">刪除</button>
              </td>
            </tr>
          </tbody>
        </table>
        <p>目前有 <span>{{ products.length }}</span> 項產品</p>
      </div>
      <div class="col-md-12">
        <h2>單一產品細節</h2>

        <div v-if="data" class="row">
          <div class="card mb-3 col-md-8 py-3">
            <img class="card-img-top primary-image" v-bind:src="data.imageUrl" v-bind:alt="data.title">
            <div class="card-body">
              <h5 class="card-title">
                {{ data.title }}
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

  <div class="modal fade" id="productDetailModal" tabindex="-1" aria-labelledby="productDetailModal" aria-hidden="true">
    <div class="modal-dialog modal-xl">
      <div class="modal-content">
        <div class="modal-header bg-primary text-white">
          <h1 class="modal-title fs-5" id="productDetailModalLabel">---</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body row ">
          <div class="col-md-4">
            <div class="form-floating">
              <input type="text" class="form-control" id="imageUrl" placeholder="請輸入圖片連結" required>
              <label for="imageUrl">主要圖片</label>
            </div>
            <button type="button" class="btn btn-outline-primary">新增圖片</button>
          </div>
          <div class="col-md-8 g-5">
            <div class="form-floating">
              <input type="text" class="form-control" id="title" placeholder="請輸入標題" required>
              <label for="title">標題</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="category" placeholder="請輸入分類" required>
              <label for="category">分類</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="Unit" placeholder="請輸入單位" required>
              <label for="Unit">單位</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="origin_price" placeholder="請輸入原價" required>
              <label for="origin_price">原價</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="price" placeholder="請輸入售價" required>
              <label for="price">售價</label>
            </div>
            <div class="form-floating">
              <textarea class="form-control" placeholder="請輸入產品描述" id="description"></textarea>
              <label for="description">產品描述</label>
            </div>
            <div class="form-floating">
              <textarea class="form-control" placeholder="請輸入說明內容" id="content"></textarea>
              <label for="content">說明內容</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="is_enabled">
              <label class="form-check-label" for="is_enabled">
                是否啟用
              </label>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary">Save changes</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomeView',
  data() { // data 是個 function
    return {
      products: [],
      data: undefined,
      url: 'https://vue3-course-api.hexschool.io/v2', // 請加入站點
      path: 'sesame-store', // 請加入個人 API Path
      thSetting: [
        {
          name: '產品名稱',
          width: 150,
        },
        {
          name: '原價',
          width: 120,
        },
        {
          name: '售價',
          width: 120,
        },
        {
          name: '是否啟用',
          width: 150,
        },
        {
          name: '查看細節',
          width: 120,
        },
        {
          name: '',
          width: 200,
        },
      ],

    };
  },
  methods: {
    getData() {
      // console.log(token);
      // header 夾帶 token

      axios.get(`${this.url}/api/${this.path}/admin/products`)
        .then((res) => {
          if (res.data.products.length === 0) {
            this.alert('無商品');
            this.$router.push({ path: '/', replace: true });
          }
          // console.log(res);
          this.products = res.data.products;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    checkLogin() {
      // eslint-disable-next-line
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*\=\s*([^;]*).*$)|^.*$/, '$1');
      axios.defaults.headers.common.Authorization = token;
      // https://vue3-course-api.hexschool.io/v2/api/user/check
      axios.post(`${this.url}/api/user/check`)
        .then(() => {
          this.getData();
        })
        .catch(() => {
          this.alert('未登入');
          this.$router.push({ path: '/', replace: true });
        });
    },

    listenModal() {
      const productDetailModal = document.getElementById('productDetailModal');
      // eslint-disable-next-line func-names, no-undef, arrow-parens
      productDetailModal.addEventListener('show.bs.modal', event => {
        const button = event.relatedTarget; // Button that triggered the modal
        const recipient = button.getAttribute('data-bs-title');
        console.log(recipient);
        const modalTitle = productDetailModal.querySelector('.modal-title');
        modalTitle.textContent = `${recipient}`;
      });
    },
  },
  mounted() {
    this.checkLogin();
    this.listenModal();
  },
};
</script>

<style>
.container-product {
  height: 100%;
  width: 80%;
  margin: auto;
}

img {
  object-fit: contain;
  max-width: 100%;
}

.primary-image {
  height: 300px;
}

.images {
  height: 150px;
}
</style>
