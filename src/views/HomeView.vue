<!-- eslint-disable vuejs-accessibility/label-has-for -->
<!-- eslint-disable max-len -->
<template>
  <div class="container-product">
    <div class="col py-3">
      <div class="col-md-12">
        <div class=" row ">
          <h2 class="col-md-2">產品列表</h2>
          <button type="button" class="btn btn-primary offset-md-8 col-md-2" data-bs-toggle="modal" @click="addPro()"
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
                <button type="button" class="btn btn-primary" v-on:click="showProDetail(item)"
                  v-bind:disabled="item.is_enabled != 1">查看細節</button>
              </td>
              <td class="action-td">
                <button type="button" class="btn btn-outline-primary" v-on:click="editPro(item)"
                  v-bind:disabled="item.is_enabled != 1" data-bs-toggle="modal" data-bs-target="#productDetailModal"
                  data-bs-title="編輯產品">編輯</button>
                <button type="button" class="btn btn-outline-warning" v-on:click="selectPro = item"
                  v-bind:disabled="item.is_enabled != 1">刪除</button>
              </td>
            </tr>
          </tbody>
        </table>
        <p>目前有 <span>{{ products.length }}</span> 項產品</p>
      </div>
      <div class="col-md-12" v-if="selectPro">
        <h2>單一產品細節</h2>

        <div class="row">
          <div class="card mb-3 col-md-8 py-3">
            <img class="card-img-top primary-image" v-bind:src="selectPro.imageUrl" v-bind:alt="selectPro.title">
            <div class="card-body">
              <h5 class="card-title">
                {{ selectPro.title }}
                <span class="badge bg-primary ms-2">{{ selectPro.category }}</span>
              </h5>
              <p class="card-text">商品描述：{{ selectPro.description }}</p>
              <!--                 <p class="card-text">商品內容：</p> -->
              <p class="card-text">{{ selectPro.content }}</p>
              <div class="d-flex">
                <p class="card-text me-2">{{ selectPro.price }}</p>
                <p class="card-text text-secondary"><del>{{ selectPro.origin_price }}</del></p>
                元 / {{ selectPro.unit }}
              </div>
            </div>
          </div>
          <div class="col-md-4">
            <template v-for="(url, index) in selectPro.imagesUrl" :key="index">
              <img v-bind:src="url" v-bind:alt="url" class="images m-2">
            </template>
          </div>

        </div>

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
        <div class="modal-body row justify-content-around">
          <div class="col-md-5">
            <div class="form-floating">
              <input type="text" class="form-control" id="imageUrl" v-model="tmpPro.imageUrl" placeholder="請輸入圖片連結"
                required>
              <label for="imageUrl">主要圖片網址</label>
              <img v-bind:src="tmpPro.imageUrl" v-bind:alt="tmpPro.imageUrl" class="images m-2">
            </div>
            <h5> 詳細圖片 </h5>
            <div class="form-floating" v-for="(iurl, index) in tmpPro.imagesUrl" :key="index">

              <input type="text" class="form-control" v-model="tmpPro.imagesUrl[index]" :id="`imagesurl-${index}`"
                placeholder="請輸入圖片連結" required>

              <label :for="`imagesurl-${index}`">圖片網址</label>
              <img v-bind:src="tmpPro.imagesUrl[index]" v-bind:alt="tmpPro.imagesUrl[index]" class="images m-2">
            </div>
            <button type="button" class="btn btn-outline-primary" @click="addImgsUrl()">新增圖片</button>
          </div>

          <div class="col-md-6">
            <div class="form-floating">
              <input type="text" class="form-control" id="title" placeholder="請輸入標題" v-model="tmpPro.title" required>
              <label for="title">標題</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="category" placeholder="請輸入分類" v-model="tmpPro.category"
                required>
              <label for="category">分類</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="Unit" placeholder="請輸入單位" v-model="tmpPro.unit" required>
              <label for="Unit">單位</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="origin_price" placeholder="請輸入原價"
                v-model="tmpPro.origin_price" required>
              <label for="origin_price">原價</label>
            </div>
            <div class="form-floating">
              <input type="text" class="form-control" id="price" placeholder="請輸入售價" v-model="tmpPro.price" required>
              <label for="price">售價</label>
            </div>
            <div class="form-floating">
              <textarea class="form-control" placeholder="請輸入產品描述" id="description"
                v-model="tmpPro.description"></textarea>
              <label for="description">產品描述</label>
            </div>
            <div class="form-floating">
              <textarea class="form-control" placeholder="請輸入說明內容" id="content" v-model="tmpPro.content"></textarea>
              <label for="content">說明內容</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" id="is_enabled" v-model="tmpPro.is_enabled"
                :true-value="1" :false-value="0">
              <label class="form-check-label" for="is_enabled">
                是否啟用
              </label>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
          <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="savePro()">儲存</button>
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
      selectPro: undefined,
      tmpPro: {
        imagesUrl: [],
      },
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
      productModal: undefined,
    };
  },
  methods: {
    showProDetail(item) {
      // console.log(item);
      this.selectPro = item;
    },
    addImgsUrl() {
      if (this.tmpPro.imagesUrl[this.tmpPro.imagesUrl.length - 1] !== '') {
        this.tmpPro.imagesUrl.push('');
      }
    },
    editPro(item) {
      this.tmpPro = item;
    },
    addPro() {
      this.tmpPro = {
        imagesUrl: [],
      };
    },
    savePro() {
      // console.log(this.tmpPro);
      // console.log(document.getElementById('productDetailModal').hide());
      // document.getElementById('productDetailModal').modal('hide');
      console.log(this.tmpPro);
      if (this.tmpPro.id) {
        axios.put(`${this.url}/api/${this.path}/admin/product/${this.tmpPro.id}`, { data: this.tmpPro }).then(() => {
          this.$router.go(0);
        });
      } else {
        axios.post(`${this.url}/api/${this.path}/admin/product`, { data: this.tmpPro }).then(() => {
          this.$router.go(0);
        });
      }
    },
    getData() {
      // console.log(token);
      // header 夾帶 token

      axios.get(`${this.url}/api/${this.path}/admin/products`)
        .then((res) => {
          if (res.data.products.length === 0) {
            this.alert('無商品');
            this.$router.push({ path: '/', replace: true });
          }
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

.form-floating {
  margin-bottom: 8px;
}
</style>
